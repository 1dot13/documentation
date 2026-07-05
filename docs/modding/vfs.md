# Virtual File System (VFS)

The Virtual File System (VFS) is the layer of 1.13 that decides which actual file on
disk the game gets when it asks for something like `Tilesets/0/smguns.sti`. It manages
all file handling and offers the game a single, unified view of the file system at
runtime: every file is accessible through a path *inside* the VFS, independent of where
it really lives on your hard disk. Real directories, SLF archives and **uncompressed**
7z archives can all be mapped into that view.

This page is a restructured version of *VirtualFileSystem_Setup.txt* (v1.1) by
**BirdFlu**, the authoritative VFS document from the 1.13 SVN repository. If you just
want to understand the `Data` / `Data-1.13` folder layering as a player, see the
[configuration overview](../configuration/index.md) instead; if you want to package a
mod of your own, start with [how 1.13 modding works](index.md).

## Why the VFS exists

The layout of the VFS is based on the original JA2 game data directory: the `Data`
directory is the root of the VFS. The original SLF archives (`Anims.slf`,
`BigItems.slf`, `Sounds.slf`, `Tilesets.slf` and so on) define its base directories.

Vanilla JA2 already layered these two sources:

1. The SLF archives form the **base layer**.
2. Loose files in the `Data` directory sit **on top** of that layer.

Whenever a file is accessed — say `Tilesets/0/smguns.sti` — the game looks in the
`Data` directory first, and only if the file is not found there does it look in the
corresponding archive (`tilesets.slf` in this case).

The 1.13 mod adds another layer on top: the `Data-1.13` directory, which behaves the
same way. When a file is accessed, the game looks in `Data-1.13` first, then falls back
to `Data`, then to the SLF archives.

The VFS generalizes this system. It externalizes the definition of these layers —
called **profiles** in VFS jargon — into a configuration file and allows an unlimited
number of them. It also adds support for uncompressed 7z archives. The order of the
profiles, and which real files and directories they contain, is entirely in your hands.
The price for that flexibility is that the VFS needs a correct configuration.

## Glossary

These are the core VFS terms, from the bottom of the system to the top:

| Term | Meaning |
|------|---------|
| **Location** | A real directory or a file archive (SLF or uncompressed 7z). All files inside a location are mapped into the VFS recursively — contents of subdirectories included. You choose the path inside the VFS where they appear. |
| **Profile** | A named collection of locations. Define a new profile for every mod, and keep a mod's data together: a profile can define a *profile root* directory that is prepended to its locations' paths during initialization. Leaving the profile root empty lets you stick together directories from very different places on your disk — but defining a separate profile for unrelated directories is the better solution. |
| **Profile list (stack)** | The VFS is built from a list of profiles, processed left to right: the leftmost profile is included first, the rightmost last (on top of the stack). |
| **Profile mode** | A profile is read-only by default, or read-write if declared so. You cannot write to a read-only profile. |

!!! warning "You need exactly one sensible write profile"
    Some files must be written at runtime — temporary files, savegames, logs. At least
    one profile must therefore be read-write, and it should normally be the **top
    (rightmost)** profile. Otherwise a read-only file in a higher profile could block
    access to the writable copy underneath it.

## File lookup order

During file access the profile stack is processed in **reverse inclusion order**: a
file search starts in the **rightmost** profile of the list and continues to the left
until the file is found. In other words, later profiles override earlier ones — exactly
like `Data-1.13` overriding `Data` overriding the SLF archives in the classic setup.

## The configuration file

The VFS is configured in an INI file, by default `vfs_config.ini`. You can point the
game at a different file with the `VFS_CONFIG_INI` key in the `[Ja2 Settings]` section
of `ja2.ini`; if the key is absent it defaults to `vfs_config.ini`.

