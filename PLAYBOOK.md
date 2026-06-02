# 🎮 QUANG ARCADE — Game Playbook

Tài liệu chuẩn để **build một game mới**, **tạo repo + deploy**, rồi **gắn vào arcade hub**.
Cứ làm theo đúng đây là game mới sẽ đồng bộ về phong cách, chất lượng và quy trình với 6 game hiện có.

> Quy ước viết: **prose tiếng Việt** cho giải thích, **tiếng Anh** cho code / lệnh / technical terms.
> Mọi game đều: **single `index.html`, zero-build, no framework, no bundler, chơi ngay trên browser**.

**Hub:** https://quangle1997.github.io/arcade/ · **Pattern URL game:** `https://quangle1997.github.io/<repo>/`

---

## 0. Triết lý (đọc 30 giây)

1. **Một file `index.html`** chứa hết HTML + CSS + JS. Không build step, không dependency cài đặt — chỉ Google Fonts qua CDN.
2. **Chơi ngay (instant play)** — mở link là vào game, không install, không login, free.
3. **Mobile + Desktop** đều mượt: touch + keyboard/mouse, layout responsive, vùng chơi là "hero".
4. **Sang trọng / arcade-neon**: gradient-mesh background, glassmorphism panel, font Orbitron + Space Grotesk, mỗi game một màu `--accent`.
5. **UI tiếng Anh** (public-facing). Code/comment tiếng Anh.
6. **Ship được là phải verify**: 0 console error, test desktop + mobile, có OG image để share, có README.

---

## 1. Style / Design System — chuẩn build UI game mới

### 1.1 Tech constraints (bắt buộc)
- 1 file `index.html`. Asset (ảnh, og-card) để trong `assets/`.
- Không framework (no React/Vue), không bundler, không npm install. JS thuần.
- Chỉ external resource = Google Fonts. Mọi thứ khác inline.

### 1.2 Fonts — luôn dùng cặp này
```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;800;900&family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet" />
```
- **Orbitron** = display / tiêu đề / số / nút (cảm giác arcade, hi-tech).
- **Space Grotesk** = body / mô tả / text thường.

### 1.3 Color tokens + nền gradient-mesh (copy vào `:root`)
```css
*{box-sizing:border-box;margin:0;padding:0;}
:root{
  --bg:#06060d; --ink:#eef0ff; --mut:#9aa0c0; --line:rgba(255,255,255,.10);
  --disp:'Orbitron',system-ui,sans-serif;
  --body:'Space Grotesk',system-ui,-apple-system,sans-serif;
  --accent:#56d4ff;            /* ĐỔI MÀU NÀY cho mỗi game (xem bảng accent §5) */
}
body{ background:var(--bg); color:var(--ink); font-family:var(--body);
  min-height:100vh; overflow-x:hidden; line-height:1.45; -webkit-font-smoothing:antialiased; }

/* nền mesh trôi nhẹ phía sau mọi thứ */
body::before{
  content:""; position:fixed; inset:-30%; z-index:-2; pointer-events:none;
  background:
    radial-gradient(40% 40% at 18% 22%, rgba(231,181,74,.16), transparent 60%),
    radial-gradient(38% 38% at 82% 18%, rgba(127,224,224,.16), transparent 60%),
    radial-gradient(45% 45% at 76% 84%, rgba(255,90,138,.15), transparent 60%),
    radial-gradient(42% 42% at 22% 82%, rgba(167,139,250,.15), transparent 60%);
  filter:blur(20px); animation:mesh 26s ease-in-out infinite alternate;
}
@keyframes mesh{ from{transform:translate3d(-3%,-2%,0) scale(1)} to{transform:translate3d(3%,3%,0) scale(1.08)} }
@media (prefers-reduced-motion:reduce){ *{animation:none!important;} }
```

### 1.4 Glassmorphism panel (công thức tấm kính mờ)
```css
.panel, .card, .chip {
  border:1px solid var(--line);
  background:linear-gradient(180deg,rgba(22,24,38,.72),rgba(12,13,22,.82));
  backdrop-filter:blur(8px);
  border-radius:16px;
}
```
- Glow theo accent (hover / khung vùng chơi):
```css
box-shadow:0 24px 60px -18px color-mix(in srgb,var(--accent) 55%,transparent),
           0 0 0 1px color-mix(in srgb,var(--accent) 30%,transparent) inset;
```

### 1.5 Typography hierarchy
- **Tiêu đề lớn**: Orbitron 900, `text-transform:uppercase`, `letter-spacing:.04em`, gradient clip text:
```css
background:linear-gradient(95deg,#ffd24a,#7fe0e0,#a78bfa,#ff5a8a);
-webkit-background-clip:text; background-clip:text; color:transparent;
```
- **Nhãn / kicker / chip / nút**: Orbitron 600–800, uppercase, `letter-spacing:.12em–.42em`, font nhỏ (10–13px).
- **Body**: Space Grotesk 400–500.
- Dùng `clamp()` cho mọi font-size lớn để tự co theo màn hình, vd `font-size:clamp(34px,9vw,104px)`.

