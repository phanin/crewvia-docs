# Staging server

Staging is the shared test copy of Crewvia. It runs the same containers and settings as production will.

| | |
|---|---|
| Address | `https://staging.crewvia.net` |
| Host | Oracle Cloud, Always Free ARM virtual machine (Ubuntu) |
| Stack | Docker Compose: `web` (Django + Gunicorn), `db` (PostgreSQL 16), `caddy` (HTTPS) |
| Settings | `config.settings.prod`, values from `~/crewvia/.env.production` on the server |
| Domain | GoDaddy DNS record for `staging` pointing at the server. Caddy gets the certificate |

## Settings that matter

The env file lives only on the server and is never committed. Names, not values:

| Setting | Purpose |
|---|---|
| `DJANGO_SECRET_KEY` | Signs sessions. Changing it logs everyone out |
| `DJANGO_ALLOWED_HOSTS`, `DJANGO_CSRF_TRUSTED_ORIGINS`, `DOMAIN` | The staging address |
| `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` | Database. Change the password in the database first, then in this file |
| `DJANGO_HSTS_SECONDS` | HTTPS-only lifetime. Low (3600) until staging is stable |
| `GUNICORN_WORKERS` | Web worker count |
| `DEFAULT_TAX_RATE` | Tax percent on new estimates. `0.00` unless set |
| `DJANGO_EMAIL_BACKEND`, `EMAIL_*`, `DEFAULT_FROM_EMAIL` | Outgoing email. See [Email delivery](email.md) |

A change to the env file only takes effect when the container is recreated: `dc up -d --force-recreate web`.

## Deploying

On the server, after a change is merged to `develop`:

```bash
cd ~/crewvia
git pull
dc up -d --build
```

`dc` is a shell alias for `docker compose` (kept in `~/.bashrc`). Migrations run when the web container starts. A pull without `--build` leaves the old code running.

## Checks

```bash
dc ps                      # all three containers running, web healthy
dc logs web --tail 50      # recent requests and errors
curl -s https://staging.crewvia.net/health/    # returns 200
```

Gunicorn prints `Control server error: Permission denied: '/home/app'` at start. It is harmless.

## Network notes

- Oracle blocks outgoing mail on port 25. Mail goes out on port 587 through an email provider.
- Only ports 80 and 443 (and SSH) need to be open to the internet.

## Still to do

- Test restoring a backup into a scratch database, and schedule a nightly backup.
- Copy backups off the server.
- Raise `DJANGO_HSTS_SECONDS` once staging has been stable.
- Cost alert in Oracle Cloud, and uptime monitoring.
- Rotate the secret key and database password on a schedule, and whenever they are shared.