!!! note "What current releases ship"
    Modern 1.13 releases from GitHub ship four ready-made configurations in the game
    folder — `vfs_config.JA2113.ini` (the default), `vfs_config.JA2Vanilla.ini`,
    `vfs_config.UB113.ini` and `vfs_config.UBVanilla.ini` — and select between them
    with commented-out `VFS_CONFIG_INI` lines in `Ja2.ini`. The shipped
    `vfs_config.JA2113.ini` is a real-world instance of exactly the profile stack
    described on this page: `PROFILES = SlfLibs, Vanilla, v113, UserProf`, with the
    writable `UserProf` profile rooted at `Profiles\UserProfile_JA2113`. (The shipped
    file also marks a few locations, such as the Intro library, with `OPTIONAL = True`
    — a key that BirdFlu's original document does not cover.)

### Main section: `[vfs_config]`

The file starts with the main section:

```ini
[vfs_config]
PROFILES = profile1, profile2, profile3
```

The value of `PROFILES` is a list of profile names. For every name in this list a
section must exist with the format `[PROFILE_$name]`, where `$name` is the value from
the list — for example `[PROFILE_profile1]`.

### Profile sections: `[PROFILE_x]`

```ini
[PROFILE_profile1]
NAME = Some Arbitrary Name (probably a mod's name)
LOCATIONS = location1, location2, location3
PROFILE_ROOT = Profiles\mod1
WRITE = TRUE
```

| Key | Required | Meaning |
|-----|----------|---------|
| `NAME` | yes | A human-readable name; it can be used inside the game to access the data of this profile. |
| `LOCATIONS` | yes | A list of location names; each needs a matching `[LOC_x]` section. |
| `PROFILE_ROOT` | no | A path prepended to all `PATH` values defined in this profile's location sections. |
| `WRITE` | no | `TRUE` makes the profile read-write. Default is `FALSE`. |

At least one profile must have the `WRITE` property, because that is where temporary
files are saved. Usually the top (rightmost) profile is the writable one.

A file can obviously only be saved into a real directory, so a writable profile is
supposed to contain a location with `TYPE = DIRECTORY`, and that directory should be
mapped as the VFS root (`MOUNT_POINT =` left empty). The simplest setup is a single
location with these properties: all written files then end up in one place instead of
being scattered over multiple unrelated directories.

### Location sections: `[LOC_x]`

A location section name is the element from the profile's `LOCATIONS` list prefixed
with `LOC_`. Every location must specify a `TYPE`, which is either `DIRECTORY` or
`LIBRARY`:

```ini
[LOC_location1]
TYPE = DIRECTORY
PATH =
MOUNT_POINT =

[LOC_location2]
TYPE = LIBRARY
PATH = profiles/libs/archive.slf
VFS_PATH = archive.slf
MOUNT_POINT =
```

**`DIRECTORY`** locations take a `PATH` (a directory) and a `MOUNT_POINT`. The `PATH`
value is appended to the profile's `PROFILE_ROOT` and decides where the files live in
the real file system. The `MOUNT_POINT` value decides where they appear in the virtual
file system:

```text
real file:   PROFILE_ROOT/PATH/local_dir/file.name
maps to:     /MOUNT_POINT/local_dir/file.name
```

(accessible in the game simply as `MOUNT_POINT/local_dir/file.name`).

**`LIBRARY`** locations (archives) work the same way at initialization, but have one
extra key: `VFS_PATH`, which is similar to `PATH` but names a file *inside the VFS*.
If `PATH` is not defined (empty), or the file it points to cannot be opened, the
`VFS_PATH` value is evaluated instead. For that to work, a location containing the
specified file must already have been processed. This makes the configuration more
complex, but it lets you integrate archives that are themselves located inside another
archive.

## Use cases

The three worked examples below build on each other, from a bare vanilla setup to a
self-contained user mod.

### Use case 1: the main game files

The main use case is setting up the base files of the game — the SLF archives as one
profile, the `Data` directory as the profile on top of it:

```ini
[vfs_config]
PROFILES = Libs, Data

[PROFILE_Libs]
NAME = Ja2 game libraries
LOCATIONS = Anims, Faces, Tilesets
; ... in practice: one location per SLF archive ...
PROFILE_ROOT = Data

[PROFILE_Data]
NAME = Ja2 game files
LOCATIONS = Files
PROFILE_ROOT = Data

[LOC_Files]
TYPE = DIRECTORY
PATH =
MOUNT_POINT =

[LOC_Anims]
TYPE = LIBRARY
PATH = anims.slf
MOUNT_POINT =

; ... one [LOC_x] section like the above for each archive ...

[LOC_Tilesets]
TYPE = LIBRARY
PATH = tilesets.slf
MOUNT_POINT =
```

This reproduces the vanilla layering: the `Libs` profile holds the game libraries, and
the `Data` profile with the `Data` directory sits on top. Because the VFS also needs a
writable profile, we add a user profile that will contain only user data (the other
profiles contain only game data):

```ini
[vfs_config]
PROFILES = Libs, Data, UserProfile

; [PROFILE_Libs] and [PROFILE_Data] as before

[PROFILE_UserProfile]
NAME = User files
LOCATIONS =
PROFILE_ROOT = Profiles\UserProfile
WRITE = true
```

If `LOCATIONS` is empty and `WRITE` is set to `TRUE`, a writable directory at
`PROFILE_ROOT` is initialized automatically.

### Use case 2: adding the 1.13 mod

Adding a mod is simple. Taking 1.13 itself as the example:

```ini
[vfs_config]
PROFILES = Libs, Data, v113, UserProfile

; [Libs], [Data] and [UserProfile] as before

[PROFILE_v113]
NAME = v1.13
LOCATIONS = datav113_dir
PROFILE_ROOT =

[LOC_datav113_dir]
TYPE = DIRECTORY
PATH = Data-1.13
MOUNT_POINT =
```

The new profile for the 1.13 mod is inserted into the profile list *after* the `Data`
profile but *before* `UserProfile`. Whenever a file is accessed, it is taken from the
`Data-1.13` directory if it exists there; if not, the game searches the profiles below
it (`Data`, then `Libs`).

### Use case 3: user mods with their own write profile

Instead of only adding game data, a mod can also add or replace the writable user
profile. That way you can try out a new mod (one that uses the VFS system) without
risking overwriting your savegames and other temporary files:

```ini
[vfs_config]
PROFILES = Libs, Data, v113, ExperimentalMod, UserProfile_exp

; [Libs], [Data] and [v113] as before

[PROFILE_ExperimentalMod]
NAME = Files for experimental Mod
LOCATIONS = ExpMod_Libs, ExpMod_Files
PROFILE_ROOT = Profiles/ExpMod

[LOC_ExpMod_Libs]
TYPE = LIBRARY
PATH = exp_mod.7z
MOUNT_POINT = Tabledata

[LOC_ExpMod_Files]
TYPE = DIRECTORY
PATH = ModFiles
MOUNT_POINT = Interface

[PROFILE_UserProfile_exp]
NAME = User file for ExpMod
LOCATIONS =
PROFILE_ROOT = Profiles\User_ExpMod
WRITE = true
```

!!! note
    The user profile directory has to exist, even if it is empty.

Because the configuration filename is defined in `ja2.ini`, you can keep several VFS
configuration files side by side and switch between them easily:

```ini
[Ja2 Settings]
;VFS_CONFIG_INI = vfs_config.main.ini
;VFS_CONFIG_INI = vfs_config.my_mod.ini
VFS_CONFIG_INI = vfs_config.ExpMod.ini
```

This is exactly how current releases let you switch between JA2 1.13, vanilla JA2, and
the Unfinished Business variants — one shipped `vfs_config.*.ini` per game mode, with
the inactive lines commented out in `Ja2.ini`.

## The `+=` extension: composing config files

In a VFS-related INI file you can use the `+=` operator, which appends a value to a
previously set entry, essentially turning it into a list. This works because
internally a data structure (`PropertyContainer`) is used that contains a set of
mappings, each mapping two keys to a property:

```text
[key1][key2] = property
```

When initialized from an INI file, `key1` is a section, `key2` a key, and `property`
the value of that section–key pair. The same structure can also be initialized from
*multiple* INI files, where `+=` appends a new value to an already defined entry.

The VFS uses this so that `VFS_CONFIG_INI` in `ja2.ini` accepts a **list** of
configuration files. Since a single value is just a one-element list, the extension
fits into the system without requiring changes to existing configuration files.

The practical application is the stand-alone configuration of mods: each mod brings
its own configuration file, which is merged with the configuration files of other
mods. That makes it easy to build a valid configuration for a large number of mods —
otherwise a separate configuration file would be needed for every mod *combination*,
and even with a small number of combinable mods the total number of files would grow
very fast.

For the default combination (vanilla + 1.13) there would be two mod configuration
files and one user configuration file:

```ini
; ------------- vfs_config.ja2.ini
[vfs_config]
PROFILES = SlfLibs, Ja2Data

[PROFILE_SlfLibs]
; ...

[PROFILE_Ja2Data]
; ...

; [LOC_...] sections ...
```

```ini
; ------------- vfs_config.v113.ini
[vfs_config]
PROFILES += v113

[PROFILE_v113]
NAME = v1.13
LOCATIONS = datav113_dir
PROFILE_ROOT =

[LOC_datav113_dir]
TYPE = DIRECTORY
PATH = Data-1.13
MOUNT_POINT =
```

```ini
; ------------- vfs_config.user.ini
[vfs_config]
PROFILES += UserProf

[PROFILE_UserProf]
NAME = User Profile
LOCATIONS =
PROFILE_ROOT = Profiles\UserProfile
WRITE = true
```

The actual combination of mods is then done in `ja2.ini`. This is the vanilla game:

```ini
[Ja2 Settings]
VFS_CONFIG_INI = vfs_config.ja2.ini, vfs_config.user.ini
```

and this is the setup for the 1.13 mod:

```ini
[Ja2 Settings]
VFS_CONFIG_INI = vfs_config.ja2.ini, vfs_config.v113.ini, vfs_config.user.ini
```

Other mods are added by simply inserting their configuration file into the list:

```ini
[Ja2 Settings]
VFS_CONFIG_INI = vfs_config.ja2.ini, vfs_config.v113.ini, mod1.ini, mod2.ini, vfs_config.user.ini
```

!!! tip "One user profile per mod combination"
    To avoid mixing savegames and temporary files of different mod combinations, use a
    separate user profile directory for each: define every user profile in its own
    user configuration file, and select it by putting that file at the **end** of the
    `VFS_CONFIG_INI` list.

## Credits

The VFS and the original setup guide this page is based on were written by
**BirdFlu**. The original document is
[`VirtualFileSystem_Setup.txt` (v1.1)](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/How%20it%20works/VirtualFileSystem_Setup.txt)
in the 1.13 SVN repository. For what the acronyms on this page mean elsewhere in the
community, see the [glossary](../reference/glossary.md).

## Sources

- [VirtualFileSystem_Setup.txt v1.1 by BirdFlu](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/How%20it%20works/VirtualFileSystem_Setup.txt), from the 1.13 SVN repository (primary source; all syntax, semantics and examples)
- [`vfs_config.JA2113.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/vfs_config.JA2113.ini) from the current 1dot13/gamedir repository (shipped profile stack, `OPTIONAL` key)
- [`Ja2.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Ja2.ini) from the current 1dot13/gamedir repository (`VFS_CONFIG_INI` selection between the four shipped configs)
