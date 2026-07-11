# neon-canyon-run
Simple temple run replica - retro style, contains simple feature like. Made just for fun, welcome any feedback.

# Neon Canyon Run — Build Log

## Initial Request (Slack draft brief)
A premium mobile game developer brief for a single-file, mobile-responsive HTML5 canvas game inspired by Temple Run:
- Pseudo-3D perspective canyon/road loop, no external assets or heavy frameworks
- Procedurally curving road (sine-wave/segment curvature) with retro-neon styling
- Obstacles spawning on the horizon, scaling up as they approach
- Touch swipe / screen-split controls across 3 lanes
- Scoring by distance, speed multiplier that ramps over time, tap-to-restart Game Over screen
- Canvas auto-scales to any phone screen
- Output constraint: under 250 lines, zero layout crashes

---

## v1 — Initial build
Delivered the brief as specified: pseudo-3D projected road with sine-wave curvature, retro-neon synthwave sky, glowing block/archway obstacles scaling in from the horizon, swipe + screen-split touch controls, distance-based scoring with ramping speed multiplier, and a tap-to-restart Game Over screen.

## v2 — Difficulty, lives, gems, missions foundation
Added: difficulty selection (Chill / Classic / Insane), traps that cost a life on contact, gems that accumulate toward a life refill (max 3 lives), jump via quick upward swipe, a top HUD dashboard (lives, score, gem progress bar), a 😢 Game Over screen, pause with in-memory saved progress, a greyish "grim" animation on falling into a trap, and mini fireworks + sound + haptics on gem pickup.

## v3 — Bug fixes + power-ups, combo, missions, day/night, tilt
Fixed two bugs from v2: a script crash on load (menu rendered before any road existed) and a scoring bug (bonus points silently overwritten each frame). Added: three power-ups (🧲 magnet, 🛡️ shield, 🐌 slow-mo), a combo multiplier for gem streaks, a day/night palette cycle every 500m, near-miss bonus scoring, a tilt-steering toggle, and a 3-mission system per run.

## v4 — Fixed invisible obstacles/gems/traps/curves
Diagnosed and fixed the core reported bug: the perspective projection math was placing every road row below the visible screen, so obstacles, traps, gems, power-ups, and the curving road were all rendering off-canvas — only the sky, ground color, and ship were visible. Rebuilt the projection so the near row fills the bottom of the screen and rows converge correctly at the horizon; collision detection re-aligned to match what's now visible. Added a scrolling attract-mode road behind the menu.

## v5 — Fluid swipe response
Fixed sluggish steering: lane changes previously only registered on finger-lift (touchend), adding the full gesture duration as input lag. Switched to live `touchmove` steering that fires mid-gesture, nearly doubled the lane-slide speed, and added a banking lean animation plus a haptic tick per lane change.

## v6 — Script-error troubleshooting + tilt fix + real QA
Built a headless QA harness (Node + stubbed browser APIs) and ran ~25,000 simulated frames of play, difficulty switching, input, pause/resume, and death cycles — confirmed the script itself was error-free, so added an on-screen error reporter (`window.onerror` toast) to catch any environment-specific error on your device going forward. Found and fixed a real bug via the harness: a "death cascade" where multiple lives could be lost during the single grim-animation window. Rebuilt the tilt-steering button to show its actual permission state (Requesting / Denied / Blocked / Not Supported) instead of failing silently.

## v7 — Continuous steering, accelerator pads, rocket power-up
Replaced stepped lane-hopping with continuous drag steering — the ship now tracks your finger pixel-for-pixel (diagonal drags work), and the camera shift was widened so the outer lanes actually reach the road edges. Jump is now triggered by swipe velocity rather than distance. Added accelerator pads (glowing chevron strips) that launch a mega jump over a pre-laid line of gold gems, and a 🚀 rocket power-up granting 10 seconds of mega jumps that clear every obstacle, including arches. Clarified that tilt-steering "Denied" is an iOS HTTPS requirement, not a bug — the button now explains this directly. Verified with the full QA harness plus targeted tests for the boost pad, gem-scooping, continuous steering, and rocket mechanics.
