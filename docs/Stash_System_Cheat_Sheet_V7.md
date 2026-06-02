**Stash Media Server — Master Cheat Sheet**

*Aaron's Mac Mini M4 Pro  ·  Last updated: June 2, 2026  ·  V7 — Docker Portal Edition*

Fields highlighted in **amber** should be updated whenever credentials change.

## **Pipeline Flow (June 2026)**

| Stash (local) | ↔ | Stasharr Portal (Docker) | ↔ | Whisparr (Docker) | ↔ | qBittorrent (local) | → | downloads | → | media | → | Stash |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |

**Note:** Prowlarr & FlareSolverr are optional. Core flow: Stash ↔ Stasharr Portal ↔ Whisparr ↔ qBittorrent.

## **Whisparr**

| Whisparr  ·  http://localhost:6969  ·  Docker container |  |
| :---- | :---- |
| Username | aaronismeux |
| Password | \[ update here \] |
| API Key | fc636ab078914351911b850ec26c0e80 |
| Auth method (config.xml) | AuthenticationMethod: None |
| Config file | \~/.config/whisparr/config.xml |
| Media / library path | /data/media |
| Downloads path | /data/downloads |
| Version | v3.3.3.683 |

## **Prowlarr**

| Prowlarr  ·  http://localhost:9696  ·  Docker container |  |
| :---- | :---- |
| Password | \[ update here \] |
| Config file | \~/.config/prowlarr/config.xml |
| Active indexers | 13 (ZkTorrent removed — failed tests) |
| Synced to | Whisparr — Full Sync |

## **qBittorrent**

| qBittorrent  ·  Native Mac app  ·  Web UI on port 8080 |  |
| :---- | :---- |
| Username | admin |
| Password | \[ update here \] |
| Host (as seen by Whisparr) | 192.168.68.62  ← NOT localhost |
| Port | 8080 |
| Category | whisparr |
| Note | Must be open for Whisparr to download. Auto-opens at login. |

## **Stash**

| Stash  ·  http://localhost:9999  ·  Native Mac app |  |
| :---- | :---- |
| Username | admin |
| Password | \[ update here \] |
| API Key | \[ update here — from Stash Settings → Security \] |
| Library path | /Volumes/MEDIA 1  (4TB)/Sparklebox |
| Scene count | \~2,963 scenes |
| Local URL | http://localhost:9999 |
| Remote URL (via Tailscale) | http://100.70.231.115:9999 |
| Note | Must be manually opened from Dock after restart. |

## **Stasharr Portal (Docker Container)**

| Stasharr Portal  ·  Docker  ·  ghcr.io/enymawse/stasharr-portal:latest |  |
| :---- | :---- |
| Local access | http://localhost:3000 |
| Docker Compose location | ~/stasharr/compose.yaml |
| Database | PostgreSQL 17-alpine |
| DB Host | postgres (internal) |
| DB Name | stasharr |
| DB User | stasharr |
| DB Password | **\[ update in compose.yaml before first run \]** |
| Stash Base URL (Portal config) | http://host.docker.internal:9999 |
| Stash API Key | **\[ copy from Stash Settings → Security \]** |
| Whisparr Base URL (Portal config) | http://host.docker.internal:6969 |
| Whisparr API Key | fc636ab078914351911b850ec26c0e80 |
| Catalog Provider | StashDB (locked after first setup) |
| Status | ✅ Deployed & running  ·  Setup wizard complete (June 2) |

## **Docker Networking (Important for Containers)**

