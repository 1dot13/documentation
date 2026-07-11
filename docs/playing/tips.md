# Starter tips

New to 1.13, or coming back after years of vanilla JA2? This page collects practical
advice for your first campaign: who to hire, how to fight, how to actually hit things
under the new aiming system, how to keep money coming in, and how to survive (or skip)
the infamous Drassen counterattack.

If you want a step-by-step path from installation to holding your first town instead,
follow the [guided tutorial](../getting-started/tutorial.md). For a beginner-paced
campaign opening with spoilers, see the
[walkthrough first steps](../walkthrough/first-steps.md).

## Your first hires

A balanced starting team covers a few key roles. Some example picks from the AIM
roster:

- **One or two medics** — MD, Fox, or Spider. Someone has to patch up the inevitable
  bullet wounds, and doctors keep your team in the field between fights.
- **A technician** — Vinny or Barry. A repairman keeps your weapons and armor in
  working order.
- **Several good fighters** — Buns, Grizzly, Igor, Grunty, or Meltdown. You need
  people who can shoot back.

If you prefer a quieter approach, consider mercs who:

- are **stealthy** — Mouse, Igor
- specialize in **Night Ops** — Spider, Barry

A few hiring tips:

- Hover over a mercenary's portrait to see their traits. The tooltips explain what
  each trait does.
- With `SHOW_SKILLS_IN_HIRING_PAGE` enabled in `Ja2_Options.INI`, skills and traits
  show as a tooltip on merc portraits on both the AIM and MERC hiring pages — see
  [recommended settings](../configuration/recommended-settings.md).
- Your IMP character (the custom merc you create at the start) has no upkeep cost, so
  build them to fill whatever role your hired team lacks.

## Early strategy and tactics

### In combat

- **Save AP for interrupts.** A merc who ends the turn with leftover action points has
  a greater chance to trigger an interrupt when an enemy moves into view. Don't spend
  every last point moving.
- **Use lower stances.** Crouching and prone mercs are harder to see and harder to
  hit. Stand up only when you need to move fast.
- **Rest your weapon.** When prone, or crouching near cover, a merc can rest their
  weapon, improving accuracy. Position your shooters against sandbags, low walls, and
  window sills.
- **Suppress the enemy.** Suppressive fire reduces the target's AP, so even bursts
  that miss can pin enemies down and stop them from shooting back. It is one of 1.13's
  most powerful new tools.
- **Ready your weapon to get scope bonuses.** Press ++l++ to look in a direction, and
  ++l++ again to raise your weapon. Some bonuses — like a scope's vision range bonus —
  only apply while the weapon is raised.
- **Drop your backpack before a fight.** Backpacks reduce a merc's AP. Press
  ++shift+b++ to drop backpacks for every merc in the sector, and ++ctrl+shift+f++ to
  pick them all back up afterwards (this also sorts the sector inventory). See the
  [inventory guide](features/inventory.md) for how LBE gear works.

More combat hotkeys worth learning early are on the
[hotkeys reference](hotkeys.md).

### Between fights

- **Let your mercs sleep.** Make sure everyone sleeps every so often — a campaign is a
  marathon, not a sprint.
- **Reload before moving on.** ++alt+r++ reloads the selected merc's weapons in hand;
  ++shift+r++ reloads the whole squad.
- **Train militia.** When you have captured most of a city, train militia to defend it
  while you are elsewhere. In 1.13 you can also give militia direct orders on both the
  tactical and strategic map — see the [militia guide](features/militia.md).
- **Try the support roles.** Spotters, assistant machinegunners, and radio operators
  are optional systems that can give your squad an edge — see
  [support roles](features/support-roles.md).

## Hitting things under NCTH

If you enabled the New Chance to Hit system, aiming works very differently from
vanilla — read the [NCTH guide](features/ncth.md) for how it actually calculates your
shots. Practical advice:

- **Optics make a huge difference.** Once you secure Drassen airport and Bobby Ray's
  opens up, consider ordering 2x scopes for your rifles. Reflex sights make weapons
  faster to aim, and are great on close-quarters guns.
- **Before you have optics, fight at night.** Darkness levels the playing field, and
  Night Ops mercs excel here. Press ++shift+n++ to toggle night-vision goggles (or sun
  goggles by day) for every merc in the active team.