### 1.6 Responsive (mobile + desktop)
- **Vùng chơi (board/canvas/play-area) = hero**: to nhất, ở trên (mobile) hoặc bên trái (desktop). Cap để không tràn: `max-width:min(100%,70vh)` hoặc tương tự.
- **Desktop**: cân nhắc layout 2 cột (vùng chơi | panel điều khiển) bằng CSS grid.
- **Mobile**: stack 1 cột, control nằm dưới vùng chơi (vừa tầm ngón cái), list lựa chọn cuộn ngang nếu dài.
- Breakpoint hay dùng: `@media (max-width:860px)` (đổi sang 1 cột), `@media (max-width:560px)`.
- **Viewport cho GAME** (khoá zoom, cảm giác như app):
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
```
  (Hub arcade thì để scalable — bỏ `maximum-scale/user-scalable`.)
- Input: hỗ trợ **cả** touch (tap/drag) **và** keyboard (arrows/WASD) + mouse.

### 1.7 Audio
- SFX nhỏ bằng **WebAudio API** (oscillator/gain) inline — không file mp3.
- Phải khởi tạo `AudioContext` sau user-gesture đầu tiên (mobile chặn autoplay). Có nút bật/tắt tiếng.

### 1.8 State / lưu tiến độ
- Dùng `localStorage` cho best score / record / lựa chọn người chơi. Namespace key theo game, vd `slidequest.best.4`, `slidequest.custom`.

---

## 2. `<head>` template cho game mới

Copy nguyên block, thay `SLIDE QUEST`, mô tả, emoji favicon, `<repo>`, và kích thước OG.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
<title>GAME NAME — One-line tagline · Play Free</title>
<meta name="description" content="GAME NAME — short pitch. What you do, why it's fun. Play free in your browser." />
<!-- Open Graph / social link preview -->
<meta property="og:type" content="website" />
<meta property="og:site_name" content="GAME NAME" />
<meta property="og:title" content="GAME NAME — tagline" />
<meta property="og:description" content="Short pitch for sharing." />
<meta property="og:url" content="https://quangle1997.github.io/<repo>/" />
<meta property="og:image" content="https://quangle1997.github.io/<repo>/assets/og-card.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="675" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="GAME NAME — tagline" />
<meta name="twitter:description" content="Short pitch for sharing." />
<meta name="twitter:image" content="https://quangle1997.github.io/<repo>/assets/og-card.jpg" />
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='88'>🧩</text></svg>" />
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;800;900&family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet" />
<style> /* ... §1 tokens + game CSS ... */ </style>
</head>
```

---

## 3. Yêu cầu bắt buộc cho mỗi game (definition of done)

Một game chỉ được coi là "xong + ship" khi đủ hết:

- [ ] **Single `index.html`**, zero-build, mở file là chạy.
- [ ] **Responsive**: chạy đẹp desktop (≥1280px) **và** mobile (390px). Vùng chơi là hero, không tràn.
- [ ] **Input kép**: touch + keyboard/mouse.
- [ ] **UI tiếng Anh** toàn bộ (không sót tiếng Việt — kể cả chữ HOA có dấu).
- [ ] **Style chuẩn §1**: fonts Orbitron+Space Grotesk, mesh bg, glass panel, có `--accent` riêng.
- [ ] **OG / social tags** đầy đủ + `assets/og-card.jpg` (**1200×675**) → share FB/Zalo/X có ảnh đẹp.
- [ ] **Key-art**: ảnh đẹp (gen bằng media-tools, §7) cho og-card và/hoặc banner trong game.
- [ ] **`localStorage`** lưu record/lựa chọn (nếu game có điểm/tiến độ).
- [ ] **Audio**: SFX WebAudio + nút mute, init sau user-gesture.
- [ ] **0 console error** (trừ favicon 404 khi test local — chấp nhận được).
- [ ] **`README.md`** (xem §6) có Live link + dòng "part of QUANG ARCADE".
- [ ] **Verify** đã chạy (xem §4.3) trước khi commit.
- [ ] **Đã gắn vào arcade hub** (§5).

---

## 4. Quy tắc phát triển

### 4.1 Tự chủ — KHÔNG cần confirm
- Tự quyết định hướng làm → **implement → verify → commit + push thẳng `main`**, không hỏi xác nhận từng bước.
- Làm xong mỗi feature thì commit + push luôn, không gom lại.

