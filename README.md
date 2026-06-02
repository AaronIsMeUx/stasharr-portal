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

## Using GitHub issue templates

If you want to report a problem, request a feature, or ask for help, open an issue in GitHub and choose one of these templates:

- `Bug report`
- `Feature request`
- `Support request`

These templates help capture the key details up front so the repository maintainers can respond faster.

## Notes for macOS / Apple Silicon

If your local Stash instance runs on `localhost:9999`, use this URL inside Stasharr:

```text
http://host.docker.internal:9999
```

## Docker usage

See `DOCKER_USAGE.md` for startup, shutdown, logs, and macOS Docker networking notes.

## Backup

Keep your backup copy of `~/.stash` safe. This repo stores only the install configuration.
