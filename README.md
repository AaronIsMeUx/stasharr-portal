# Stasharr Portal Docker Install

This repository contains a clean local install of Stasharr Portal using Docker Compose.

## What is included

- `compose.yaml` — Docker Compose configuration for Stasharr Portal and PostgreSQL

## Startup

```bash
cd ~/stasharr
docker compose up -d
```

## Stasharr UI

Open:

```bash
open "http://localhost:3000"
```

## Notes for macOS / Apple Silicon

If your local Stash instance runs on `localhost:9999`, use this URL inside Stasharr:

```text
http://host.docker.internal:9999
```

## Backup

Keep your backup copy of `~/.stash` safe. This repo stores only the install configuration.
