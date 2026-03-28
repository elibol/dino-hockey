# DINO HOCKEY - Zelime's 50th Birthday Edition!

A retro 80s-style hockey game where all team positions are played by different dinosaurs, built as a single-file HTML5 Canvas game.

## How to Play

Open `index.html` in any modern web browser. No server or build step needed.

### Controls (Atari-inspired: simple joystick + one button)

| Input | Action |
|---|---|
| **Arrow Keys / WASD** | Skate around the rink |
| **SPACE + Arrow Keys** | Shoot the puck in the direction you're holding |
| **SPACE** (no arrows) | Pass to nearest teammate |
| **SPACE** (near opponent with puck) | Body check to steal the puck |
| **Z** | Switch control to the teammate nearest the puck |

### Teams

Each team has 5 dinosaurs, one per hockey position:

| Position | Dinosaur | Traits |
|---|---|---|
| Center | T-Rex | Balanced speed, strong shot |
| Left Wing | Velociraptor | Fastest skater, weaker shot |
| Right Wing | Pterodactyl | Agile with flapping wings |
| Defense | Triceratops | Slow but tough body checker |
| Goalie | Ankylosaurus | Widest body, blocks the net |

Green Dinos (player) vs Red Dinos (AI). First to 5 goals wins.

### Referee

**Zelime Elibol** officiates every match, dressed in full 80s glory: big blonde hair, hot pink headband, neon pink off-shoulder top, teal leggings, leg warmers, and wrist sweatbands. She follows the play on ice and raises her arm to signal goals. It's her 50th birthday!

---

## How This Game Was Made

This game was built iteratively through a conversation with Claude (Anthropic's AI assistant, model claude-opus-4-6) in March 2026. The user provided a series of natural-language instructions, and Claude generated all the code.

### Instruction History

1. **"Can you make a fortnite character?"**
   - Initial request. Claude created a single-file HTML5 Canvas interactive Fortnite-style character with movement, emotes, inventory, and a HUD. Top-down perspective wasn't specified — the first version was a side-view character.

2. **"Can you instead make a simple 80s retro style hockey game, where all the roles in the team are different dinosaurs?"**
   - Complete pivot from Fortnite to a retro hockey game. Claude designed:
     - A top-down rink rendered at 384x240 pixels (low-res for retro feel), scaled up with `image-rendering: pixelated`
     - 5 dinosaur species mapped to 5 hockey positions
     - Simple AI for both teammates and opponents
     - Puck physics with ice friction, wall bouncing, and goal detection
     - Retro sound effects via Web Audio API (square/sine wave beeps)
     - CRT scanline overlay and screen shake effects
     - Game state machine: TITLE → FACEOFF → PLAY → GOAL → WIN

3. **"Have the referee be Zelime Elibol (an actual person). It's her 50th birthday today. Have Zelime dressed in classic 80's clothing."**
   - Added Zelime as an on-ice referee character with:
     - Pixel-art sprite in 80s outfit (hot pink top, teal leggings, leg warmers, headband, big hair, shoulder pads, sweatbands, hoop earrings)
     - AI that follows the play, offset to the side of the puck
     - Arm-raise animation when goals are scored
     - Birthday references throughout (title screen, ticker, win screen)
     - Birthday jingle sound effect on game start

4. **"Some improvements: Slow the pace of the game down. Improve resolution and better graphics. Make Zelime blonde."**
   - Major visual overhaul:
     - Resolution doubled from 384x240 to 768x480
     - All sprites redrawn at 2x size with more detail (shading, belly colors, teeth, pupils)
     - Rink upgraded: gradient ice, ice scratch textures, 3D board edges, goal posts, crowd in stands
     - Zelime's hair changed from dark brown (#2a1506) to golden blonde (#e8c44a) with highlights
     - Zelime's sprite made larger with more visible details (earrings, shoe detail, etc.)
   - Gameplay slowed:
     - Player acceleration reduced from 0.22 to 0.14
     - AI acceleration reduced from 0.15 to 0.08
     - Friction increased for more deceleration
   - Added ice spray particles when skating and on body checks

5. **"More improvements: The happy birthday zelime text overlaps with the period time ticker. Make the selected player indicator more obvious. Spend a bit of time improving the dinosaurs. Add instructions on how to steal the puck, and pass the puck to teammates, and shoot the puck in a particular direction."**
   - HUD fix: Moved the birthday ticker from the scoreboard area to the bottom of the screen
   - Selection indicator completely redesigned: pulsing glow rings, bouncing double-chevron above player, direction arrow, dark background behind name label
   - Dinosaur sprites improved across all 5 species:
     - T-Rex: dorsal ridge, body spots, nostril, more teeth, brow ridge, toed feet
     - Raptor: body stripes, proto-feather tufts, fiercer yellow eye, curved sickle claws
     - Pterodactyl: wing finger bones, furry body texture, taller dramatic crest, colored beak tip
     - Triceratops: scalloped frill with spots, beak, more prominent horns, body spots
     - Ankylosaurus: multiple rows of armor plates, side spikes, beak, head horn nubs
   - New gameplay mechanics:
     - **Directional shooting**: Hold arrow keys + press SPACE to shoot in any direction
     - **Passing**: Press SPACE with no arrows held to pass to nearest teammate
     - **Stealing**: Press SPACE near an opponent who has the puck for a body check
   - Controls text updated everywhere (info bar, title screen, HUD)

6. **"Take inspiration from early Atari games on the controls."**
   - Simplified to Atari-style one-button design: SPACE is context-sensitive
     - With puck + arrows = directional shot
     - With puck + no arrows = pass
     - Without puck + near opponent = steal
   - Z remains as a secondary player-switch button

7. **"Also, on the intro page, avoid overlapping text."**
   - Title screen layout redesigned:
     - Zelime moved to the left side at reduced scale (1.4x instead of 1.8x)
     - Birthday text positioned to the right, separate from Zelime's sprite
     - Dino lineups pushed down with more vertical spacing
     - Controls section given its own bordered box with clear two-column layout
     - All text items spaced to avoid any overlap

### Technical Architecture

- **Single HTML file** — no dependencies, no build step, no frameworks
- **Dual-canvas rendering** — game drawn at 768x480 on an offscreen canvas, then scaled up to display canvas with `image-rendering: pixelated` for the retro chunky pixel look
- **Game loop** — `requestAnimationFrame` driving `update()` → `draw()` at 60fps
- **State machine** — TITLE / FACEOFF / PLAY / GOAL / WIN states with timers
- **Physics** — velocity + friction model for players and puck; wall collision with bounce; goal detection through open goal areas
- **AI** — role-based behavior: goalies track puck Y, closest non-goalie chases loose pucks, puck carriers shoot when in range, defenders fall back, attackers get open
- **Audio** — Web Audio API oscillators generating retro beeps (square/sine/sawtooth waves)
- **Sprites** — all drawn procedurally with `fillRect` calls (no image assets), giving an authentic pixel-art look at the low game resolution
