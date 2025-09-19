# Repository Guidelines

## Core Principles & Guardians

"First Things First"—what sits at the top ships first, and task order equals priority. "Weniger ist mehr" means every new idea must beat the challenge "Siehst du ein Feature, töte es!" before it lands. "Deine Daten, dein JSON" forbids logins, servers, and third-party calls; everything must keep working offline on a 2015 phone. The four guardians enforce this flow: shiva keeps us on vanilla tech, apollo defends clarity, thanos deletes the excess, and themis verifies only what would cost money if it broke. Pass the Mutter-Test each time: would a non-technical person grasp the change in 30 seconds?

## Project Structure & Minimal Footprint

`index.html` is the product—HTML shell, inline CSS, and vanilla JS scheduling logic. Supporting philosophy lives in `manifest.md`, `CLAUDE.md`, and `GENESIS.md`; agent briefs sit in `.claude/agents/`. `example.jsonc` shows the canonical schema with human-readable names. Do not add pipelines, asset directories, or bundled tooling unless the single-file constraint is genuinely impossible.

## Build & Manual Test Loop

There is no build step. Open `index.html` in a browser, or serve the root via `python3 -m http.server 8000` when LocalStorage needs an origin. Smoke-test drag-and-drop, owner edits, dependency arrows, and save/load persistence within 30 seconds. Validate layout in at least one Chromium and one WebKit browser, and sanity-check on a small screen to honour the Bauhaus-like minimal surface.

## Coding Style & Naming Conventions

Stay under 500 lines, keep everything inline, and rely on the existing 8px spacing rhythm and three-colour palette (black, gray, one accent). Use `const`/`let`, camelCase functions like `renderTimeline`, and noun-based keys (`shortCode`). Prefer early returns over branching pyramids, and document intent only when behaviour is non-obvious. Preserve the schema rule "Position = Priorität"—IDs are ordered numbers, not opaque hashes.

## Commit & PR Guidelines

Write imperative commits (`Trim drag inertia`), keep diffs under ~100 lines, and explain every addition in terms of what was removed or simplified. Pull requests should restate the user-facing effect, link issues, include before/after visuals for UI tweaks, and list the manual 30-second test steps you ran. Flag any trade-offs that could inflate bundle size, add dependencies, or weaken the manifesto so reviewers can veto early.
