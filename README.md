# 🕹️ QUANG ARCADE

A one-page showcase of my browser games — click any card and it plays **instantly** in the browser. No install, no login, free.

**▶ Live:** https://quangle1997.github.io/arcade/

## Games

| # | Game | Genre | Play |
|---|------|-------|------|
| 01 | **STEEL SIEGE** | 3D tank defense (twin-stick, roguelite) | https://quangle1997.github.io/tank-shooter/ |
| 02 | **NEON SERPENT 3D** | Classic snake, reborn in 3D | https://quangle1997.github.io/neon-serpent-3d/ |
| 03 | **DINO EGG POP** | Match-3 bubble shooter | https://quangle1997.github.io/dino-egg-shooter/ |
| 04 | **SUIKA MERGE** | Watermelon merge physics puzzler | https://quangle1997.github.io/suika-merge/ |
| 05 | **BRICK BLITZ** | Glossy "metal Tetris" speed-ramp | https://quangle1997.github.io/brick-blitz/ |

## Tech

- Single `index.html`, **zero build** — no framework, no bundler.
- Google Fonts (Orbitron + Space Grotesk), a CSS gradient-mesh background, and a tiny `<canvas>` particle field.
- Card thumbnails live in `assets/` (each game's key art / title screen).
- Deployed on **GitHub Pages**.

## Adding a game

1. Drop a `assets/<id>.jpeg` thumbnail (≈16:10 or portrait — cards crop with `object-fit:cover`).
2. Copy a `.card` block in `index.html`, set the `--accent` colour, title, tagline, description, tags and the live URL.

---
Built by [QuangLe1997](https://github.com/QuangLe1997) · crafted with ♥ &amp; Claude Code.
