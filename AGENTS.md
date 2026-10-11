# Base44 Setup Notes

## Project Overview
This is a single static HTML file (`domain version 1.1.html`) — a personal gaming
dashboard with a live clock, Google search bar, quick-command launcher, and
categorized link cards (Subnautica, Portal, Half-Life, Steam, VR, streaming, etc.).

## How It Runs
- Served by **nginx:alpine** via `docker-compose.base44.yml`.
- The repo is bind-mounted read-only at `/usr/share/nginx/html`.
- `index.html` is a symlink to `domain version 1.1.html` so nginx serves it as
  the default page.
- nginx runs as `user root;` (overridden via `nginx.main.base44.conf`) because the
  sandbox repo directory has 700 permissions that the default `nginx` worker user
  cannot read.
- Listens on port 3000 (mapped to host 3000).

## Files Added by Base44
- `docker-compose.base44.yml` — compose stack
- `nginx.base44.conf` — server block (port 3000, serves index.html)
- `nginx.main.base44.conf` — main nginx config (runs as root for sandbox perms)
- `index.html` — symlink to `domain version 1.1.html`
- `.base44/environment.json` — Base44 metadata

## Verification
- `curl http://localhost:3000/` returns 200 with the dashboard HTML.
- Healthcheck: `wget --spider http://127.0.0.1:3000/` (uses 127.0.0.1, not
  localhost, because the container resolves localhost to IPv6 ::1 where nginx
  isn't listening).

## No External Secrets Required
This is a static site with no backend, database, or external API dependencies.
