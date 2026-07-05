# Prisoners of war

Vanilla JA2 only ever let you shoot your way through Arulco. 1.13 adds a full
prisoners-of-war system, written by Flugente: you can handcuff downed enemies, demand
that outnumbered squads surrender, ship your captives off to real prison facilities,
and then interrogate them for militia recruits, intel and ransom money. The feature
entered the SVN trunk in r5709 (December 2012) and has grown considerably since; it is
enabled by default in current GitHub releases.

## Enabling and tuning it

Everything lives in the `[Strategic Gameplay Settings]` section of `Ja2_Options.INI`,
under the `; Prisoner system` header. The master switch is `ALLOW_TAKE_PRISONERS`.
Defaults below are from the current GitHub `Data-1.13\Ja2_Options.INI`; see the
[options tour](../../configuration/options-ini.md) for how to edit safely.

| Setting | Default | What it does (from the INI comments) |
| ------- | ------- | ------------------------------------ |
| `ALLOW_TAKE_PRISONERS` | `TRUE` | "can you capture enemy soldiers?" |
| `ENEMY_CAN_SURRENDER` | `TRUE` | "can you offer enemies a surrender option?" |
| `DISPLAY_SURRENDER_VALUES` | `TRUE` | Show the surrender numbers when you demand surrender — "helpful if you wonder why the enemy does not surrender at overwhelming odds." |
| `SURRENDER_MULTIPLIER` | `5.0` | "the higher this value, the more superiority you need over the enemy for them to surrender. Range: 2.0 - 10.0" |
| `PLAYER_CAN_ASK_TO_SURRENDER` | `TRUE` | Lets you offer *your own* surrender to the enemy. |
| `PRISONER_RETURN_TO_ARMY_CHANCE` | `50` | Chance that released soldiers rejoin the Queen's army. |
| `PRISONER_DEFECT_CHANCE` | `25` | Chance that interrogated soldiers join you — "they will be turned into militia of same or lower quality". |
| `PRISONER_INTEL_CHANCE` | `25` | Chance that interrogated soldiers give you intel. |
| `PRISONER_RANSOM_CHANCE` | `25` | Chance that interrogated soldiers get you ransom money. |
| `PRISONER_INTERROGATION_POINTS_*` | 25–250 | Points needed to fully interrogate each prisoner class (see [Interrogation](#interrogation)). |

## Capturing enemies in tactical

### Handcuffs, binders and tasers

To capture an enemy you physically restrain him:

1. Put **Handcuffs** (item 1625) or a **Stack of Binders** (item 1632) in your merc's
   main hand. Both carry the `Handcuffs` item flag in `Items.xml`; mods can add more
   such items.
2. Click on the target. A special handcuff cursor shows whether a capture attempt is
   possible at all.
3. **Unconscious enemies are always captured successfully.** Conscious enemies resist,
   and the attempt may fail.

The attempt costs action points (`AP_HANDCUFF = 50` and `BP_HANDCUFF = 100` in
`APBPConstants.ini`). The default quick-item shortcuts in `Ja2_Options.INI` help here:
++alt+4++ readies handcuffs and ++alt+3++ a taser (`QUICK_ITEM_4 = -4`,
`QUICK_ITEM_3 = -3`).

The two restraint types differ:

- **Handcuffs** are reusable — you can steal them back from the prisoner — and other
  enemies *cannot* un-cuff a handcuffed comrade.
- **Binders** are a cheap consumable kit that degrades with every capture and is not
  recovered. Enemies who reach a bound comrade *can* free him (since r6248: an enemy
  who spots a captured comrade raises the alarm and tries to release him).

Since capturing works automatically on unconscious targets, less-than-lethal tools are
the natural partner of this feature — stun guns were added alongside it specifically to
make capturing enemies alive easier.

!!! warning "Handcuffing is loud"
    The AI is alerted when you handcuff someone, so quietly cuffing an entire sector
    one guard at a time will not work easily. Failed handcuff attempts also blow a
    [covert merc's](covert-ops.md) disguise (since r6241).

A handcuffed enemy is effectively out of the fight — he no longer acts in combat.
The battle ends when every remaining enemy is dying or handcuffed. You can bandage
your own prisoners if you want them alive (possible since r6028).

### Who cannot be captured

- **Profile-based NPCs** — anyone with a "talking face" portrait (Conrad, Mike, the
  Alma General and so on). Attempting it is simply blocked.
- **Tanks.** In Flugente's words, "our handcuffs are too small". As long as a tank is
  present the AI will also never surrender.
- **Non-hostile civilians.** Hostile civilians *can* be captured (since r7763) if
  their faction has `<fCanBeCaptured>1</fCanBeCaptured>` in `CivGroupNames.xml` — in
  the stock game that is set for the Kingpin, Hicks and Warden factions.

## Demanding surrender

With `ENEMY_CAN_SURRENDER = TRUE` you can skip the handcuffs and ask whole squads to
give up. Talk to any enemy soldier (the same way you would talk to an NPC); a dialog
box offers the choice to demand their surrender, offer your own, or just exchange
pleasantries. If they accept, **all remaining enemies who are not dying become
prisoners and the battle ends immediately**.

Whether they accept depends on a comparison of both sides' *surrender strength*:

- Every conscious soldier contributes a value based on experience level, strength,
  marksmanship and leadership. Unconscious, collapsed or dying soldiers contribute
  nothing; wounds and fatigue reduce the value.
- Elite soldiers count 1.5×, administrators (and green militia on your side) 0.75×.
- Your mercs count double compared to your militia.
- Enemy officers give their whole team a surrender-strength bonus
  (`ENEMY_OFFICERS_SURRENDERSTRENGTHBONUS`, 10% per lieutenant, doubled for captains),
  which is part of what makes officers hard to capture.
- Your own [covert mercs](covert-ops.md) disguised as enemy soldiers are counted on
  *their* side — spies in the sector make a surrender less likely (since r5757).

Your side's total must exceed the enemy's by a wide margin, scaled by
`SURRENDER_MULTIPLIER` (default 5.0). Turn on `DISPLAY_SURRENDER_VALUES` (default) to
see the actual numbers when you ask — very useful for judging why that last elite
refuses to drop his rifle. Enemy groups that include profile-based NPC members (Mike,
Kingpin's people, the Hicks brothers...) will never surrender — "these evil leaders
simply won't allow it", so remove the leaders first.

### Surrendering yourself

With `PLAYER_CAN_ASK_TO_SURRENDER = TRUE` (default) you can offer your own surrender
through the same talk dialog (added in r5793). It works the same way as when the enemy
demands *your* surrender — your mercs are taken captive. It is refused while certain
prison-related quests are active, or if the enemy already asked you to surrender in
this battle and you said no.

## After the battle: release or imprison

Once a battle ends with captured enemies on the map, a dialog pops up. You can:

- **Let them go.** Each released prisoner has a `PRISONER_RETURN_TO_ARMY_CHANCE`
  (default 50%) chance of rejoining the Queen's army; the rest decide they have had
  enough of the war.
- **Send them to a prison.** The dialog lists the prison sectors under your control
  (up to 7); pick one and the prisoners are transferred there. If the sector you are
  in has a prison — say you just took Tixa — you can lock them up on the spot.

Either way, the captives first drop their equipment according to the usual enemy
item-drop rules, so you lose no loot by taking prisoners.

If you control no prison at all (or just do not feel like the paperwork), prisoners
are **field-interrogated on the spot** (since r8522): the interrogation resolves
instantly, but the chances of getting anything out of it — defectors, intel, money —
are halved. Slower prison interrogation is more effective, but a lucky field
interrogation can hand you your first militia very early.

## The prisons of Arulco

Prisons are facilities (see `TableData\Map\FacilityTypes.xml`, facility types 3, 21
and 22, placed on the map in `Facilities.xml`). The stock map has six of them:

| Sector | Town | Facility | Base capacity | Interrogator slots |
| ------ | ---- | -------- | ------------- | ------------------ |
| J9 | Tixa | Prison Complex | 60 | 4 |
| I13 | Alma | Military Prison | 20 | 3 |
| N7 | near Meduna | Military Prison | 20 | 3 |
| B2 | Chitzena | Town Prison | 5 | 1 |
| G9 | Cambria | Town Prison | 5 | 1 |
| D5 | San Mona | Town Prison | 5 | 1 |

"Base capacity" is the `<usPrisonBaseLimit>` tag: the number of inmates the prison is
built for. You *can* stuff more prisoners in, but past this limit riots become possible
even when the prison is guarded. Each prison also caps how many mercs can work as
interrogators there, and its `<usPerformance>` value makes interrogators more or less
efficient (120% at Tixa's complex, 90% at a town jail). The prison shows up in the
sector info panel once you have explored the sector, with the inmate count, the
capacity and a compact per-class breakdown — the letters A/R/E/O/G stand for admins,
regulars, elites, officers and generals.

Prisoners are not just numbers: since r5896 they physically appear in the prison cells
when you visit the sector, wearing prisoner garb, and they mock you, plead for mercy or
offer bribes when talked to. They are defenseless — you took their guns, remember — and
harming them costs you town loyalty as the population learns that Arulco's so-called
liberators are as brutal as the army. Killing a prisoner of course removes him from
your prisoner pool. Modders can define their own prisons anywhere: any sector can get a
prison facility, and the exact cells used are set with prison-room entries in
`Map\SectorNames.xml`.

!!! danger "Lose the sector, lose everything"
    If the army retakes a sector where you keep prisoners, **all of them are freed,
    re-arm and join the attacking force**. 30 inmates in Tixa means the victorious
    enemy platoon just grew by 30 soldiers. Guard your prisons well.

## Guards and riots

Every hour the game checks each of your prisons for a revolt. A riot happens when:

- **nobody guards the prison**, or
- **the prison is overcrowded and the prisoners vastly outnumber the guards.** The
  higher the prisoner-to-guard ratio, the higher the riot chance.

Your "guard value" comes from the number and quality of militia in the sector plus the
stats and traits of the mercs there. A riot arms *all* prisoners in the sector — 50
inmates in Tixa means 50 fresh enemies spawning inside your prison. There is no INI
switch to turn riots off; the cure is guards and staying near the base capacity.

Two things help beyond raw numbers:

- **Backgrounds.** Mercs with a fitting background — prison guard or law enforcement
  (think Bull or Raider) — are better suited to prison duty than, say, Flo.
- **Snitches.** The prison facilities offer a `PRISON_SNITCH` assignment:
  "Masquerade as a prisoner and gain their trust to easily gather information and
  prevent mutiny attempts."

## Interrogation

Locking soldiers up is only the means; the point is getting something out of them. Put
a merc on the prison's **interrogate prisoners** facility assignment in the strategic
screen. Every hour the merc generates interrogation points — how many depends on his
stats, experience level, personality, background and even gear (in Flugente's words,
"waving a shotgun under their tongues might get some people to speak their mind more
clearly"), multiplied by the prison's performance value. The running total is shown on
the merc's portrait in the strategic screen: `145/7` means 145 points accumulated and 7
prisoners processed so far.

When the total reaches the cost for a prisoner class, one prisoner of that class is
"processed". The costs (current `Ja2_Options.INI` defaults):

| Prisoner class | Points needed |
| -------------- | ------------- |
| Civilian | 25 |
| Administrator | 30 |
| Regular trooper | 50 |
| Elite | 80 |
| Officer | 150 |
| General | 250 |

You can order an interrogator to target a specific class first (since r7327) — useful
to squeeze that one officer without chewing through a hundred admins beforehand.

A processed prisoner can produce:

- **A defector** (`PRISONER_DEFECT_CHANCE`, default 25%): he joins your cause and
  becomes militia of the same or lower quality in that sector. With
  [individual militia](militia.md) enabled these show up as the "army defector" origin,
  and with militia resources enabled, equipping them costs resources. If the
  [volunteer pool](militia.md) is on, interrogation is also one of the ways to gain
  volunteers. Captured *civilians* cannot join as militia directly — they join your
  volunteer pool instead.
- **Intel** (`PRISONER_INTEL_CHANCE`, default 25%): the prisoner talks, feeding the
  1.13 intel resource; the amount depends on the prisoner type. In older SVN builds
  this was a set of `PRISONER_INFO_*` chances that revealed enemy patrols, numbers and
  movement directions directly; current builds route it through the `[Intel Settings]`
  system (`RESOURCE_INTEL`) instead.
- **Ransom** (`PRISONER_RANSOM_CHANCE`, default 25%): his family — or is it the
  Queen? — pays for his release.

Prisoners that do not defect are eventually released, with the usual 50% chance of
turning up in the army again.

Class matters beyond the point cost. Officers are rare (they boost their team's
surrender strength, so they seldom give up) and hard to break, but with the
`ENEMY_GENERALS` feature enabled (off by default) elite and officer prisoners are your
main way to learn where the enemy's generals hide. Captured generals themselves never
defect, but they command a substantial ransom and have a high chance of betraying the
locations of their colleagues. And if you use the enemy helicopter feature: captured
downed pilots count as officers for interrogation purposes.

## Strategic considerations

- **Tixa is the prize.** With a base capacity of 60 and four interrogator slots at
  120% performance, the [Tixa prison complex](../../walkthrough/late-game.md) is by far
  the best place to run your POW operation.
- **Prisoners are a militia pipeline.** A steady flow of captives plus a good
  interrogator converts enemy troops into free(ish) militia and cash — the pacifist's
  version of farming the Drassen counterattack.
- **Budget guards from the start.** A town jail with 5 slots and a lone merc is fine
  for the odd captive; 20+ prisoners need real militia coverage or they will riot at
  the worst possible moment.
- **Demand surrender to end fights early.** Even with the default 5.0 multiplier, a
  large militia force plus your squad can talk the last few defenders into giving up —
  no more chasing that final fleeing admin across the map.

## Sources

- [New feature: Take prisoners, interrogate them](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20543)
  — Flugente's feature thread at The Bear's Pit (first post maintained with updates;
  pages 1–6 consulted, including the r5714/r5793 surrender updates, r6248 comrade
  freeing, r7179/r7327 officer & general prisoners, r7763 civilian capture and r8522
  field interrogation posts).
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  — current GitHub gamedir: `; Prisoner system` block, quick-item slots,
  `[Intel Settings]`, `[Militia Volunteer Pool Settings]`, enemy officer/general and
  enemy helicopter sections.
- [`TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml)
  and [`TableData/Map/Facilities.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/Facilities.xml)
  — prison facility definitions and sector placement.
- [`TableData/Items/Items.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Items.xml)
  — Handcuffs (1625) and Stack of Binders (1632).
- [`APBPConstants.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/APBPConstants.ini)
  — `AP_HANDCUFF` / `BP_HANDCUFF` costs.
