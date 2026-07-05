# Faces & portraits

Every character in Jagged Alliance 2 — AIM and M.E.R.C. mercenaries, NPCs,
recruitable RPCs and your IMP — has a *profile*, and every profile has a set of
face images. If you want to give a merc a new look, or add portraits for a brand
new character, you need to supply those images in the game's own STI format, in
the right sizes, in the right folders, under the right file names.

This page is based on the community "face guide" by Kazuya that ships with the
1.13 documentation, with file names, sizes and XML fields cross-checked against
the current [1.13 game directory on GitHub](https://github.com/1dot13/gamedir).

## The STI image format

JA2 stores its 2D graphics in `.sti` files (the header calls the format `STCI`).
For faces, the important properties are:

- **8-bit indexed color** — each file has its own palette of at most **256
  colors**. Reduce your artwork to 256 colors *before* converting it to STI;
  the classic conversion tools do a poor job with high-color images.
- **Multiple sub-images per file** — an animated face file contains the full
  portrait plus seven smaller sub-images: four frames for the **eyes** and
  three frames for the **mouth**. The game blits these over the portrait to
  make characters blink and talk.

You cannot edit STI files in a normal paint program. The usual workflow is:
export an existing STI to BMP with an STI tool, edit the BMP in any graphics
editor, then import it back to STI (see [Tools](#tools) below).

## The five face types

A fully equipped character has up to five face files. All of them are named
after the character's **profile slot number** (see
[numbering](#file-numbering-and-mercprofilesxml) below).

| File | Size (px) | Contents | Used for |
|---|---|---|---|
| `Data\Faces\BigFaces\<slot>.sti` | 106 × 122 | single image, no animation | mercenary overview in the laptop, AIM and M.E.R.C. web pages |
| `Data\Faces\B<slot>.sti` | 90 × 100 | face + 7 animation frames | dialogue portrait when you talk to an NPC or recruitable character in a sector |
| `Data\Faces\<slot>.sti` | 48 × 43 | face + 7 animation frames | the mercenary panel in the tactical screen and various other places |
| `Data\Faces\65Face\<slot>.sti` | 31 × 27 | face (originals also contain animation frames, but the game does not use them) | the panel shown when a battle is auto-resolved |
| `Data\Faces\33Face\<slot>.sti` | 15 × 14 | face | undocumented — the guide's author jokes that you should not even ask |

Which files a character needs depends on what kind of character it is:

- **AIM / M.E.R.C. mercenaries** need the big face (they are shown on the
  hiring web pages) and the 48 × 43 merc file, but *not* the `B`-prefixed NPC
  file — they are never talked to in a sector before hiring.
- **NPCs and in-game recruitables (RPCs)** need the `B<slot>.sti` dialogue
  portrait.
- **Everyone who can be hired or escorted** needs the 48 × 43 merc file.

The eye and mouth frame regions are cut-outs of the portrait, so their exact
pixel size varies from face to face. In the shipped files, sub-image 0 is the
full portrait, sub-images 1–4 are the eye frames and 5–7 are the mouth frames.

!!! note "Other face files"
    Any other kinds of face files you come across in the game data were put
    there for entertainment purposes only by the original JA2 developers.

## Where face files live

The folders in the table above are the classic vanilla locations under `Data`.
1.13 ships its own additional and replacement faces as loose files in the
mod's data layer, using the same structure:

```text
Data-1.13\faces\<slot>.sti          merc portraits (48 x 43)
Data-1.13\faces\B<slot>.sti         NPC dialogue portraits (90 x 100)
Data-1.13\faces\BigFaces\<slot>.sti big faces (106 x 122)
Data-1.13\faces\65Face\<slot>.sti   auto-resolve faces (31 x 27)
Data-1.13\faces\33Face\<slot>.sti   very small faces (15 x 14)
```

Because `Data-1.13` is layered over `Data` by the
[Virtual File System](vfs.md), a face file in `Data-1.13\faces` wins over the
vanilla one. For your own mod, the clean approach is to put your face files in
your own mod folder (for example `Data-MyMod\faces\...`) and let the VFS load
them over the originals, instead of overwriting files in `Data` or `Data-1.13`
— see [How 1.13 modding works](index.md) and the [VFS page](vfs.md).

There is also a separate `Data-1.13\IMPFaces` folder with the same
numbered-file structure (including `33Face`, `65Face` and `BigFaces`
subfolders) holding the portrait sets for IMP characters. Adding extra IMP
portraits is covered on the [externalization page](externalization.md).

## File numbering and MercProfiles.xml

The number in a face file name is the character's **profile slot index**. In
1.13 the profiles live in `Data-1.13\TableData\MercProfiles.xml`, where the
slot index is the `uiIndex` tag. Valid profiles are 0–254, and index 200 is
reserved and cannot be used as a profile. Each profile also carries a
`ubFaceIndex` tag that selects the face file number; in the shipped data it is
the same as the profile's `uiIndex`.

The same XML record holds the **animation coordinates** — the position of the
top-left pixel of the eye and mouth sub-images within the main portrait:

```xml
<PROFILE>
    <uiIndex>0</uiIndex>
    <zName>Barry Unger</zName>
    <zNickname>Barry</zNickname>
    <ubFaceIndex>0</ubFaceIndex>
    <usEyesX>10</usEyesX>
    <usEyesY>8</usEyesY>
    <usMouthX>7</usMouthX>
    <usMouthY>28</usMouthY>
    ...
</PROFILE>
```

!!! warning "One pixel off is visible"
    The eye and mouth coordinates must match your artwork exactly. Even a
    deviation of a single pixel makes the blinking and talking animation look
    strange.

For general advice on editing the XML files safely, see
[XML files & the XML Editor](../configuration/xml-files.md).

### The legacy way: ProEdit

Before profiles were externalized to XML, they were stored in the binary file
`Prof.dat` and edited with `PROEDIT.EXE`, which you can still find in
`Data\BinaryData`. You can use ProEdit to look up a character's slot number
and, on vanilla-style installs, to change the face coordinates. It has two
known limitations: you cannot change the coordinates for IMPs, and you cannot
change the mercenary-portrait coordinates for RPCs this way. On current 1.13
versions, edit `MercProfiles.xml` instead.

## Step-by-step: replacing or creating a face

1. **Find the slot number.** Look up the character's `uiIndex` in
   `Data-1.13\TableData\MercProfiles.xml` (or use ProEdit on legacy installs).
2. **Export a template.** Open the existing face file for that slot in an STI
   editor and export it to BMP. This gives you the exact canvas sizes and the
   layout of the eye and mouth frames to work from.
3. **Paint the portrait.** Edit the BMP in any graphics program. You need
   every size the character uses: 106 × 122, 90 × 100 (NPC/RPC only), 48 × 43,
   31 × 27 and 15 × 14.
4. **Reduce the colors.** Convert each image to a 256-color palette before
   importing — the STI tools handle anything more poorly.
5. **Build the STI files.** Import the images back into STI. For the animated
   files (`<slot>.sti` and `B<slot>.sti`) also create the seven sub-images:
   four eye frames and three mouth frames, cut from your portrait.
6. **Name and place the files.** Name each file after the slot number and put
   it in the matching folder — ideally inside your own VFS mod folder rather
   than directly in `Data` or `Data-1.13`.
7. **Set the animation coordinates.** Enter the top-left pixel position of the
   eye and mouth regions as `usEyesX`/`usEyesY` and `usMouthX`/`usMouthY` in
   the character's `MercProfiles.xml` entry.
8. **Test in game.** Check the portrait everywhere it appears — laptop,
   tactical panel, dialogue — and watch the blink/talk animation for
   misaligned frames.

!!! tip "Just swapping portraits?"
    If you only want to use ready-made portraits from a community portrait
    pack, you can skip the drawing: copy the STI files into the correct
    folders, rename them to the slot number you want to replace, and fix the
    eye/mouth coordinates in `MercProfiles.xml` if they differ from the
    original face.

## Tools

The tools traditionally used for face work, as recommended by the original
guide, are:

- **STI-Edit** — opens STI files, exports to BMP and imports BMP back to STI.
- **GRV** — another STI viewer/converter.

The download site the guide pointed to (`kermi.pp.fi/JA_2/Modding_Tools/`) is
no longer reachable, so you will have to hunt for current mirrors — the
[Bear's Pit forum](../reference/links.md) is the place to ask.

The 1.13 project has a GitHub repository intended as a tool collection,
[1dot13/tools](https://github.com/1dot13/tools), but at the time of writing it
contains only a README and no published tools yet.

## Sources

- "A nice face guide" by Kazuya — the *Create New Faces* document from the
  1.13 documentation collection.
- [1dot13/gamedir](https://github.com/1dot13/gamedir) — used to verify folder
  names (`Data-1.13\faces`, `33Face`, `65Face`, `BigFaces`, `IMPFaces`), STI
  image dimensions and sub-image structure of the shipped face files,
  `PROEDIT.EXE` in `Data\BinaryData`, and the `uiIndex`, `ubFaceIndex`,
  `usEyesX`/`usEyesY`/`usMouthX`/`usMouthY` fields in
  `Data-1.13\TableData\MercProfiles.xml`.
- [1dot13/tools](https://github.com/1dot13/tools) — repository listing checked
  for available tools.
