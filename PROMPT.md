# Prompt: Adrian Cornejo — personal portfolio (three sections + footer)

Build a single-file, three-section personal portfolio for **Adrian Cornejo Palacios**, a full-stack software developer based in Lima, Peru. Use plain HTML, CSS and vanilla JS, with no framework and no build step. The look is a strict monochrome editorial layout. A light grey hero with a large black-and-white photo of Adrian seen from behind (his name on a glass plate over his back) is followed by a black manifesto section whose words fill from grey to white, then a bordered "work & skills" panel with stat cards, and a slim footer with contact links.

The site is **bilingual: Spanish by default, English as an option** (see "Language"). The copy written throughout this prompt is the English version.

The page must look intentional at **three layouts**: desktop (≥ 1200px), tablet (761–1199px) and phone (≤ 760px). Each layout has its own artboard (see "Scale"). It must never scroll sideways at any width.

---

## Media — source files (find them by name)

All media files are already in the folder where this prompt is run (the project root or any subfolder). **Find each one by its base name, case-insensitive, with any extension.** Throughout this prompt, `{{KEY}}` refers to the processed asset in this table.

**Before building:**
1. Locate every file below. If a **required** file is missing, list the missing names and stop; do not invent or substitute images. If two files match one name, ask which to use.
2. Process each file as described and save it to its target path. Move the originals into `_source/` and add `_source/` to `.gitignore`, so the repo only carries the optimised versions.
3. Use `ffmpeg` (and `cwebp` if present) for conversions. Check they exist first (`ffmpeg -version`); if a tool is missing, tell Adrian what it is for and ask before installing anything. Never upscale an image or video beyond its source resolution.

| Key | Source file name | What it is | Process → target |
|---|---|---|---|
| `{{HERO_BACK}}` | `hero-back` | **Required.** B&W photo of Adrian seen from behind, on a flat light-grey background. | Convert to webp (quality 82), keep its resolution → `assets/img/hero-back.webp` |
| `{{AVATAR_IDLE_1}}` | `idle-front-1` | **Required.** Pixel-art avatar, head, front view, 32 × 30 transparent PNG. | **Copy untouched** (no conversion, no resize) → `assets/img/avatar/idle-front-1.png` |
| `{{AVATAR_IDLE_2}}` | `idle-front-2` | *Optional.* Blink frame of the same avatar. | Copy untouched → `assets/img/avatar/idle-front-2.png` |
| `{{VIDEO_FINGIDAZO}}` | `fingidazo` (video) | **Required.** Screen recording of *El Fingidazo*, landscape, ~40 s+. | **Trim** to one 8–10 s segment (see "Video trimming"), then encode → `assets/video/fingidazo.mp4` |
| `{{POSTER_FINGIDAZO}}` | `fingidazo-poster` | Adrian's still (fallback only). | **Regenerate** from the first frame of the trimmed video, webp quality 80 → `assets/video/fingidazo-poster.webp`. Use Adrian's still only if extraction fails. |
| `{{VIDEO_MENTOR}}` | `mentor` (video) | **Required.** Screen recording of *Mentor*, landscape, ~48 s, ~59 MB, demo data only. | **Trim** to one 8–10 s segment, then encode → `assets/video/mentor.mp4` |
| `{{POSTER_MENTOR}}` | `mentor-poster` | Adrian's still (fallback only). | **Regenerate** from the first frame of the trimmed video → `assets/video/mentor-poster.webp` |
| `{{VIDEO_METAHUMAN}}` | `metahuman` (video) | **Required.** ~6 s loop of Adrian's MetaHuman, landscape source. | No trimming. **Crop to vertical 3:5** around the face (see "Video trimming"), scale to 600 × 1000, encode → `assets/video/metahuman.mp4` |
| `{{POSTER_METAHUMAN}}` | `metahuman-poster` | **Required.** Adrian's own landscape still of his MetaHuman, front-facing, face centred, on a dark grey background. **This is the poster — do not regenerate it from the video.** | Crop to vertical 3:5 using the full height, centred on the face; scale down to max 600 × 1000 (never up); webp quality 82 → `assets/video/metahuman-poster.webp` |
| `{{CV_PDF_ES}}` | `cv-es` | **Required.** Adrian's CV in Spanish. | Copy → `assets/cv/Adrian_Cornejo_CV_ES.pdf` |
| `{{CV_PDF_EN}}` | `cv-en` | *Optional.* Adrian's CV in English. If missing, the English site also links the Spanish CV. | Copy → `assets/cv/Adrian_Cornejo_CV_EN.pdf` |
| `{{OG_BASE}}` | `og_image` | **Required.** Same B&W photo as the hero but with much more empty background above the head (figure in the lower half). Only used to generate the OG image. | Keep in `_source/` (not served) |
| `{{OG_IMAGE}}` | — | Social share image, **generated** from `{{OG_BASE}}` (see "OG image"). | → `assets/img/og.jpg` |
| `{{PORTFOLIO_REPO}}` | — | Text: **`Adrian52507/portafolio`** (remote `https://github.com/Adrian52507/portafolio.git`). | Use this value. If `git remote get-url origin` says something different, ask Adrian which one is right. |