### 4.2 Git safety (CỰC KỲ QUAN TRỌNG — repo public/shared)
- ❌ **TUYỆT ĐỐI KHÔNG** `git add -A` / `git add .` / `git add --all`.
- ✅ **LUÔN** chạy `git status` + `git diff --stat` trước khi add.
- ✅ **CHỈ** `git add <file1> <file2>` đúng những file mình tạo/sửa.
- ❌ Không commit file của người khác, không commit secrets / API keys.
- ✅ `git diff --staged` review lại trước khi commit.
- ✅ **Remote = HTTPS** (SSH thường chưa auth → push fail):
  ```bash
  git remote set-url origin https://github.com/QuangLe1997/<repo>.git
  ```
- ✅ Commit message kết thúc bằng:
  ```
  Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>
  ```

### 4.3 Verify protocol (làm trước khi ship)
1. **Syntax**: tách inline `<script>` ra file tạm rồi `node --check`. (Hoặc `node test.js` nếu game có test, như tank-shooter.)
2. **Serve local no-cache**:
   ```bash
   cd <repo> && python3 -m http.server 8773
   # test no-store để khỏi dính cache khi sửa nhanh
   ```
3. **Playwright MCP**: navigate tới `http://localhost:8773`, chụp **desktop 1280×800** + **mobile 390×844**, kiểm tra layout + `browser_console_messages(level:error)` = **0**.
4. **Functional**: bấm thử qua đúng UI thật (không gọi hàm tắt) — chơi 1 lượt, đổi setting, win/lose, reload còn nhớ record.

---

## 5. Tạo repo mới + deploy GitHub Pages

```bash
cd /Users/quang/Documents/my_project/game
mkdir <repo> && cd <repo>
# tạo index.html, assets/, README.md ...

git init
git add index.html README.md assets/*           # CHỈ file của mình (xem §4.2)
git commit -m "feat: initial <GAME NAME> — single-file HTML5 game

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"

# tạo repo public trên GitHub (gh đã auth sẵn dưới user QuangLe1997)
gh repo create QuangLe1997/<repo> --public --source=. --remote=origin

# ⚠️ Nếu remote bị set thành SSH → đổi sang HTTPS rồi push
git remote set-url origin https://github.com/QuangLe1997/<repo>.git
git push -u origin main
```

**Bật GitHub Pages** (branch `main`, folder `/root`):
```bash
gh api -X POST repos/QuangLe1997/<repo>/pages \
  -f 'source[branch]=main' -f 'source[path]=/' 2>/dev/null || \
gh api -X PUT repos/QuangLe1997/<repo>/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```
→ Vài chục giây sau live tại `https://quangle1997.github.io/<repo>/`.
Verify deploy bằng `curl -fsSL "https://quangle1997.github.io/<repo>/?cb=$(date +%s)"` rồi grep marker.

> Bảng màu `--accent` đã dùng (chọn màu mới khác biệt cho game tiếp theo):
> tank `#e7b54a` · serpent `#7fe0e0` · dino `#ffb24a` · suika `#ff5a8a` · brick `#a78bfa` · slide `#56d4ff`.

---

## 6. README.md cho game (template)

```markdown
# <emoji> GAME NAME — Tagline

One-paragraph pitch: cái gì, làm gì, vui chỗ nào.

**▶ Live:** https://quangle1997.github.io/<repo>/ · part of [QUANG ARCADE](https://quangle1997.github.io/arcade/)

## Features
- ...
- ...

---
Built by [QuangLe1997](https://github.com/QuangLe1997) · crafted with ♥ & Claude Code.
```

---

## 7. Gen key-art + OG image bằng media-tools (MCP)

1. **Tạo generation** — `create_generation_api_generation_post`:
   - `model: "gpt-image-2"`, `aspect_ratio` (vd `"16:9"` cho og-card), `prompt`, `negative_prompt`.
   - ⚠️ **Lưu ý quan trọng**: MCP hay **timeout ở tầng giao tiếp** nhưng task **vẫn chạy/hoàn tất ở server**. Đừng gen lại nhiều lần.
2. **Lấy kết quả**:
   - `list_history_api_history_get` (search theo name) hoặc `get_generation_api_generation__task_id__get` → lấy `output_urls`.
3. **Tải + resize** bằng `sips` (macOS):
   ```bash
   curl -fsSL "<output_url>" -o assets/og-card.jpg
   sips -z 675 1200 assets/og-card.jpg            # OG 1200×675
   sips -Z 800 assets/<id>-key.jpg                # thumbnail cho arcade (~800px)
   sips -g pixelWidth -g pixelHeight assets/og-card.jpg   # verify kích thước
   ```
4. **Dọn rác**: gen xong, ship xong thì xoá task tạm bằng `delete_task_api_history__task_id__delete`
   (⚠️ search có thể trả về task không liên quan của project khác — **chỉ xoá đúng task mình tạo**).

