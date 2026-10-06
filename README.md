# DOG vs HUMAN — Party Game (v1.0)

A single-player prototype of the asymmetric party game: you play a mischievous
dog trying to maximize chaos while two AI-controlled humans try to finish
their chores and catch you. Runs entirely in a browser — no install, no
server, no external assets.

## How to run
1. Unzip this folder.
2. Double-click `dog-vs-human.html` (or open it in any modern browser —
   Chrome, Safari, Firefox, Edge).
3. Play fullscreen for the best experience. Works on desktop (keyboard) and
   mobile/tablet (on-screen touch controls appear automatically).

No build step, no dependencies to install, no internet connection required
(the only external resource is the Google Font "Baloo 2"; if you're offline
the game falls back to a system font automatically).

## Controls
| Action | Keyboard | Touch |
|---|---|---|
| Move | Arrow keys / WASD | D-pad (bottom-left) |
| Act (steal food / knock over / bark) | Space | "Act" button |
| Speed Burst | Shift | "Burst" button |
| Fake Sleep (become hard to notice) | X | "Sleep" button |

## Core loop
- **Chaos Meter**: earn points by stealing food, breaking vases/furniture,
  and barking near humans. Chain actions quickly to build a combo
  multiplier — "CHAOS COMBO x3!" pops up when you're on a streak.
- **Suspicion**: each human has a suspicion bar that rises when they're near
  you (and you're not hidden) or near fresh footprints. At 100% you get
  "BUSTED" — you're teleported back to a start point and lose some chaos
  points, and your combo resets.
- **Footprints**: moving leaves a fading trail that raises suspicion if a
  human walks near it. Fake Sleep also hides you from detection briefly.
- **Tasks**: humans wander to random spots in the house and slowly complete
  a task (progress bar + label). Every finished task adds to the Human Task
  Score.
- **Match length**: 120 seconds. At the end you get a score screen with:
  - Dog Chaos Score
  - Human Task Score (chores finished)
  - Most food stolen
  - Objects destroyed
  - Longest chase
  - Most suspicious moment (peak suspicion reached)

## What's included in this build
- Full menu flow: Main Menu → Settings (volume) → Setup (map + dog
  character) → Gameplay → Score Screen → Restart.
- One fully playable map: **House** (Kitchen / Living Room / Bedroom), with
  stealable food, breakable vase and chair props, and two AI humans with
  independent tasks and suspicion.
- Three dog characters with different speed profiles: Scrappy Pup (balanced),
  Lazy Hound (slow but steady), Zoomie Terrier (fast).
- Two dog abilities: Speed Burst and Fake Sleep (each on its own cooldown).
- Procedural sound effects (bark, steal, crash, busted, victory jingle) via
  the Web Audio API — no audio files needed. Volume is adjustable in
  Settings.
- Illustrated, cartoon-styled UI: gradient sky, drifting cloud shapes, paw
  print decorations, a hand-drawn-feel color palette, and a chunky rounded
  typeface (Baloo 2) with thick outlines and drop-shadow text for a playful,
  "kids' cartoon" tone.
- Fully responsive: playable on phones/tablets via on-screen touch controls,
  and the canvas resizes to fit the window.

## What's stubbed for a future pass
The Apartment, Backyard, Restaurant, Office, and School maps are visible
(but disabled) in the map-select screen as a preview of what's coming. The
remaining dog abilities (Super Bark, Invisible, Mega Jump, Food Radar) and
human tools (Vacuum, Flashlight, Food Lure, Toy, Net, Security Camera) are
designed for but not yet wired in.

## Code structure (for extending it)
Everything lives in one self-contained file, `dog-vs-human.html`, organized
into clearly separated sections so it's easy to extend:

- **Screen management** — a simple `goTo(id)` function toggles which
  `.screen` div is visible; add a new screen by adding a `<div class="screen">`
  and wiring a button to `goTo()`.
- **Sound** — `beep()` is a tiny synth helper; `sfx.*` are named presets.
  Add a new sound by adding one more `sfx.yourName = () => beep(...)`.
- **Game data** — `DOGS` (character stats) and `ROOMS` (map layout) are
  plain data objects. A new map is just a new array of room rectangles plus
  a new `objects` layout — the collision, drawing, and task logic already
  read from these generically.
- **`startGame()`** — resets all state for a match (dog, humans, objects,
  stats, timer).
- **`handleAction()`** — the dog's "Act" button logic (steal/break/bark);
  this is the natural place to add new abilities.
- **`loop()`** — the main per-frame update: dog movement, human AI,
  suspicion, footprints, particles, then `draw()`.
- **`draw()`** — all canvas rendering, kept separate from game logic so you
  can restyle visuals without touching rules.
- **`endGame()`** — computes and displays the score screen.

### Adding a new map
1. Add a new entry to the map-select buttons (remove `disabled`).
2. Define a `ROOMS`-style layout and an `objects` array for it.
3. In `startGame()`, branch on the selected map to load the right layout.

### Adding a new ability
1. Add a cooldown/timer field to the `dog` object in `startGame()`.
2. Handle the key/button in `handleAction()` or `loop()`.
3. Draw any visual feedback in `draw()`.

### Multiplayer note
This build simulates both sides with AI so the core loop can be tested
solo. Because game state (`dog`, `humans`, `objects`, `chaos`, timers) is
already centralized in a handful of plain objects updated once per frame in
`loop()`, it's straightforward to later swap the human AI for real player
input, and to sync `dog`/`humans`/`objects` over a network layer (e.g.
WebRTC or WebSockets) without restructuring the rendering or rules code.

## Known limitations
- Single map only (House) in this build.
- No persistent high scores (nothing is saved between sessions).
- Sound is synthesized (chiptune-style beeps), not recorded audio.

Enjoy the chaos!
