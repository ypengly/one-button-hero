# ONE BUTTON HERO

A polished arcade game controlled entirely with **one button** — tap, hold, or double-tap
your way through spikes, gaps, enemies, projectiles, falling traps, and goofy bosses.

Single self-contained file: `one-button-hero.html`. No install, no dependencies,
no external assets — just open it in a browser.

## How to run
Double-click `one-button-hero.html` (or open it in any modern browser: Chrome, Safari,
Firefox, Edge). Works on desktop and mobile.

## Controls (one button, five meanings)
| Input | Effect |
|---|---|
| **Tap** | Jump (or Attack, if an enemy/vulnerable boss is right in front of you) |
| **Tap while falling** | Air Recovery (short window right after your jump arcs over) |
| **Hold** | Raise Shield (release cleanly for a "Perfect Shield") |
| **Double-tap** | Dash (dodge projectiles / cross gaps / outrun spikes) |
| Timing near an obstacle | "Perfect" version of the action, worth more combo/score |

Desktop: **Space** or **mouse click**. Mobile: **tap/hold anywhere on the screen**.

## Modes
- **Story** — normal run, first boss shows up early and they keep coming as your score climbs.
- **Endless** — same core loop, no end, just chase your best score.
- **Boss Rush** — back-to-back boss fights, no filler obstacles.
- **Daily Challenge** — same obstacle pattern for everyone on a given calendar day
  (seeded from today's date), with its own separate best score. Resets automatically
  at midnight.

## Progression
- 4 unlockable heroes (Hero, Ninja, Wizard, Robot), unlocked by beating score thresholds
  (0 / 150 / 400 / 800). Each has its own color, which also tints your dash trail.
- Cosmetic only — no character affects hitboxes, speed, or timing windows.
- Everything (best score, daily best, unlocked heroes, selected hero, mute setting)
  is saved to `localStorage` on your device, so progress persists between sessions.

## Combo system
Perfect Jump / Perfect Attack / Perfect Dodge / Perfect Shield / Perfect Air Recovery
all build a combo multiplier shown on screen; the higher the combo, the more score
each successful action is worth. Getting hit resets the combo to zero.

## Bosses
Three recurring mini-bosses (Sir Flopsalot, Grumpy Cloud, Doom Duck), each with its
own attack pattern (ground smash, falling-object rain, charging projectile), a
telegraph warning before every attack, a health bar, and a brief "HIT ME!" vulnerable
window after each attack where your attacks actually land. Difficulty (obstacle speed
and spawn rate) increases the longer you survive.

## Audio & visuals
All sound effects are synthesized on the fly with the Web Audio API (jump, attack,
perfect, combo, damage, boss, victory, game over, shield, dash) — nothing to load,
nothing to break. There's a mute button (top-left) if you'd rather play in silence.
Colorful cartoon-arcade look with squash-and-stretch on attacks, screen shake on
hits, particle bursts, and floating combo text.

---

## Bugs fixed in this pass
This build replaces an earlier draft that had several real, playable-experience-breaking
bugs. In case it's useful context, here's what was actually wrong and what changed:

1. **Boss fights were unbeatable.** The boss stood far off-screen from the player's
   fixed position, so the "is the player close enough to hit the boss" check never
   passed — attacks always whiffed. The boss now spawns at melee range so vulnerable
   windows are actually reachable.
2. **Air Recovery never expired.** A timing window meant to last ~0.35s after a jump
   was accidentally being reset to full every single frame, making the "must time it
   right" mechanic trivial (it was always available). It now opens once per fall and
   depletes properly, so it's a real skill window again.
3. **Jumping over spikes/enemies was needlessly strict.** The old height check
   required you to be near the very peak of your jump to count as "cleared," which
   felt unfair. Clearing now just requires being airborne, with a tighter proximity
   window rewarding a well-timed "Perfect Jump" on top.
4. **Gaps did nothing.** They scrolled past with no collision or scoring logic at
   all. Gaps now genuinely require a jump or dash to cross — walk into one and it's
   game over, like every other obstacle.
5. **Menu buttons could be blocked by the invisible tap layer.** The full-screen
   input zone used for one-button gameplay was stacked above the menu/pause/game-over
   screens, so clicks could land on the invisible layer instead of the button under
   it. Z-index ordering is fixed so overlays and their buttons always receive input.
6. **Distorted / stretched canvas on some phone screen ratios.** The game area now
   keeps a fixed 3:2 aspect ratio using `aspect-ratio` + `dvh`-aware sizing, so it
   always fits the screen without squashing or stretching, portrait or landscape.
7. **Double game-over triggers.** Multiple obstacles overlapping in the same frame
   could each independently end the run, double-counting the "new best score" save.
   `hitPlayer` now guards against firing more than once per run.
8. **Stale timers leaking into new runs.** Some boss attacks (e.g. the falling-object
   "rain") schedule follow-up obstacles a fraction of a second later with
   `setTimeout`. If you restarted mid-attack, those could land in your new run. Each
   run now has an id, and delayed spawns check it before doing anything.
9. **Daily Challenge wasn't actually daily.** It used the same random obstacle
   pattern as Endless, so it wasn't a fair "same challenge for everyone" mode. It's
   now seeded from the real calendar date (deterministic per day) with its own
   tracked best score that resets when the date rolls over.
10. **No mute option.** Added a persistent mute toggle (top-left speaker icon), since
    a purely synthesized-audio game should let you turn it off.

## Known limitations (kept simple on purpose)
- Weapons and unlockable trails beyond per-character color tinting aren't implemented —
  only heroes/costumes are (all cosmetic, none affect gameplay).
- Boss defeats share one particle-burst animation rather than a unique animation per boss.
- There's no scripted "ending" — Story and Boss Rush both scale difficulty forever
  rather than stopping after a fixed number of stages, in keeping with a
  score-chasing arcade design.

## File structure
```
one-button-hero.html   # the entire game — HTML, CSS, and JS in one file
README.md              # this file
```
