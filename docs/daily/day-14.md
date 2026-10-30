# Day 14 - Staging on Oracle Cloud (Tue Oct 27)

## Goals

- Put the app on a real server with HTTPS, so Person B and later a first client can open it from anywhere.

## Built

- **Staging server** on Oracle Cloud (Always Free, ARM), running the production Compose stack from Day 5: Django behind Gunicorn, PostgreSQL 16, and Caddy for HTTPS.
- **Domain.** `staging.crewvia.net` points at the server through a GoDaddy DNS record. Caddy gets and renews the certificate on its own.
- **Production settings** are read from `.env.production` on the server (never committed). See [Staging server](../staging.md).
- **Demo data** loaded so the app opens with customers, roles and users.

## Decisions

- Staging runs the same images and settings module (`config.settings.prod`) that production will, so a problem shows up here first.
- `DJANGO_HSTS_SECONDS` starts low (3600) so a mistake with HTTPS is easy to undo. Raise it once staging has been stable for a while.
- One short alias, `dc`, stands for `docker compose` on the server.

## Problems and fixes

- A deploy fetched new code but did not rebuild the image, so the old containers kept running. A deploy is always `git pull` and then `dc up -d --build`.
- Gunicorn prints `Control server error: Permission denied: '/home/app'` at start. It is harmless: the control socket cannot be created for the non-root user, and the app is unaffected.

## Result

Staging answers on `https://staging.crewvia.net` and `/health/` returns 200. Day 15 ran the first real deploy of new code on it.

## Still to do

- Test a database backup restore, and schedule a nightly backup.
- Copy backups off the server.
- Set a small cost alert in Oracle Cloud.
- Set up uptime monitoring.

## Next

Invoice PDF and email, then tested on staging.
