# 🕹️ QUANG ARCADE

A one-page showcase of my browser games — click any card and it plays **instantly** in the browser. No install, no login, free.

**▶ Live:** https://quangle1997.github.io/arcade/

> 📘 **Building a new game?** Read the **[Game Playbook](PLAYBOOK.md)** — the full spec for style/design system, repo + GitHub Pages deploy, dev rules (git safety, verify protocol), media-tools key-art, and how to wire a finished game into this hub.

## Games

| # | Game | Genre | Play |
|---|------|-------|------|
| 01 | **STEEL SIEGE** | 3D tank defense (twin-stick, roguelite) | https://quangle1997.github.io/tank-shooter/ |
| 02 | **NEON SERPENT 3D** | Classic snake, reborn in 3D | https://quangle1997.github.io/neon-serpent-3d/ |
| 03 | **DINO EGG POP** | Match-3 bubble shooter | https://quangle1997.github.io/dino-egg-shooter/ |
| 04 | **SUIKA MERGE** | Watermelon merge physics puzzler | https://quangle1997.github.io/suika-merge/ |
| 05 | **BRICK BLITZ** | Glossy "metal Tetris" speed-ramp | https://quangle1997.github.io/brick-blitz/ |
| 06 | **SLIDE QUEST** | Sliding picture puzzle (3×3–5×5) | https://quangle1997.github.io/slide-puzzle/ |
| 07 | **BLOCK BLAST** | Neon 8×8 block puzzle (clear lines, no gravity) | https://quangle1997.github.io/block-blast/ |
| 08 | **TRIPLE MATCH 3D** | Glossy 3D tile-match puzzle (match 3, bloom glow) | https://quangle1997.github.io/triple-tile/ |
| 09 | **PRISM POUR** | Water-sort puzzle (pour colors into matching tubes) | https://quangle1997.github.io/prism-pour/ |
| 10 | **PULSE SURVIVOR** | Neon survivors-like bullet-heaven (auto-fire, OVERDRIVE burst) | https://quangle1997.github.io/pulse-survivor/ |
| 11 | **OVERCLOCK** | Active-idle energy-reactor clicker (tap core, generators, SURGE, prestige) | https://quangle1997.github.io/overclock/ |
| 12 | **BEATFALL** | Synthwave 4-lane rhythm tap (Perfect/Good/Miss, combo, STROBE ×2, S/A/B/C grades) | https://quangle1997.github.io/beatfall/ |
| 13 | **ION TOWERS** | Synthwave tower defense — 8 maps, 3 towers (RAPID/CANNON/TESLA), boss waves, OVERCHARGE active ability | https://quangle1997.github.io/ion-towers/ |
| 14 | **SKYLINE STACK** | One-tap 3D tower stacker — slice the overhang, chain PERFECT drops to grow back & turn the skyline white-hot | https://quangle1997.github.io/skyline-stack/ |

## Tech

- Single `index.html`, **zero build** — no framework, no bundler.
- Google Fonts (Orbitron + Space Grotesk), a CSS gradient-mesh background, and a tiny `<canvas>` particle field.
- Card thumbnails live in `assets/` (each game's key art / title screen).
- Deployed on **GitHub Pages**.

## Adding a game

Short version (full step-by-step in **[PLAYBOOK.md §8](PLAYBOOK.md)**):

1. Drop a `assets/<id>.jpeg` thumbnail (≈16:10 or portrait — cards crop with `object-fit:cover`).
2. Copy a `.card` block in `index.html` (static HTML only — no JS templating), set the `--accent` colour, `num`, title, tagline, description, tags and the live URL. Move the `★ Newest` badge to the new card.
3. Bump the game count in `<title>` / meta / OG / hero chip, update the Games table above, then `git add` only those files and push.

---
Built by [QuangLe1997](https://github.com/QuangLe1997) · crafted with ♥ &amp; Claude Code.
