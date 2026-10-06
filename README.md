# Handoff: UX Portfolio (carousel of project documents)

## Overview
A personal UX portfolio for recruiters. One fixed-viewport page: a horizontal, infinitely looping carousel of project "documents". The selected document sits in the center; its neighbours peek on both sides. A tab bar at the bottom lists all projects.

Target: a static site deployed on Vercel from GitHub repo `ggg111G/portfolio2026` (branch `main`). Vercel auto-deploys on every push.

## About the files
- `index.html` — the current working version as ONE self-contained file (compiled bundle, ~490 KB). It runs as-is and can be pushed to the repo root right now. Don't hand-edit it; it's generated.
- `source/Portfolio v2.dc.html` — the editable source of the current version. It's a "Design Component" file that needs `source/support.js` next to it to run (open it in a browser through a local server). Styling is inline, logic is a class at the bottom of the file.
- `source/Portfolio v1.dc.html` — previous version (all documents A4-style, modal for embeds).
- `source/assets/` — images. `source/image-slot.js` — drag-and-drop image placeholder used for the slide fallback.
- `CLAUDE.md` — owner's working preferences. Follow them.

Recommended first task in Claude Code: rebuild the source as plain, maintainable HTML/CSS/JS (no `support.js` runtime), matching `index.html` pixel-for-pixel, then commit to the repo. Ask the owner before choosing a framework; plain static files are enough for Vercel.

## Fidelity
High-fidelity. Recreate exactly.

## Layout
- Background `#f7f7f7`, font Karla (Google Fonts, 400/700), text `#383838`.
- **Name box**: fixed top-left, `left:24px; top:24px; width:264px`, radius 8px, background `rgba(255,255,255,.9)` + `backdrop-filter: blur(12px)`, shadow `inset 0 0 0 1px #383838, 0 0 4px rgba(0,0,0,.15)`, padding `12px 16px`. Holds name and contact links.
- **Documents**: start at `top:24px` (196px when viewport < 1180px so the name box doesn't overlap). A4-style docs are 595px wide (tweakable 420–760), white, radius 8px, border via `box-shadow: inset 0 0 0 1px #E9E9E9`, padding `24px 32px`, column gap 14px. The active document scrolls with the page.
- **Peeks** (prev/next): opacity 0.45, `scale(.86)` from center, height `min(600px, available band)`, vertically centered in the band between the top and the tab bar. Horizontal spacing is computed from each neighbour's real width with a 32px gap, so wide and A4 documents never overlap. Hidden below 780px viewport width.
- **Tab bar**: bottom. Selected tab is centered and wider; unselected tabs are fixed width. Tab surface `rgba(236,235,235,.8)` + `backdrop-filter: blur(36px)`; selected tab has a 4px coloured ring. A 5-colour blurred pattern sits behind the peeks and under the active doc (intensity 0.4).
- Tab labels are read from each document's title (kept in sync).

## Document formats
- **A4-style** (CV, Journey Orchestration, AI-Driven Design System Workflow, …): header stack = title (17px/700), year (17px/400, `#a0a0a0`), company chip (12px, `#f9f8f8` background, padding `2px 6px`). Section titles 14px/700, line-height 1.5, `#383838`. Paragraphs 14px, line-height 1.5, letter-spacing -0.491px, colour `#4f2b0a`. Journey Orchestration has a full-width header image at the top and two buttons (Case study, Prototype).
- **Wide slide** (Platform Agents): `width: min(816px, 100vw - 160px)`, aspect ratio 816/461, centered vertically when active. It shows the **live Figma presentation** in an iframe (no static image), so Figma edits appear automatically. On top: a 18% white wash, a centered white play icon, and a top-left info card (same style as the name box) with title, year, company and a Case study button (`#4f2b0a`, white 12px/700 text).

## Interactions
- Rotate the carousel with arrows, ←/→ keys, clicking a peek, or clicking a tab. Transform transition `.44s cubic-bezier(.22,.61,.36,1)`. Infinite loop.
- Hover: peeks lift, tabs brighten.
- **Opening the wide slide** (play icon or Case study): the slide itself animates from its rect to full viewport (`left/top/width/height`, `.45s` same easing, radius → 0). The iframe is always rendered at full-viewport size and scaled with `transform: scale()` to fit the card, and the scale animates alongside, so the presentation doesn't re-layout mid-animation. The info card, play icon and wash fade out; the iframe becomes interactive; a **Close** button appears top-right. Carousel navigation is disabled while open.
- **Closing** (Close or Esc): sends Figma Embed API `postMessage({type:"RESTART"}, "https://www.figma.com")`, waits 1s, then animates back and restores the saved inline styles.
- A4 docs with embeds (e.g. Journey Orchestration) open a modal sheet that inherits the doc's border, top-corner radius and shadow; embeds inside get a 1px `#E9E9E9` border.

## Figma embed
- URL: `https://embed.figma.com/deck/f15G259Ezobt6ayDfMYUAw/Designing-an-AI-Agent-Chat-Case-Study?node-id=4043-63349&embed-host=share&client-id=mIetPE84BG13ympRyhNjw9&footer=false&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1`
- Client ID `mIetPE84BG13ympRyhNjw9` (owner's Figma OAuth app). The Vercel production URL must be added as an embed origin in that app (Embed API → Add an embed origin).
- Untested: whether `RESTART` works on a Figma **presentation** (docs only cover prototypes). Test on the live Vercel URL. If it does nothing, ask the owner before removing the 1s delay.
- Rejected approaches, don't reintroduce without asking: reloading the iframe on close (long white screen), keeping two iframes (overloaded the browser), a static cover image over a reload.

## Design tokens
- Colours: text `#383838`, body copy `#4f2b0a`, secondary `#a0a0a0`, page `#f7f7f7`, chip/surface `#f9f8f8`, borders `#E9E9E9`, button hover `#67370d`.
- Radius 8px (docs, boxes), 6px (buttons).
- Blur: name box and info card 12px, tabs 36px.

## Open items
1. Push `index.html` to the repo and connect Vercel.
2. Register the Vercel URL in the Figma app, then test Close → restart.
3. Content for remaining projects. Journey Orchestration may also become a wide slide.
