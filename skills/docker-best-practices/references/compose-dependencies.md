# Compose Dependencies

## Principle

Startup ordering and readiness are different concerns. `depends_on` can express order, but readiness gating only works when the upstream service has a valid healthcheck.

## Rules

- Use plain `depends_on` when you only need startup ordering.
- Use `depends_on: condition: service_healthy` only when the upstream service defines a real executable healthcheck.
- Do not gate services on a healthcheck that depends on missing tools or unstable probes.

## Fallback

If readiness cannot be checked reliably inside the container, prefer:

- plain `depends_on`, or
- retries in the application layer where retry behavior belongs.
