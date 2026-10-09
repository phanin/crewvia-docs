# Day 1 - Repository and Docker (Sun Oct 4)

## Goals

- Stand up the repository and a Docker-based local environment.
- Decide the technology stack and how the work is split.
- Prepare the client conversation (scope, cut list, accounts).

## Built

- Django 5.2 project with split settings (`base`, `dev`, `prod`) read from environment variables.
- Docker Compose with PostgreSQL 16 (healthcheck, persistent volume) and the Django app.
- `.env.example`, `.gitignore`, README, and a `/health/` endpoint.
- Private GitHub repository with `main` and `develop` branches.
- Project board, milestones (M1 Oct 17, M2 Nov 1), and the first issues.
- Slack workspace for team updates.

## Decisions

- **Stack:** Django, PostgreSQL, HTMX, Tailwind (all open source), chosen for one language and one deployable app.
- **Project name:** MonsterOS, the client's name for the product.
- **Repository ownership:** held by the developers until the agreement's payment terms are met, then transferred.
- **Longer-term ideas:** treated as direction, not scope.

## Problems and fixes

- Old git did not support `git init -b main`; used `git init` then `git checkout -b main`.
- SSH push was rejected (no key on GitHub); switched to HTTPS with a personal access token.

## Next

Custom User, Company, and the tenant base, before the first migration.
