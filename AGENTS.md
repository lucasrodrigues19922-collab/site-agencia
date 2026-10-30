# Base44 sandbox notes

## What this project is
A single-file static landing page: `index.html` only. No build step, no backend,
no package manager, no database, and no external credentials. Tailwind, Font
Awesome and Google Fonts are pulled from public CDNs at runtime by the page.

## Running it
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
The `web` service runs `python -m http.server 3000` on the `python:3.12-slim`
image with the repo bind-mounted read-only at `/app`, so edits to `index.html`
appear on the next page load — never rebuild an image to see a change.
Public entry point: host port 3000. Healthcheck curls `/` with `urllib`.

## Verifying
- `curl -s -o /dev/null -w '%{http_code}' http://localhost:3000/` → `200`
- The served HTML must be the repo's own `index.html` (47 KB, title
  "Lucas Rodrigues | Estratégia, Projetos Digitais & Negócios").

## Known gaps in the repo
- **Hero photo missing.** `mentor_bg.jpg` (the official Lucas Rodrigues photo)
  was never committed — it only existed in the environment the page was
  exported from. Its `<img>` is therefore commented out inside `#hero` so the
  browser does not request a 404; the hero shows its own dark gradients
  (`bg-[#0B0F17]` + `.hero-bg-overlay` / `.hero-bg-side`) instead. To restore
  the photo: drop `mentor_bg.jpg` next to `index.html` and uncomment that
  `<img>`. `og:image` still points at `mentor_bg.jpg`, so social previews need
  the same file.
- The `/_aistudio-iframe.js` script tag (an artifact of the export tool) was
  removed — the file never existed in the repo and the page never used it.
