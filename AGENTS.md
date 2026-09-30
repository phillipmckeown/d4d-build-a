# Base44 Dev Environment

## What this is

A three-screen static site (catalog, tool detail, hold confirmation) for a neighborhood tool lending library. No backend, no database, no external services. Pure HTML + Tailwind CSS v4 + a small amount of client-side JavaScript.

## Running it

```
docker compose -f docker-compose.base44.yml up -d
```

Serves on port 3000. The dev command (`npm run dev`) runs two processes via `concurrently`:
- **css**: `tailwindcss --watch` rebuilding `dist/styles.css` from `src/input.css`
- **site**: `serve . -l 3000` serving the static files

Dependencies install on container startup via `npm ci`.

## Quirk: serve.json and the root URL

`serve` v14 with `cleanUrls: false` shows a **directory listing** at `/` instead of serving `index.html`. The project's `serve.json` had `cleanUrls: false`, so the root URL was broken. Fixed by adding a `rewrites` rule mapping `/` → `/index.html`. This preserves `cleanUrls: false` (so `/tool` does not serve `tool.html`) while making the root URL work.

## Multi-page navigation

This is a static multi-page site, not a SPA. Pages link to each other via full page loads (`tool.html`, `confirmation.html`). Client-side route navigation tools won't work here — use full loads or `curl` to verify individual pages.

## No secrets needed

The app has no external dependencies. No environment variables or credentials are required.