| Host ↔ Container access |  |
| :---- | :---- |
| From macOS host | Use `localhost:PORT` (e.g., http://localhost:3000 for Stasharr) |
| From Docker container | Use `host.docker.internal:PORT` to reach host services (e.g., Stash at http://host.docker.internal:9999) |
| Between Docker services | Use service name from compose.yaml (e.g., `postgres:5432` for database) |
| qBittorrent host (for Whisparr) | Use IP address `192.168.68.62` NOT localhost (qBittorrent is native macOS app) |
| Verify connectivity from container | docker compose exec app curl -sI http://host.docker.internal:9999 |

## **FlareSolverr**

| FlareSolverr  ·  http://localhost:8191  ·  Docker container |  |
| :---- | :---- |
| Purpose | Cloudflare bypass for Prowlarr indexers |
| Config needed | None — starts automatically with Docker |

## **Tailscale (Remote Access VPN)**

| Tailscale  ·  Installed on Mac Mini \+ iPhone  ·  Free personal plan |  |
| :---- | :---- |
| Purpose | Secure remote access to Stash from anywhere — cellular, other WiFi, etc. |
| Mac Mini Tailscale IP | 100.70.231.115 |
| Protocol for Stash | HTTP — Tailscale encrypts tunnel automatically, no HTTPS needed |
| Must be running on | BOTH Mac Mini AND iPhone for connection to work |
| Mac Mini setting | Set to Start on login in Tailscale menu bar app |
| Plan | Free personal plan — supports up to 100 devices |

## **Stashy (iOS App)**

| Stashy  ·  iPhone app  ·  TestFlight (free) or App Store (paid) |  |
| :---- | :---- |
| Purpose | Browse and play your Stash library from iPhone |
| Server name | Aaron's Stash |
| Protocol | HTTP |
| Local address (home WiFi only) | 192.168.68.62:9999 |
| Remote address (via Tailscale) | 100.70.231.115:9999 |
| API Key | \[ update here — same as Stash Settings \> Security \] |
| TestFlight link | testflight.apple.com/join/KBYqHCuD |
| Note | Tailscale must be running on both iPhone and Mac Mini for remote access |

## **Drive & Path Reference**

| Paths  ·  ⚠️ Drive name has TWO spaces: "MEDIA 1  (4TB)" |  |
| :---- | :---- |
| Main media drive | /Volumes/MEDIA 1  (4TB) |
| Sparklebox root | /Volumes/MEDIA 1  (4TB)/Sparklebox |
| Downloads folder | /Volumes/MEDIA 1  (4TB)/Sparklebox/downloads |
| Media / library folder | /Volumes/MEDIA 1  (4TB)/Sparklebox/media |
| Docker sees downloads as | /data/downloads |
| Docker sees media as | /data/media |
| Remote path mapping host | 192.168.68.62 |
| Whisparr config location | \~/.config/whisparr/config.xml |
| Prowlarr config location | \~/.config/prowlarr/config.xml |

## **Startup Order After Restart (June 2026)**

| Step | Service | How | Notes |
| :---- | :---- | :---- | :---- |
| 1 | Docker Desktop | Auto | Login item — starts all containers |
| 2 | Stasharr Portal + PostgreSQL | Auto | Docker restart policy in compose.yaml |
| 3 | Whisparr | Auto | Docker restart policy |
| 4 | FlareSolverr | Auto | Docker restart policy |
| 5 | Prowlarr | Auto | Docker restart policy (optional) |
| 6 | qBittorrent | Auto | macOS login item |
| 7 | Stash | Manual | Open from Dock — required for Portal to connect |
| 8 | Browser + Stasharr | Manual | open http://localhost:3000 in any browser |

**Quick verify:** After restart, run `cd ~/stasharr && docker compose ps` — all services should show **Up**

## **Health Check — Verify Everything Is Running**

| Run this in Terminal |  |
| :---- | :---- |
| All services | cd ~/stasharr && docker compose ps |
| What you should see | At least 4 containers: stasharr_app, stasharr_postgres_data, whisparr, and optionally prowlarr/flaresolverr |
| Stasharr Portal | stasharr-portal:latest — Up X minutes — port 3000 |
| PostgreSQL | postgres:17-alpine — Up X minutes — port 5432 (internal only) |
| Whisparr | ghcr.io/hotio/whisparr — Up X minutes — port 6969 |
| FlareSolverr (if used) | ghcr.io/flaresolverr/flaresolverr — Up X minutes — port 8191 |
| If a container is down | cd ~/stasharr && docker compose up -d |
| View logs | docker compose logs --tail 50 app (or postgres, whisparr) |
| Also verify | Stash running at http://localhost:9999  ·  qBittorrent running in Dock |

## **Docker Quick Commands (docker compose)**

| Terminal commands |  |
| :---- | :---- |
| Navigate to repo | cd ~/stasharr |
| Check status | docker compose ps |
| Start all services | docker compose up -d |
| Stop all services | docker compose down |
| Restart all | docker compose restart |
| Restart one service | docker compose restart app  (or postgres, whisparr) |
| View app logs (last 50 lines) | docker compose logs --tail 50 app |
| View logs live | docker compose logs -f app |
| Get Whisparr API key | cat ~/.config/whisparr/config.xml | grep ApiKey |
| Test Stash connectivity from app container | docker compose exec app curl -sI http://host.docker.internal:9999 |
| Backup postgres database | docker compose exec postgres pg_dump -U stasharr stasharr > ~/stasharr.db.backup |

## **Known Issues & Fixes**

| Problem | Fix |
| :---- | :---- |
| **Whisparr HTTP 500 / DryIoc crash** | Check config.xml — AuthenticationRequired must be exactly 'Enabled' or 'DisabledForLocalAddresses'. Never 'DisabledLocalAddress' (missing 'es') — that value crashes Whisparr v3. |
| **Can't log in / password won't save** | Stop container → set AuthenticationMethod to 'None' in config.xml → start container → access with no password → set new password in UI (Settings → General → Security). |
| **Prowlarr won't start — config corrupt** | docker stop prowlarr → mv config.xml to config.xml.backup → docker start prowlarr. Indexers survive as they're in the database. |
| **qBittorrent connection refused** | qBittorrent must be open. Use host 192.168.68.62 — NOT localhost — because qBittorrent is native, not Docker. |
| **Whisparr 'downloading into root folder'** | Root folder (library) must be /data/media. Downloads go to /data/downloads. These must ALWAYS be separate paths. |
| **Stasharr button missing on StashDB** | Check: Developer Mode ON in Chrome, Allow User Scripts ON in TM, cross-origin set to 'Always allow' for localhost:6969. Hard refresh: Cmd+Shift+R. |
| **Red 'error looking up scene' toasts** | Stash app must be running on port 9999\. Errors clear automatically once Stash is open. |
| **Duplicate 'Add to Whisparr' buttons** | Two Stasharr versions installed. Check TM Dashboard and remove the older duplicate. (Pending fix) |

## **When to Visit Each App (Updated June 2026)**

| Quick reference — where do you actually go? |  |
| :---- | :---- |
| **Stasharr Portal** (localhost:3000) | Your main discovery + acquisition hub. Browse StashDB, track scene acquisitions, manage integrations. |
| **Whisparr** (localhost:6969) | Check download status, manually add scenes, review library imports. Whisparr is the acquisition backend. |
| **Stash** (localhost:9999) | Browse local library, scrape metadata, organize scenes, check what has been imported from Whisparr. |
| **qBittorrent** (Dock app) | Monitor torrent progress, seed status, or troubleshoot a stuck download. |
| **Prowlarr** (localhost:9696) | Rarely — only to add/remove indexers or debug why Whisparr cannot find content. Leave alone otherwise. |
| **FlareSolverr** (localhost:8191) | Set-it-and-forget-it. No direct UI needed. Used by Prowlarr for Cloudflare bypass. |
| **Tailscale** (menu bar) | Only to check connection status or troubleshoot remote access from iPhone. |
| **Stashy** (iPhone app) | Browse and watch your Stash library remotely (requires Tailscale running). |
| **Docker Desktop** (Dock) | Rarely — only if containers won't start. Normally runs silently. Use `docker compose` from Terminal instead. |

## **Emergency Password Reset**

| Prowlarr — locked out / wrong password |  |
| :---- | :---- |
| Step 1 — stop container | docker stop prowlarr |
| Step 2 — remove auth from config | sed \-i '' '/\<AuthenticationMethod\>/d' \~/.config/prowlarr/config.xml |
| Step 3 — restart container | docker start prowlarr |
| Step 4 — set new password | Go to localhost:9696 — Prowlarr will prompt you to set new credentials |

| Whisparr — locked out / wrong password |  |
| :---- | :---- |
| Step 1 — stop container | docker stop whisparr |
| Step 2 — disable auth in config | sed \-i '' 's|\<AuthenticationMethod\>Forms\</AuthenticationMethod\>|\<AuthenticationMethod\>None\</AuthenticationMethod\>|' \~/.config/whisparr/config.xml |
| Step 3 — restart container | docker start whisparr |
| Step 4 — set new password | Go to localhost:6969 — no password needed. Set new password in Settings \> General \> Security |
| Step 5 — restart again | docker restart whisparr |

## **Stasharr Portal Setup (June 2 Deployment)**

| Configuration Details |  |
| :---- | :---- |
| Deployment method | Docker Compose (`~/stasharr/compose.yaml`) |
| Git repo | github.com/AaronIsMeUx/stasharr-portal (public) |
| Setup wizard status | Completed — all 3 required integrations active |
| Catalog provider locked | ✅ StashDB |
| Stash connection | ✅ http://host.docker.internal:9999 (verified) |
| Whisparr connection | ✅ http://host.docker.internal:6969 (verified) |
| Database health | ✅ PostgreSQL 17 running and healthy |
| GitHub CI | ✅ validate docker compose config on push |
| Issue templates | ✅ bug_report, feature_request, support_request |
| Documentation | ✅ README, CONTRIBUTING, DOCKER_USAGE.md |
| Next: Optimize | Media path mapping, service interconnections, qBittorrent sync |

## **Still To Do**

| Add Stash to login items (auto-open on restart) | Pending |
| Unified Docker Compose for full stack (Stasharr + Whisparr + Prowlarr + FlareSolverr) | Planned |
| Media path optimization review | Pending |
| qBittorrent ↔ Whisparr connectivity optimization | Pending |

*Last revision: June 2, 2026  ·  Author: Aaron  ·  Reviewed for Docker Portal architecture*