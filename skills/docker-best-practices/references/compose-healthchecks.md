# Compose Healthchecks

## Principle

A healthcheck must be executable inside the container image. If the command does not exist in the image, the healthcheck is invalid and can block dependent services.

## Rules

- Never assume `wget` or `curl` exists in an image.
- Validate command availability first by checking image documentation or inspecting a shell in the container.
- If no stable built-in command exists, do not force a healthcheck.

## Validation

After Compose edits, run:

```bash
docker compose config
```

Then confirm the healthcheck command is actually present in the image being used.
