# Technology and libraries

Everything in the stack is open source.

## Runtime stack

| Layer | Choice | Version | Purpose |
|---|---|---|---|
| Language | Python | 3.12 | Application code |
| Framework | Django | 5.2 LTS | Web framework, ORM, auth, admin, migrations |
| Database | PostgreSQL | 16 | Primary data store |
| Front end | Django templates + HTMX | - | Server-rendered pages with partial updates |
| Styling | Tailwind CSS (standalone CLI) | 4.x | Utility CSS, compiled to `output.css` |
| Containers | Docker Compose | v2 | Local app and database |
| Web server | Gunicorn | 22+ | Production WSGI server (staging and production) |
| Static files | WhiteNoise | 6.7+ | Serves static files from the app |
| HTTPS proxy | Caddy | planned | Automatic HTTPS on the VPS |

## Python libraries

| Library | Purpose |
|---|---|
| `Django` | Core framework (5.2 LTS) |
| `psycopg[binary]` | PostgreSQL driver (psycopg 3) |
| `gunicorn` | Production application server |
| `whitenoise` | Static file serving |
| `django-environ` | Reads configuration from environment variables |
| `django-htmx` | Detects HTMX requests so views can return partials |
| `django-simple-history` | Change history on key models (planned for Milestone 2 models) |
| `django-filter` | Filtering list pages |

## Development and quality tools

| Tool | Purpose |
|---|---|
| `pytest`, `pytest-django` | Test runner |
| `ruff` | Linting (rules `E4`, `E7`, `E9`, `F`; migrations excluded) |
| GitHub Actions | CI on every pull request (ruff, Django checks, migration check, pytest with PostgreSQL 16) and documentation deploy |
| MkDocs + Material | This documentation site |

## Project tooling

| Tool | Purpose |
|---|---|
| GitHub | Source code, issues, project board, milestones |
| Slack | Team chat and daily updates |
| Google Meet | Calls |

## Notes

- Licenses: the libraries above use permissive open-source licenses (BSD, MIT,
  Apache 2.0, PostgreSQL License). Confirm before any redistribution.
- Deferred on purpose: Celery and Redis (notifications), WeasyPrint (PDF
  invoices), Django REST Framework (mobile app).