> Prompt nên: theo tông neon/arcade của game, có chừa khoảng "an toàn" cho việc crop 16:10 ở arcade card, không nhồi chữ (text do gpt-image hay bị méo).

---

## 8. Gắn game thành phẩm vào ARCADE HUB

File: `arcade/index.html`. **Quy tắc bảo mật: chỉ HTML tĩnh** — KHÔNG dùng `innerHTML`/JS templating để render card (security hook chặn). Cứ copy-paste 1 block `<a class="card">`.

### B1 — Thumbnail
- Bỏ ảnh key-art vào `arcade/assets/<repo>.jpeg` (tỉ lệ ~16:10, ~800px, crop `object-fit:cover`).

### B2 — Thêm card (copy block này vào trong `<main class="grid">`)
```html
<a class="card" style="--accent:#XXXXXX" href="https://quangle1997.github.io/<repo>/" target="_blank" rel="noopener" aria-label="Play GAME NAME">
  <div class="thumb">
    <span class="badge">★ Newest</span>           <!-- chỉ card mới nhất mới có badge này -->
    <span class="num">0N</span>                     <!-- số thứ tự 2 chữ số -->
    <img src="assets/<repo>.jpeg" alt="GAME NAME screenshot" loading="lazy" />
  </div>
  <div class="meta">
    <div class="gtitle"><span class="dot"></span>GAME NAME</div>
    <div class="gsub">Short genre line</div>
    <p class="gdesc">2-3 câu mô tả hấp dẫn, đúng giọng quảng cáo.</p>
    <div class="tags"><span class="tag">Tag1</span><span class="tag">Tag2</span><span class="tag">Tag3</span></div>
    <span class="play">▶ Play Now</span>
  </div>
</a>
```
- ➜ Gỡ `<span class="badge">★ Newest</span>` ở card **cũ trước đó** (chỉ 1 game được "Newest").

### B3 — Cập nhật số lượng game (đang là **6** → sửa thành 7…)
Trong `arcade/index.html`:
- `<title>` + `<meta name="description">` + `og:title`/`og:description`/`twitter:*` (chữ "6 Free Browser Games").
- Hero: `.sub` ("Six hand-built…") + chip `<span class="chip"><b>6</b>&nbsp; Games</span>`.

Và bảng Games trong `arcade/README.md`.

### B4 — (tuỳ chọn) Regen OG collage của hub
- `arcade/assets/og-arcade.jpg` là ảnh collage AI. Nếu muốn cập nhật có game mới thì gen lại theo §7.

### B5 — Commit + push arcade
```bash
cd /Users/quang/Documents/my_project/game/arcade
git status && git diff --stat
git add index.html README.md assets/<repo>.jpeg     # chỉ file liên quan
git commit -m "feat: add GAME NAME to arcade (now N games)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
git push origin main
```

---

## 9. Ship checklist (TL;DR — bám đúng thứ tự)

```
[ ] 1. Build index.html  → style §1, head §2, responsive mobile+desktop, EN, audio, localStorage
[ ] 2. Gen key-art + og-card.jpg (1200×675) bằng media-tools §7
[ ] 3. Viết README.md §6
[ ] 4. Verify §4.3: node --check + Playwright desktop 1280×800 & mobile 390×844 + 0 console error + chơi thử
[ ] 5. Tạo repo + push (HTTPS) §5 → bật GitHub Pages → verify live
[ ] 6. Gắn vào arcade §8: thumbnail + card + tăng số game + README + push
[ ] 7. Confirm cả 2 URL live: game + arcade
```

---

## 10. Inventory hiện tại (6 games)

| # | Game | `--accent` | Genre | Repo / Live |
|---|------|-----------|-------|-------------|
| 01 | **STEEL SIEGE** | `#e7b54a` | 3D tank defense (twin-stick, roguelite) | `tank-shooter` |
| 02 | **NEON SERPENT 3D** | `#7fe0e0` | Snake reborn in 3D | `neon-serpent-3d` |
| 03 | **DINO EGG POP** | `#ffb24a` | Match-3 bubble shooter | `dino-egg-shooter` |
| 04 | **SUIKA MERGE** | `#ff5a8a` | Watermelon merge physics | `suika-merge` |
| 05 | **BRICK BLITZ** | `#a78bfa` | Glossy "metal Tetris" | `brick-blitz` |
| 06 | **SLIDE QUEST** | `#56d4ff` | Sliding picture puzzle (+ upload ảnh) | `slide-puzzle` |

---
Built by [QuangLe1997](https://github.com/QuangLe1997) · crafted with ♥ &amp; Claude Code · zero-build HTML5 · GitHub Pages
