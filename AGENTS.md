# Base44 Dev Environment

## Project Overview
Static HTML website — a FOREX day trade simulator (Portuguese). No backend, no build system, no package manager. Pure static HTML/CSS/JS files served directly.

## Architecture
- nginx serves the static files from the repo root on port 3000 (mapped to container port 80)
- A custom `nginx.base44.conf` runs the worker as `root` because the sandbox repo directory has restrictive permissions (mode 700) that the default `nginx` user cannot traverse
- No external credentials or secrets required — the app fetches public exchange-rate APIs (`economia.awesomeapi.com.br`) from the browser at runtime

## Running
```
docker compose -f docker-compose.base44.yml up -d
```

## Verification
- `curl http://localhost:3000/` should return the `index.html` page containing "Simulador"
- Subpages: `about.html`, `services.html`, `project.html`, `cad.html`, `eur.html`, `gbp.html`, etc.
- Healthcheck: wget to `127.0.0.1:80/index.html` (must use IPv4 — BusyBox wget resolves `localhost` to IPv6 `::1` which nginx doesn't listen on)

## Notes
- Static files are bind-mounted read-only, so edits to HTML/CSS/JS are immediately visible on browser refresh — no rebuild needed
- The `games/` directory contains SNES ROM files and an emulator loader (`loader.js`)
