# ICSO website — Node/Express rebuild

## Run it
```
npm install
npm start
```
Then open http://localhost:3000 — no separate local-server workaround needed like the old `fetch()`-based include.js (that only worked over http, not file://). Header/footer are now real server-side includes (EJS partials), rendered once per request.

## What changed
- **Express + EJS** instead of static HTML + client-side `fetch()` includes. Nav/footer live in `views/partials/`, one source of truth, no CORS/file:// gotcha.
- **All page text is untouched** — every paragraph, heading, FAQ answer, bio, and committee entry is copied verbatim from your original files.
- **One bug fixed**: `support/index.html` had a stray `<<section>` (double angle bracket) that would have broken rendering — corrected to `<section>`.
- **Intro splash**: full-screen animated ICSO logo reveal on first load each browser session (uses `sessionStorage` so it doesn't replay on every internal link click — only once per visit). Respects `prefers-reduced-motion`.
- **Scroll reveals**: cards, section heads, and images fade/rise into view via `IntersectionObserver` as you scroll (`.reveal` class + `initReveal()` in `main.js`).
- **Sticky header** now shrinks slightly and gains a shadow once you scroll.
- **Count-up animation** for "N years at Imperial" / "N years of ICSO" instead of the number just appearing.
- **Nav underline, card lift, button hover/press states** — small motion polish throughout, all reduced-motion safe.
- Asset/script paths are now root-relative (`/css/style.css`, `/assets/...`) instead of the old `../` relative paths, so nothing breaks moving between routes.

## What I could NOT migrate
- **`/concerts`** — this page was linked from your nav and footer but its HTML was never included in what you gave me, so there's no source text for me to move. The route currently 404s with a note. Send me that file and I'll wire it in with the same treatment.
- **Images** — none of the actual image files (`assets/*.jpg`, etc.) were uploaded, only the HTML/CSS referencing them. Drop your real images into `public/assets/` using the same filenames referenced in the code (e.g. `ghent.jpg`, `war-requiem-2026.jpg`, `destiny-and-dreams-spring-2022.jpg`, `ghent-group.jpg`, `socials.jpeg`, `strings-rach-prok-2025.jpg`, `icso-logo.jpg`) and they'll appear immediately — no code changes needed.

## Ideas if you want to go further
- Swap the intro logo animation for an actual short video/audio sting (a few bars of a past concert) for stronger brand recall.
- Add a lightweight photo lightbox/gallery component for past concerts.
- Consider a CMS-lite approach (a `data/committee.json` file) so next year's committee/exec can be updated without touching code.
- Add Open Graph meta tags per page for nicer link previews when shared on Instagram/WhatsApp.
