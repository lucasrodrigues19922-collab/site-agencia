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

## Known gaps in the repo (pre-existing, not setup issues)
- `mentor_bg.jpg` is referenced (hero background `<img>` and `og:image`) but is
  **not in the repository**. The hero falls back to its dark gradient overlay,
  and the browser logs "Failed to load img mentor_bg.jpg". Add the real image
  next to `index.html` to restore it.
- `/index.html` loads `/_aistudio-iframe.js`, an artifact of the tool the page
  was originally exported from. It is absent and returns 404; harmless, since
  the page has no dependency on it.
