# Web Swing Dash — Game Design Brief

A birthday gift game for **Bronx**, turning 12. He loves Spider-Man/Marvel. Drawing was deliberately left out of the gameplay/design.

---

## Core Concept
- **Genre:** Endless runner, arcade-style, chase-a-high-score
- **Camera/perspective:** Third-person, over-the-shoulder — like **Temple Run / Subway Surfers**. The camera sits behind the character; he runs/swings forward, away from the viewer, with the world rushing toward the camera. **This is the corrected vision — not a flat 2D side-view.**
- **Theme:** Spider-Man inspired. Build an **original spider-hero** (classic red/blue color split, web-slinging) rather than reproducing Marvel's actual copyrighted character art — keep it a generic homage, not a licensed likeness.
- **Hero personality:** Generic spider-hero, no special nickname (explicitly decided against something like "Bronx-Man").

## Gameplay Mechanic
- Original framing from the start: "swing where the obstacles aren't" — a Temple-Run-style dodge game but swinging on a web instead of running on foot.
- **Control input: single tap** (simple, one-button control — confirmed, not tap-and-hold or swipe).
- **Open question (unresolved):** in a behind-the-character 3D/pseudo-3D view, what does a single tap actually do? Possibilities to decide before building:
  - Tap = swing to dodge left/right between lanes (like Temple Run's lane-shifting)
  - Tap = swing upward over low obstacles (vertical dodge, more like the original "swing where obstacles aren't" idea)
  - Tap location (left half / right half of screen) determines swing direction
  - Some combination (e.g., tap-left/tap-right for lanes, with obstacles also requiring vertical timing)
  - This needs to be nailed down before implementation — it's the single biggest open design question left.

## Obstacles & Hazards
- General obstacles: gaps, debris, rooftop hazards to dodge
- **Classic Marvel villains as hazards** (not generic-only, not collectibles):
  - Goblin bombs (lobbed hazards)
  - Vulture swoops (diagonal aerial hazard)
- **No collectibles** — explicitly decided against adding bonus pickups.
- **Hit behavior: one hit = instant game over** (classic arcade tension, not a lives/health system).

## Difficulty
- **Endless arcade style** — no fixed ending, just survive and chase a high score.
- **Gradual difficulty ramp** — speed and obstacle frequency should increase slowly over time, not spike aggressively. Should feel forgiving early, build challenge over roughly a minute or so of play.

## Visual Style
- **Comic-book style**: bold black outlines, halftone-dot textures, comic action lines/bursts (e.g., a "WHOOSH!" effect on each swing).
- Note for implementation: however the 3D/pseudo-3D rendering is done (see Tech Notes below), the comic look should still come through — e.g. toon/flat shading with outline effects if using a 3D library, or bold-outlined 2D sprites if faking depth in Canvas2D.

## Sound
- **Yes to sound**: swing/hit sound effects, plus a light background beat/rhythm. Doesn't need to be a full music track — simple and arcade-y is fine.

## Personalization
- **Title screen** should include Bronx's name and a happy-birthday message.

## Scoring & Leaderboard
- **Top 5 high score leaderboard**, persisted between sessions (not just per session).
- **New high score entry**: classic arcade-style **3-letter initials** entry (cycle letters, confirm).

## Platform & Delivery
- **Target device: iPad only** (no computer/keyboard needed — touch-only).
- **Important technical fact:** on iOS/iPadOS, *every* browser — including Chrome — is required to run on Apple's own WebKit engine, except for users physically in the EU (where regulation allows alternative engines). Since this will be used in the US, "Chrome" and "Safari" on his iPad behave identically under the hood. No real benefit to specifically using Chrome.
- **Storage gotcha:** if the game is sent as a raw local HTML file (AirDrop, Messages, Files app), iOS can be inconsistent about treating it as the same storage origin each time it's opened — meaning the high-score leaderboard might not reliably persist.
  - **Fix:** host the game at one stable URL instead of distributing a raw file. (One easy option: Claude's "Publish" feature can generate a public claude.ai link with no account needed to view; any other static web host would work too.) He opens that one link in Safari, and can use **Add to Home Screen** to get an app-like icon. Same origin every time = high scores actually stick.

## Tech Notes for Implementation
- Because the corrected vision needs a behind-the-character camera (not flat 2D), the build needs an actual approach for faking or building depth. Rough options to weigh:
  1. **Pseudo-3D in Canvas2D** — classic trick used by old-school endless runners: scale and reposition objects based on a simulated Z-depth as they approach the camera. Keeps things dependency-free (fits the original "no frameworks, single file" preference) but takes care to get the perspective math right.
  2. **True 3D via a library like Three.js** — more convincing camera/depth/lighting, but adds a real dependency and more moving parts.
  3. A 2D framework (e.g. Phaser) doesn't naturally give a true behind-character 3D look, so it's a weaker fit unless faking depth via sprite scaling similar to option 1.
- Original tech preference (still worth keeping unless the perspective change forces otherwise): self-contained, no build step, easy to send/host as a single file or small bundle. Web Audio API for sound (no audio files needed). `localStorage` for the leaderboard.

## Discarded First Attempt
- An earlier build (`web-swing-dash.html`) was made before this was fully clarified. It's a flat 2D side-view, Flappy-Bird-style swing mechanic — **it does not match the Temple Run-style direction** and shouldn't be used as a starting point. Some decisions above (palette, villain choices, sound approach, leaderboard logic, hero design) still carry over conceptually even though the implementation doesn't.

## Still Open / Not Yet Decided
- **How tap maps to dodging in the 3D/behind-view** (see Gameplay Mechanic above — this is the big one)
- Lane-based movement (fixed left/center/right lanes) vs. free positioning
- Game name (currently just a placeholder: "Web Swing Dash")
- Birthday deadline / how much time is available to build and iterate
- Exact difficulty curve numbers (left to implementation/playtesting)
