# Day 5 - Deployment and documentation (Fri Oct 9)

Day 5 was planned for Thursday and done on Friday.

## Goals

- Make the application deployable to a server with HTTPS and backups.
- Fix the platform superuser experience.
- Complete the Milestone 1 documentation set.

## Built

- **Production stack** (`docker-compose.prod.yml`): PostgreSQL, the app on Gunicorn, and Caddy for automatic HTTPS. The database is not reachable from outside.
- **Dockerfile** now has two targets: `dev` (unchanged behaviour) and `prod` (runtime libraries only, non-root user).
- **Start-up script** runs migrations, collects static files and refreshes the roles.
- **Production settings:** HTTPS redirect, secure cookies, HSTS, security headers, hashed static files, email settings, logging. All values come from environment variables.
- **Backup and restore scripts** with 14-day retention and a typed confirmation before a restore.
- **`create_company_admin` command** creates a company and its first Admin on a server where demo data is disabled.
- **Superuser fix:** the Users menu item is hidden for accounts with no company, and the "No access" page now explains why.
- **CI:** two new steps: `manage.py check --deploy` with production settings, and a production image build.
- **Tests:** five new tests (superuser menu, both 403 messages, the new command).
- **Docs:** [Deployment](../deployment.md), [Data dictionary](../data-dictionary.md), [Security baseline](../security-baseline.md), [Milestone 1 acceptance](../m1-acceptance.md), a refreshed roadmap, and trimmed wording on the home page and Day 1.

## Decisions

- Caddy over Nginx: it gets and renews certificates by itself, so there is less to maintain.
- Backups are scripts plus cron, not a managed service, to keep the stack open source and simple. An off-server copy is still required.
- Known security gaps are listed openly on the security baseline page.
- The public documentation no longer describes the client's concept material; scope wording points to the agreement.

## Problems avoided

- `.gitignore` only covered `.env`; `.env.production` is now ignored too.
- The container health check calls the app as `localhost` over plain HTTP, so production settings allow that host and exempt `/health/` from the HTTPS redirect.

## Result

Fill in after running the checks: expected 55 tests (50 from Days 1 to 4 plus 5 new), `ruff check` clean, `makemigrations --check` with no changes.

## Next

Bring up the staging server (provider and payer to be agreed), run and test a restore, then walk through the Milestone 1 checklist. Milestone 2 models and wireframes start once scope is confirmed in writing.