### Video trimming and encoding

- **Encoding (all three):** H.264 `.mp4`, `-crf 28`, `-preset veryfast`, `-an` (no audio), `-movflags +faststart`, `-pix_fmt yuv420p`, 30 fps max. Landscape videos max 1280 px wide; each output **≤ 3 MB** (raise CRF up to 32 if needed). Never upscale.
- **Choosing the segment (fingidazo, mentor):**
  1. Pick **one continuous 8–10 s segment** that shows the product doing something: an interaction, a screen change, a result appearing. Avoid idle screens, loading spinners, the mouse wandering, or anything that looks like personal data.
  2. To choose, export a contact sheet (one frame every 2 s, with timestamps) and look at it, and use ffmpeg scene-change scores (`select='gt(scene,0.1)'`) to find where the action is.
  3. Prefer a start and end where the screen is calm and similar, so the loop restarts without a hard jump.
  4. **Adrian has already chosen the start points, so skip steps 1–3 and use these:**
- **Segments to use:**
  - `TRIM_FINGIDAZO = 00:41 → 10 s` (00:41–00:51). If the video ends before 00:51, use 00:41 to the end; if that leaves less than 6 s, tell Adrian before choosing anything else.
  - `TRIM_MENTOR = 00:05 → 10 s` (00:05–00:15).
- **MetaHuman crop:** look at the first frame, find the horizontal centre of the face, and crop the full height to a 3:5 window centred there (clamped to the frame), so the face stays centred in the vertical card. Apply the same crop to the whole clip.
- **Posters:** Fingidazo and Mentor posters come from frame 0 of the trimmed video. The MetaHuman poster is Adrian's own still (see table); a small jump when its video starts is accepted.
- **Report back:** after processing, list for each video the segment or crop used, the final duration and the file size, and save the contact sheets in `_source/`, so Adrian can ask for a different segment.

### Other media rules

- **Hero paper colour:** sample the background of `hero-back` (average of the four corner regions) and use that colour as the hero paper (`--paper`) instead of a fixed #dadada, so the photo melts into the page with no visible band. Derive the stats-frame grey from it (about 6 % darker). Put a comment next to `--paper` with the sampled value.

- **Pixel avatar rules:** never convert the avatar to webp/jpg, never resample it smoothly. Scale it only in CSS by an **integer factor** (×4 on desktop/tablet, ×4 or ×5 on phone, whichever fits) with `image-rendering: pixelated` (plus `crisp-edges` fallback), and snap its position to whole pixels so no pixel blurs.
- All photos and videos are shown **grayscale** (`filter: grayscale(1)`) to keep the page monochrome. Project and MetaHuman videos fade to full colour on hover (desktop) or while their slide is active and in view (touch).
- **Project videos:** `<video muted playsinline loop preload="metadata" poster="…">`, never with controls or sound. Only the active slide's video plays, and only while the card is in the viewport (IntersectionObserver); the others stay paused on their poster and use `preload="none"` until they are next. If a video fails to load, show its poster as a still image.
- Every `<img>` gets real `width`/`height` attributes (no layout shift), meaningful `alt` text, and `loading="lazy"` except `{{HERO_BACK}}`, which gets `fetchpriority="high"` and a `<link rel="preload">`.
- The hero photo will not match a reference exactly, so expose its framing as CSS variables at the top of the stylesheet, with a comment: `--hero-pos` (object-position), `--plate-x`, `--plate-y` (brand-plate position as % of the photo box), so they can be tuned in one place.

