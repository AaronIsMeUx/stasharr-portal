# Contributing

Thanks for helping keep this Stasharr Portal install repo clean and reliable.

## Guidelines

- Keep changes small and focused.
- Use descriptive commit messages.
- If you add or update `compose.yaml`, verify it with `docker compose config`.
- If you modify documentation, keep the README and this file accurate.

## Branches

- Use feature branches like `feature/update-notes` or `fix/docker-compose`.
- Create a pull request for review before merging to `main`.

## Testing

Before opening a PR, run:

```bash
cd ~/stasharr
docker compose config
```

This ensures the Docker Compose file is syntactically valid.

## Reporting issues

If you find a problem, create a GitHub issue with:

- what you were trying to do
- the exact command or step you ran
- the output or error message

## Help wanted

If you want to improve this repo, consider adding:

- a `docs/` folder for operation notes
- a `scripts/` folder for useful maintenance commands
- more GitHub Actions checks for docs or config validation
