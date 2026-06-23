# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A single-file browser game — birthday gift for **Bronx, turning 12**. He loves Spider-Man/Marvel. The game is an endless runner called **Web Swing Dash** (placeholder name).

## Project Status

Currently in the **design/planning phase** — only a design brief exists (`web-swing-dash-design-brief.md`). No code has been written yet. There is no build system, package.json, or test framework.

To run the game once built: open the HTML file directly in a browser, or serve it from any static web host (required for iOS localStorage reliability).

## Settled Design Decisions

- **Genre:** Endless runner, Temple Run / Subway Surfers style — third-person, over-the-shoulder camera, world rushing toward the camera
- **Hero:** Generic spider-hero homage (red/blue color scheme, web-slinging) — original art, NOT Marvel's copyrighted character
- **Controls:** Single tap only — iPad touch, no keyboard
- **Obstacles:** Rooftop gaps/debris, Goblin bombs, Vulture swoops. One hit = instant game over (no lives)
- **Difficulty:** Endless, gradual ramp — forgiving early, builds over ~1 minute of play
- **Visuals:** Comic-book style — bold black outlines, halftone-dot textures, action text bursts ("WHOOSH!")
- **Audio:** Web Audio API — procedurally generated swing/hit SFX + light background beat. No external audio files
- **Scoring:** Top 5 leaderboard, persisted via `localStorage`, 3-letter initials entry (arcade-style cycling)
- **Title screen:** Must show Bronx's name and a happy-birthday message
- **Platform:** iPad only (US, WebKit engine regardless of browser)
- **Delivery:** Must be hosted at a stable URL (not a raw local file) so localStorage persists reliably on iOS. Can use Add to Home Screen

## Open Design Questions (Resolve Before Implementing Game Loop)

The single most critical unresolved question is **how a single tap maps to dodging in the 3D behind-view**:
- Tap = lane shift left/right (Temple Run style)
- Tap = swing upward over low obstacles (vertical dodge)
- Tap-left / tap-right half of screen = directional dodge
- Some combination of the above

Also unresolved: lane-based movement (fixed left/center/right) vs. free positioning, exact game name, and difficulty curve numbers.

## Tech Approach

**Preferred:** Self-contained single HTML file, no build step, no dependencies.

Two viable rendering approaches:
1. **Pseudo-3D in Canvas2D** — scale/reposition objects by simulated Z-depth. Dependency-free; preferred if the perspective math is manageable.
2. **Three.js** — true 3D, adds a CDN dependency but more convincing depth/lighting. Use toon shading + outlines to preserve comic-book look.

Phaser is a poor fit — it's 2D and doesn't naturally give a behind-character 3D perspective.

## Do Not Use the Prior Attempt

An earlier flat 2D Flappy-Bird-style implementation (`web-swing-dash.html`) was discarded — it does not match the Temple Run behind-character direction. Do not use it as a starting point. Some conceptual decisions (palette, villain choices, sound approach, leaderboard logic) still carry over, but the implementation does not.