**Fonts:** Inter Tight (variable, 100–900) for UI and body. Archivo with a variable width axis (wdth 62–125) for the display title, the name plate and the bar labels. Self-host latin woff2 if possible; otherwise load from Google Fonts: https://fonts.googleapis.com/css2?family=Inter+Tight:wght@100..900&family=Archivo:wdth,wght@62..125,100..900&display=swap

## Links and personal data

- Email: `cornejoadrian02@gmail.com` (mailto)
- GitHub: https://github.com/Adrian52507
- LinkedIn: https://linkedin.com/in/adrian-cornejo-palacios
- El Fingidazo: https://el-fingidazo.vercel.app/
- All external links open in a new tab with `rel="noopener"`.

## Language — Spanish / English

The whole site is bilingual. **Spanish is the default**; English is the second language. The English copy is the one written throughout this prompt; the Spanish copy is in the table below.

- **Which language loads:**
  1. `?lang=es` or `?lang=en` in the URL wins (so Adrian can send recruiters a direct English link), and is saved.
  2. Otherwise, a choice the visitor made before (saved in `localStorage`, wrapped in try/catch).
  3. Otherwise, the browser language: if the first entry of `navigator.languages` starts with `es`, Spanish; if it is any other language, English.
  4. If none of that is available, Spanish.
  - No IP or geolocation lookups.
- **Switch:** a small segmented "ES / EN" pill in the same style as the nav (white pill, black sliding pill on the active language), 36u tall.
  - Desktop/tablet: just left of the clock.
  - Phone: in the header, between the nav pill and the Contact square button.
  - It has `aria-label="Language / Idioma"`, and each option is a button with `aria-pressed`.
- **Switching:** happens instantly, without reloading the page.
  - Text crossfades in 150 ms (instant under reduced motion).
  - `<html lang>`, `<title>` and the meta description are updated.
  - `history.replaceState` sets `?lang=`.
  - The statement's word spans are rebuilt and its fill progress recalculated.
  - The relative time ("2d ago") and the clock are re-formatted with the new locale.
  - The "Download CV" link changes file.
- **Implementation:**
  - Every visible string has a key (`data-i18n="key"`, `data-i18n-aria` for labels and alt text).
  - All copy lives in one `STRINGS = { es: {…}, en: {…} }` object at the top of the script, so Adrian can edit wording in one place.
  - The HTML ships with the Spanish text, so the page reads correctly without JS.
