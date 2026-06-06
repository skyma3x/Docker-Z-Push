# Docker container for Z-Push

This repository includes a Docker Compose configuration to run a `z-push` container based on the `skyma3x/z-push` image.

## Purpose

The container runs a Z-Push server that acts as an ActiveSync gateway for mobile devices, connecting to an IMAP/SMTP backend and optionally using an LDAP server for authentication.

## How it works

- The `z-push` service runs inside a Docker container.
- Z-Push persistent data is stored in the Docker volume `zp_state`, mounted at `/var/lib/z-push`.
- Environment variables configure the backend provider, IMAP/SMTP settings, LDAP options, timezone, and other service behavior.
- The container port `8080` is exposed on host port `7003`.

## Configuration via `compose.yml`

The `compose.yml` file defines the main service and the persistent volume.

### `z-push` service
- `image: skyma3x/z-push`
- `container_name: z-push`
- `volumes: zp_state:/var/lib/z-push`
- `ports: - 7003:8080`

### Main environment variables
- `BACKEND_PROVIDER=BackendIMAP`
- `IMAP_SERVER`, `IMAP_PORT`, `IMAP_SMTP_SERVER`, `IMAP_SMTP_PORT`
- `IMAP_SMTP_AUTH`, `IMAP_SMTP_USERNAME`, `IMAP_SMTP_PASSWORD`
- `LDAP_ENABLED`, `LDAP_SERVER`, `LDAP_DOMAIN`, `LDAP_USER`, `LDAP_PASSWORD`
- `TIMEZONE`, `TZ`
- `ZPUSH_HOST`
- `LOGLEVEL` and `LOGAUTHFAIL`
- `PHP_MAX_EXECUTION_TIME`, `PHP_MEMORY`, `PING_INTERVAL`, `RETRY_AFTER_DELAY`

> Note: replace example values (`example.com`, `example`, `password`, etc.) with your real settings.

## Starting the container

Run:

```sh
docker compose up -d
```

Then check the status with:

```sh
docker compose ps
```

## Persistent volume

The `zp_state` volume preserves Z-Push state and runtime data between container restarts.

## Customization

Edit the environment variables in `compose.yml` to adapt:
- the IMAP/SMTP server settings
- mail folders (`IMAP_FOLDER_*`)
- LDAP settings
- log level
- timezone

## Notes

- The container exposes the service on host port `7003`.
- Make sure the IMAP/SMTP server is reachable from the container.
- If you use LDAP, enable `LDAP_ENABLED=true` and configure the LDAP settings accordingly.