- **Check the numbers.** Press ++f++ and click a tile to display useful information
  about it, including chance to hit, range, and lighting level.
- **Autofire is very effective** — and by extension, so is suppression. 1.13 lets you
  choose how many rounds to fire in a burst. A foregrip attachment improves autofire
  accuracy.
- **Lasers help up close.** Laser aiming modules improve chance to hit within a
  certain range, but they also make your merc more visible to enemies.
- **Match the scope to the fight.** High-magnification scopes give bigger vision
  bonuses but more tunnel vision — cover your sniper's back. See the
  [attachments guide](features/attachments.md) for what each attachment class does.

!!! tip "NCTH not for you?"
    Plenty of veterans prefer the old chance-to-hit system (OCTH) because it is more
    consistent and easier to read. It's a legitimate choice, not a beginner crutch —
    see [NCTH vs OCTH](features/ncth.md) and the
    [new game options](new-game-options.md) page.

## Economy basics

- **Mines are your income.** Capturing towns with mines (Drassen is the classic first
  target) provides the steady cash flow that pays your mercs' salaries. Note that one
  mine will eventually run out of ore — the `WHICH_MINE_SHUTS_DOWN` setting in
  `Ja2_Options.INI` controls which one, and `MINE_INCOME_PERCENTAGE` scales how much
  mines pay out.
- **Sell your loot.** Battles leave behind piles of enemy weapons. With
  `SELL_ITEMS_WITH_ALT_LMB` enabled in `Ja2_Options.INI`, you can sell items directly
  from the sector inventory screen. If you want enemies to drop everything they carry,
  look at the `DROP_ALL` setting — both are covered on the
  [recommended settings](../configuration/recommended-settings.md) page.
- **Bobby Ray's is your gun shop.** The online store becomes available once you seize
  Drassen airport. The Bobby Ray quality and quantity settings you picked on the
  [new game screen](new-game-options.md) control what it stocks. Shipments can
  occasionally be lost or stolen — the `CHANCE_OF_SHIPMENT_LOSS` and
  `STEALING_FROM_SHIPMENTS_DISABLED` settings control this.
- **Better gear comes with progress.** As your campaign advances, a "progress" value
  rises, and items of higher "coolness" appear in shops and in enemy hands. The
  *Progress Speed of Item Choices* new-game setting controls how fast this happens —
  don't expect top-tier rifles in week one.
- **Short on starting cash?** The starting money per difficulty can be changed in
  `Data-1.13\TableData\DifficultySettings.xml` — see
  [recommended settings](../configuration/recommended-settings.md).

## Surviving the Drassen counterattack

Shortly after you take Drassen, the Queen sends a massive counterattack force to
retake it. In 1.13 this attack is infamous for being very difficult to handle so early
in the campaign.

!!! tip "You can turn it off"
    In `Data-1.13\Ja2_Options.INI`, set
    `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` to `FALSE` to disable the
    counterattack entirely. This is the recommended choice for a first campaign — see
    [recommended settings](../configuration/recommended-settings.md) for this and
    other new-player tweaks.

If you want to face it anyway:

- **Build militia early.** You *can* train or buy militia before you have captured the
  entire city — save up some cash for it.
- **Position your militia deliberately.** On the strategic map, keep militia in the
  mine sector and the sector next to it, so they can be called in as reinforcements
  when the attack lands.
- **Dig in before accelerating time.** Get your mercs into cover in the tactical
  screen *before* you compress time to wait for the attack.
- **Respect your weapon ranges.** Do not expose your mercs to danger until they can
  get a good shot — let the enemy walk into your effective range, not the other way
  around.

A more detailed treatment of the battle is in the
[early game walkthrough](../walkthrough/early-game.md).

## Sources

- *Jagged Alliance 2 v1.13 – Play Guide* — previous starter documentation (r8741 era),
  sections "Keyboard shortcuts", "NIS/NAS", "Support roles", and "Starter tips"
- *Jagged Alliance 2 v1.13 – Starter Documentation* — previous starter documentation
  (r8741 era), features list, FAQs, and glossary
- *Jagged Alliance 2 v1.13 – Recommended Settings* — previous starter documentation
  (r8741 era), INI and XML tweaks tables