- **Layout safety:** Spanish strings are longer. Pills, buttons and the nav must size to their content, and nothing may overflow or wrap awkwardly in either language at any tested width.
- The forced desktop line breaks of the statement apply to English only; Spanish wraps naturally.
- **OG / social tags stay in English** (crawlers don't run JS, and links are mostly shared with international recruiters).

| Key / where | English | Español |
|---|---|---|
| Nav | Home · About · Work · Contact | Inicio · Sobre mí · Proyectos · Contacto |
| Contact button | Contact ↗ | Contacto ↗ |
| Clock line 1 | Lima, Peru | Lima, Perú |
| About label | ● (ABOUT) | ● (SOBRE MÍ) |
| About copy | Software developer building full-stack web apps, / 3D experiences and AI-powered tools. | Desarrollador de software que crea apps web full-stack, / experiencias 3D y herramientas con IA. |
| CV button | Download CV ↗ | Descargar CV ↗ |
| Dark stat card | Projects shipped | Proyectos lanzados |
| Now building | ● Now building · Last commit · {time} · View repo ↗ | ● En construcción · Último commit · {time} · Ver repo ↗ |
| Open to work pill | Open to work | Disponible |
| Availability | Available for / ● remote roles | Disponible para / ● roles remotos |
| Featured | Featured · Internal | Destacado · Interno |
| Bar | LIMA, PE · SCROLL DOWN · OPEN TO REMOTE | LIMA, PE · DESLIZA · ABIERTO A REMOTO |
| Statement | (as written in "Bar and statement") | Creo que el buen software es más que código que funciona. Es una experiencia que la gente recuerda. Cada proyecto que construyo está diseñado para resolver problemas reales, sentirse natural y crecer con quienes lo usan. |
| Sub-line | Full-stack development, from / idea to deployment. *Built to ship, built to last.* | Desarrollo full-stack, de la / idea al despliegue. *Hecho para lanzarse, hecho para durar.* |
| Panel heading | Turning / Ideas Into / Working / Products | Convierto / Ideas en / Productos / Reales |
| Panel paragraph | Web apps, multiplayer games, AI agents and data pipelines — designed, built and deployed end to end, and used by real people. | Apps web, juegos multijugador, agentes de IA y flujos de datos — diseñados, construidos y desplegados de punta a punta, y usados por personas reales. |
| Stack card | Tools & technologies | Herramientas y tecnologías |
| Credentials card | Certifications | Certificaciones |
| MetaHuman caption | MetaHuman · built from my own 3D scan | MetaHuman · creado desde mi propio escaneo 3D |
| Footer links | Email ↗ · GitHub ↗ · LinkedIn ↗ | Correo ↗ · GitHub ↗ · LinkedIn ↗ |
| `<title>` | Adrian Cornejo — Full-Stack Developer | Adrian Cornejo — Desarrollador Full-Stack |
| Meta description | (as in "Meta") | Desarrollador full-stack en Lima, Perú. Apps web, experiencias 3D/interactivas y automatización con IA. Disponible para roles remotos. |

Anything not in this table (the big "FULL STACK" title, "LIMA, PE", "ES · EN · FR", tech names, the laurel badge, the credentials note, project names and stacks) stays the same in both languages. Translate any string missing from the table in the same tone, and list those translations in the final summary so Adrian can review them.

## Meta

- `<title>`: "Adrian Cornejo — Full-Stack Developer"
- Meta description: "Full-stack developer in Lima, Peru. Web apps, 3D/interactive experiences and AI automation. Open to remote roles."
- Open Graph + Twitter card tags (`summary_large_image`) using `{{OG_IMAGE}}`. `og:image`, `og:url` and `twitter:image` must be **absolute URLs** on the GitHub Pages site: `https://adrian52507.github.io/portafolio/` (so `og:image` = `https://adrian52507.github.io/portafolio/assets/img/og.jpg`). Keep that base URL in one place with a comment saying to change it if a custom domain is used later. All other asset paths in the page stay **relative** (no leading `/`), because the site is served from the `/portafolio/` subpath.

## OG image (generated, not AI)

Compose `assets/img/og.jpg` (1200 × 630, JPEG quality 85, under 300 KB) from `{{OG_BASE}}` and real text, using the page's own fonts:

- Background: the average colour of `{{OG_BASE}}`'s empty background (sample the left and right margins at the height of the head), not the hero paper.
- Photo: crop `{{OG_BASE}}` from about 8 % of the image height **above the top of the head** down to the bottom edge (head-to-hip framing; detect the head top as the first row where dark pixels appear in the central third). Scale that crop to 630 px tall and place it on the right, with the head centred around x ≈ 880. Mask its left, right and top edges to transparent over 10 %, so it melts into the flat background with no visible seam; the bottom edge stays cut by the frame.
- Left block, vertically centred, x = 72: the two-part triangular "A" mark (28 px), then "ADRIAN CORNEJO" in Archivo wdth 104 %, weight 640, 64 px, tracking −0.04em, #111; below it "Full-Stack Developer" in Inter Tight 500, 30 px, #111; below that "Lima, PE · Open to remote" in Inter Tight 500, 20 px, #111 at 55 %.
- Small "↗ github.com/Adrian52507" bottom-left, 16 px.
- Build it from a temporary `og.html` rendered with a headless browser screenshot (Playwright or Puppeteer via `npx`, ask before installing). If no headless browser is possible, compose it with ffmpeg/ImageMagick and the downloaded font files. Delete the temporary files afterwards and keep the generator script in `_source/` so the image can be regenerated.
- Inline SVG favicon: the two-part triangular "A" mark (see Brand plate), black on #dadada.

---

## Scale

- **Desktop (≥ 1200px):** drawn on a 1440-px artboard. `u = min(100vw / 1440, 1.333px)`. Above 1920px the content stays centred; backgrounds stay full-bleed.
- **Tablet (761–1199px):** drawn on an 834-px artboard. `u = min(100vw / 834, 1.2px)`, content centred, backgrounds full-bleed.
- **Phone (≤ 760px):** `u = 100vw / 390`.
- Every size below is in u for the layout it belongs to. **Readability floor:** no text renders below 11px at any width (`max(Nu, 11px)`), and tap targets are at least 44 × 44px on touch layouts.
- **Desktop heights:** hero 938u, bar 111u, statement 170u top / 230u bottom padding, trust section ends with 150u before the footer.

## Colours and type

- **Colours:**
  - hero paper `--paper`: sampled from the hero photo's background (see Media; fallback #dadada), ink #111
  - lower sections pure black #000
  - text #f4f4f4
  - statement grey #6f6f6f filling to #f4f4f4
  - hairlines white at 14–28 %
- **"FULL STACK":** Archivo at width 104 %, weight 640, 90u (desktop), line-height 0.86, tracking −0.06em.
- **UI:** Inter Tight, 13–15u, weight 500.

---

## Desktop hero (1440 × 938u)

- **Photo `{{HERO_BACK}}`:** placed at left 338u, top −86u, 764 × 1024u, object-fit cover, `object-position: var(--hero-pos)`. The left and right edges are masked to transparent over 6 %, so it melts into the paper. Adrian's head top sits about 85u from the top, and his back is cut off by the hero's bottom edge.
- **Nav:** at (57, 61). A white segmented pill, 48u tall, with **Home** (active, on a black sliding pill), **About**, **Work** and **Contact**. The black pill slides to the hovered link and, while scrolling, follows the section in view (scroll-spy). Links scroll smoothly to: top, statement, trust panel, footer.
- **Top right:**
  - a live local clock at (1038, 70), two lines: "Lima, Peru" / "17:56 (UTC−5)". Use `Intl.DateTimeFormat` with `timeZone: 'America/Lima'`, update every 30 s.
  - a black "Contact ↗" button (mailto), 144 × 46u, at (1237, 63)
- **Title:** "FULL / STACK" at (57, 246), with "(’26)" at (339, 272). Visually hidden `<h1>`: "Adrian Cornejo — Full-Stack Developer"; the big title is decorative text.
- **About:** "● (ABOUT)" at y 458, then two lines of 14u copy, "Software developer building full-stack web apps, / 3D experiences and AI-powered tools.", then a black "Download CV ↗" button (`{{CV_PDF_ES}} / {{CV_PDF_EN}} depending on the active language`, `download` attribute) at y 584.
- **Stats frame:** at (57, 693), 341 × 208u, light grey, about 6 % darker than `--paper`.
  - A dark card: "10+ / Projects shipped", six tiny stack marks in two rows (TypeScript, Python, SQL, Power BI, Blender, Unreal — each a short text label with a simple **generic** geometric glyph; no official logos), and a white "GitHub ↗" bar.
  - A **"Now building" card** for *Pixel Me*, an upcoming chat bot where Adrian's pixel avatar answers visitors' questions about him (to be built in a later phase — do **not** build the chat now).
    - Background: radial gradient #3a3a3a (centre, behind the head) → #161616 (edges), plus a very faint 4px pixel grid (white 4 %), so the avatar's black outline and dark hair stay readable.
    - Top: "● Now building" (pulsing dot) and a hairline.
    - Centre, slightly above middle: `{{AVATAR_IDLE_1}}` at an integer scale (see pixel avatar rules), **grayscale by default**. It bobs 1 avatar-pixel up and down every 1.2 s using `steps()` timing (no smooth easing). If `{{AVATAR_IDLE_2}}` is set, it blinks: swap to frame 2 for 140 ms every 3–5 s (randomised).
    - Bottom right: "Pixel Me · chat (beta)" / "Last commit · 2d ago" over a dark bottom gradient.
    - "Last commit" is fetched client-side from `https://api.github.com/repos/{{PORTFOLIO_REPO}}/commits?per_page=1` and shown as relative time (`Intl.RelativeTimeFormat`, e.g. "today", "2d ago", "3w ago"). Cache it in `sessionStorage` (wrapped in try/catch). If the request fails or is rate-limited, show "View repo ↗" instead. Never block rendering on it.
    - For now, the whole card is a link to `https://github.com/{{PORTFOLIO_REPO}}`. Give it `data-chat-trigger` and keep its markup and JS isolated (one small `initNowBuilding()` function) so the future chat can take over the click without restructuring the page.
    - Hover (desktop) / in view (touch): the avatar fades to full colour — the only colour accent on the page — a small "↗" appears top right, card lifts 2px.
    - Reduced motion: no bob, no blink, avatar shown in colour.
- **Right column:**
  - a frosted "Open to work" pill with a black check disc at (1216, 258)
  - "Available for / ● remote roles", right-aligned, at y 442 (the dot pulses softly), with a small grey third line below: "ES · EN · FR"
  - **Featured project carousel:** "Featured / [project name] / [stack]" with a landscape media card, 196u tall (≈ 16:10), showing the project's video with `object-fit: cover; object-position: center` and **no extra zoom** (the UI must stay readable), a frosted round arrow button, and a progress line (white at 35 % with a white fill). The progress line is the carousel timer: it tracks the active video's `currentTime / duration`; when the video reaches its end, the card cross-fades to the next project (fallback: 8 s if the duration is unknown). Slides:
    1. El Fingidazo — [Next.js · R3F] — `{{VIDEO_FINGIDAZO}}` / `{{POSTER_FINGIDAZO}}` → arrow links to el-fingidazo.vercel.app
    2. Mentor — [AI agent · RAG] — `{{VIDEO_MENTOR}}` / `{{POSTER_MENTOR}}` → internal tool, no public link: the arrow button is replaced by a small "Internal" tag
    - With two slides, show two small dots under the card as indicators.
    - Hover pauses the timer. Clicking the progress line area jumps to the next slide.
- **Name plate:** a dark glass plate over his back, positioned inside the photo wrapper at `left: var(--plate-x); top: var(--plate-y)` (default ≈ (532, 444) on the artboard), 364 × 73u, with a two-part triangular mark that reads as an "A", "ADRIAN CORNEJO" in wide Archivo 21u (reduce to 19u if it doesn't fit), and a two-line menu icon. Because it lives inside the photo wrapper, it stays on the jacket at every breakpoint.
- **Stack row:** a row of white text wordmarks, "React · Next.js · Three.js", at y 864 across the jacket (plain text, not logos).

## Bar and statement

- **Bar:** black, 111u, with a bottom hairline. "LIMA, PE" on the left ("PE" in grey), "SCROLL DOWN" with a white round arrow in the middle (clicking it scrolls to the statement), and "OPEN TO REMOTE" (grey) on the right. Wide Archivo, 14u.
- **Statement (id `about`):** centred, 830u wide, 52u, line-height 1.33, tracking −0.028em. On desktop, five lines: "I believe great software is more than / code that works. It's an experience / people remember. Every project I build is / designed to solve real problems, feel / effortless, and grow with its users." (Use `<br class="br-desk">` for these breaks and hide them below 1200px; tablet and phone wrap naturally.)
  - Every word is its own span. As the section scrolls through the viewport, the words fill from grey #6f6f6f to white in reading order, with a 3-word soft edge.
- **Sub-line:** two lines, 23u, 150u below: "Full-stack development, from / idea to deployment. *Built to ship, built to last.*" (the last sentence in grey).

## Work & skills panel (id `work`)

- **Panel:** 1302 × 429u, centred, with a 1px border (white at 28 %) and a 14u radius.
- **Left:** "Turning / Ideas Into / Working / Products", 55u, weight 500, plus a three-line 14.5u paragraph: "Web apps, multiplayer games, AI agents and data pipelines — designed, built and deployed end to end, and used by real people." Two frosted glass icon tiles (88u, 12u radius, backdrop blur) overlap the heading: an asterisk tile top right of line 1, and a four-dot tile over "Products".
- **Cards:** 326u tall, 8u radius.
  - **Stack card**, light grey #d6d6d6, 293u: eight round 48u discs in a 4 × 2 grid, each a black disc with a white two/three-letter monogram (TS, PY, SQL, R3F, NX, BI, BL, UE) — inline SVG/CSS, no images, no official logos; full name in a `title` tooltip and `aria-label`. Then "15+" at 43u and "Tools & technologies".
  - **Credentials card**, #262626, 298u: "4 / Certifications", an SVG laurel badge "SEEDS FOR THE FUTURE · 2022" and a two-line note: "Huawei Seeds for the Future · HackerRank: Problem Solving, SQL, REST API (Python)." Links to LinkedIn.
  - **MetaHuman card**, 200u, video `{{VIDEO_METAHUMAN}}` with poster `{{POSTER_METAHUMAN}}` (same video rules: muted, looped, plays only while the panel is in view, poster under reduced motion or on failure, grayscale → colour on hover), cut off by the panel's right edge, with a small caption bottom-left: "MetaHuman · built from my own 3D scan".

## Footer (id `contact`)

- Black, after the trust section's bottom padding, with a top hairline (white 14 %), 96u tall on desktop.
- Left: "© 2026 Adrian Cornejo Palacios". Right: "Email ↗ · GitHub ↗ · LinkedIn ↗", Inter Tight 14u, links underline on hover.

---

## Motion

- **Load (Web Animations):**
  - The title lines rise out of their masks, 1 s, staggered 110 ms.
  - The photo rises 40px, un-blurs and fades in over 1.3 s.
  - The nav drops in.
  - Every other hero element rises 14px and fades in, staggered 55 ms.
  - The featured progress line starts drawing.
  - "10+" counts up over 1.4 s.
- **Scroll:**
  - The hero photo drifts at 0.12× scroll (0.06× on phone).
  - The statement fills grey → white.
  - When the panel enters, "15+" and "4" count up and the stack discs pop in (scale 0.4 → 1, staggered).
  - Nav pill follows the section in view.
- **Hover (only inside `@media (hover: hover) and (pointer: fine)`):**
  - Nav pill slide.
  - Buttons lift 1px.
  - Panel cards lift 6u.
  - The featured video zooms slightly (scale 1.04), goes to colour, and its arrow rotates 45°.
- **Touch:** no hover-only content. Active states use `:active` (slight scale 0.98). The featured carousel can be swiped left/right.
- **Reduced motion:** no transitions, no carousel autoplay and no video autoplay (show slide 1's poster with manual arrows; a small play button starts the video on demand), everything shown filled and counted.

---

## Tablet (761–1199px, 834-u artboard)

- **Header (y 36):** nav pill at x 40, 48u tall. "Contact ↗" 144 × 46u right-aligned 40u from the edge. The clock sits right-aligned 20u to the left of the button; hide it if the viewport is under 900px.
- **Hero (~ 980u tall):**
  - Title "FULL / STACK" at (40, 150), 84u, with "(’26)" after line 1.
  - Photo on the right: left 360u, top −40u, 520 × 700u (bleeds past the right edge, clipped by the hero), masked to transparent over 10 % on the left and 12 % at the bottom, so it fades into the paper instead of hard-cutting.
  - "● (ABOUT)" at y 380, copy at y 410 (left column max 300u wide, so no text touches the face), "Download CV ↗" at y 480.
  - Name plate on the back, scaled to 300 × 60u, name at 17u. Stack row at the photo's bottom, 13u.
  - Lower row at y 720: stats frame at x 40 (341 × 208u); right block at x 420, 374u wide: "Open to work" pill and "Available for ● remote roles" on one line, then the featured carousel card (image 160u tall).
- **Bar:** 90u, labels 12u.
- **Statement:** 680u wide, 40u, padding 130u top / 170u bottom. Sub-line 19u, 110u below.
- **Panel:** 754u wide, 32u padding. Row 1: heading (48u, tiles 72u) on the left, paragraph on the right aligned to the heading's bottom. Row 2: the three cards in one row (stack 250u, credentials 250u, MetaHuman 220u cut off by the panel's right edge, like desktop), 300u tall.
- **Footer:** same as desktop, 80u tall.
- Short landscape viewports (e.g. phone in landscape, 844 × 390) also use this layout; nothing may be clipped vertically.

## Phone (≤ 760px, 390-u artboard)

- **Layout:** one column, 20u side margins.
  - **Header:** nav pill on the left (44u tall, 12u labels), "Contact" becomes a 44u square black arrow button on the right. The clock becomes one small line above the title: "Lima, PE · 17:56".
  - **Title** 64u, then the (ABOUT) copy and Download CV.
  - **Photo:** full-bleed 390 × 470u, no side masks, soft fade at top and bottom into the paper, object-position keeps the head and shoulders in frame. The name plate (≈ 280 × 56u, name 15u) and the stack row sit over it.
  - Then "Open to work" pill + "Available for ● remote roles" side by side, the stats frame (full width, two columns), and the featured carousel card (full width, image 200u, swipeable).
- **Bar:** 64u. "LIMA, PE" left, round arrow only (no "SCROLL DOWN" text) centre, "OPEN TO REMOTE" right, 11u.
- **Statement:** 28u, 350u wide, padding 90u / 120u. Sub-line 16u, 70u below.
- **Panel:** full width minus margins, 20u padding. Heading 40u with the asterisk tile (64u); the four-dot tile is hidden. Paragraph below. The cards become a horizontal **scroll-snap carousel inside the panel** (each card 260 × 320u, 12u gap, the next card peeking), with small dot indicators. Only this inner track scrolls sideways; the page never does.
- **Footer:** stacked, centred: links first (44px tap targets), copyright last. Respect `env(safe-area-inset-bottom)`.
- **Overflow:** `overflow-x: clip` on `html, body`; nothing wider than the viewport.

---

## Accessibility and quality

- Semantic landmarks (`header`, `nav`, `main`, `section`, `footer`), visible `:focus-visible` rings (white 2px outline on black, black on paper), contrast AA for all real text (the grey statement words are decorative-in-progress and end white).
- Counters and the word-fill are progressive enhancement: without JS the final values and white text show.
- No real company logos anywhere; technology names appear only as plain text or generic monograms.

## Acceptance check

Test at **375, 390, 430, 768, 834, 1024, 1280, 1440 and 1920px** wide.

- **Hero:** on desktop it matches the reference layout, with Adrian centred and the name plate on his back. On every width, no text overlaps his head or face, and the plate stays on the jacket.
- **Statement:** the words fill to white as you scroll, on all layouts, in both languages (and still work after switching language mid-scroll).
- **Language:** a Spanish browser gets Spanish, any other gets English, `?lang=` overrides, the ES/EN switch changes everything without reload and is remembered; nothing overflows in either language.
- **Panel:** the counters and stack discs animate in when it appears; on phone the cards swipe inside the panel.
- **Now building:** the pixel avatar is perfectly crisp (no blurry edges) at every width, bobs and blinks, turns to colour on hover/in view; the card links to the repo and shows the last-commit time, or "View repo ↗" if the GitHub API is unavailable.
- **Featured:** the carousel cycles with the progress line, pauses on hover, swipes on touch, plays only the active video while in view, and falls back to the poster if a video fails.
- **Responsive:** no horizontal scroll at any tested width, no text under 11px, all tap targets ≥ 44px on touch.
- **Assets:** every required file was found by name and processed; originals are in `_source/` (git-ignored); posters are webp; the avatar PNGs are byte-identical to the originals; `og.jpg` exists and the `og:image` URL is absolute.
- **Content:** no real brand logos.
