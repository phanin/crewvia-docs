# Deployment

How to run MonsterOS on a server with HTTPS and backups. Used for staging first,
and the same files are used for production.

## What you need

- A small Linux server (Ubuntu 24.04 LTS). About 2 GB of memory and 25 GB of disk is enough for staging.
- A domain or subdomain whose DNS A record points at the server's IP address. Caddy needs it to get the HTTPS certificate.
- Access to the repository (a read-only deploy key or token).

!!! note "Open decision"
    Which provider hosts staging, and who pays for it, is still to be agreed.

## The stack

| Container | Role |
|---|---|
| `caddy` | Public entry point on ports 80 and 443; gets and renews the HTTPS certificate |
| `web` | Django with Gunicorn; runs migrations and collects static files at start |
| `db` | PostgreSQL 16 with a persistent volume; not reachable from outside |

Files: `docker-compose.prod.yml`, `infra/Dockerfile` (target `prod`),
`infra/Caddyfile`, `infra/entrypoint.sh`, `.env.production.example`,
`infra/backup.sh`, `infra/restore.sh`.

## First deployment

1. **Prepare the server.** Create a non-root user, enable SSH keys only, install Docker Engine and the Compose plugin from Docker's official instructions, and allow only ports 22, 80 and 443:

    ```bash
    sudo ufw allow OpenSSH && sudo ufw allow 80 && sudo ufw allow 443/tcp && sudo ufw allow 443/udp
    sudo ufw enable
    ```

2. **Get the code.**

    ```bash
    sudo mkdir -p /opt/monsteros && sudo chown $USER /opt/monsteros
    git clone git@github.com:phanin/monsteros.git /opt/monsteros
    cd /opt/monsteros && git checkout develop
    ```

    The repository is private, so the server needs access: a read-only deploy key on the repository (Settings, Deploy keys), or clone over HTTPS with a fine-grained token that has read-only access to this repository.

3. **Create the settings file.**

    ```bash
    cp .env.production.example .env.production
    chmod 600 .env.production
    python3 -c "import secrets; print(secrets.token_urlsafe(64))"   # DJANGO_SECRET_KEY
    openssl rand -hex 24                                            # POSTGRES_PASSWORD
    nano .env.production
    ```

    Set `DOMAIN`, `DJANGO_ALLOWED_HOSTS` and `DJANGO_CSRF_TRUSTED_ORIGINS` (with `https://`) to your domain.

4. **Start it.**

    ```bash
    docker compose --env-file .env.production -f docker-compose.prod.yml up -d --build
    docker compose --env-file .env.production -f docker-compose.prod.yml ps
    docker compose --env-file .env.production -f docker-compose.prod.yml logs -f web
    ```

    Wait until `web` reports healthy and the logs show Gunicorn listening.

5. **Create the first company and Admin.** The demo data command is disabled outside development, so use:

    ```bash
    docker compose --env-file .env.production -f docker-compose.prod.yml exec web \
      python manage.py create_company_admin --company "Demo Security" \
      --username demo_admin --email you@example.com
    ```

    It asks for the password. Optionally create a platform superuser for `/admin/` with `createsuperuser`.

6. **Check it.** Open `https://<your domain>/`, sign in, open Users. Open `http://<your domain>/` and confirm it redirects to HTTPS.

!!! tip "Shorter commands"
    Add `alias mos='docker compose --env-file .env.production -f docker-compose.prod.yml'` to your shell, then use `mos ps`, `mos logs -f web` and so on.

## Backups

```bash
bash infra/backup.sh
```

Writes `backups/monsteros-<timestamp>.sql.gz` and removes files older than 14 days.
Run it every night:

```bash
crontab -e
# 02:00 every day (server time)
0 2 * * * cd /opt/monsteros && bash infra/backup.sh >> backups/backup.log 2>&1
```

Also copy the `backups/` folder off the server (another machine or object
storage), and consider the provider's server snapshots. A backup on the same
disk as the database does not protect against losing the server.

### Restore

```bash
bash infra/restore.sh backups/monsteros-<timestamp>.sql.gz
```

It asks you to type `RESTORE`, stops the web container, replaces the data, and
restarts. **Test this once on staging before relying on it.**

## Updating

```bash
cd /opt/monsteros
bash infra/backup.sh
git pull
docker compose --env-file .env.production -f docker-compose.prod.yml up -d --build
```

Migrations run automatically when `web` starts. Tag each release
(`git tag m1-staging && git push --tags`) so you can return to it.

### Rolling back

```bash
git checkout <previous tag>
docker compose --env-file .env.production -f docker-compose.prod.yml up -d --build
```

If the newer version changed the database, restore the backup taken before the update.

## Problems

| Symptom | Likely cause and fix |
|---|---|
| Browser shows a certificate error | DNS does not point at the server yet, or ports 80/443 are blocked. Check `mos logs caddy` |
| `400 Bad Request` | Domain missing from `DJANGO_ALLOWED_HOSTS` |
| `403 CSRF verification failed` on sign-in | `DJANGO_CSRF_TRUSTED_ORIGINS` must include `https://<domain>` |
| Pages unstyled or `Missing staticfiles manifest entry` | `collectstatic` did not run. Check `mos logs web` for the entrypoint output |
| `web` keeps restarting | Read `mos logs web`. Usually a blank value in `.env.production` or a bad `POSTGRES_PASSWORD` |
| Password reset email not arriving | Email goes to the logs until SMTP settings are added |
