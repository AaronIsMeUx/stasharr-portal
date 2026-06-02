# Docker Usage for Stasharr Portal

This repo installs Stasharr Portal using Docker Compose.

## Start the stack

```bash
cd ~/stasharr
docker compose up -d
```

## Stop the stack

```bash
cd ~/stasharr
docker compose down
```

## View container status

```bash
docker compose ps
```

## View logs

```bash
docker compose logs -f
```

## Stash URL on macOS

If `Stash.app` is running locally at `http://localhost:9999`, use this URL inside Stasharr:

```text
http://host.docker.internal:9999
```

That is required because Docker containers on macOS cannot use `localhost` to reach the host machine.

## Confirm compose file validity

```bash
docker compose config
```

## Notes

- Keep your `~/.stash` backup safe.
- This repo stores only the install configuration, not your Stash database.
- If you change `compose.yaml`, rerun `docker compose config` before committing.
