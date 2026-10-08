# Local setup

Requires Docker Desktop (or Docker Engine with Compose v2) and git.

```bash
git clone <repo-url> monsteros && cd monsteros
cp .env.example .env
```

Edit `.env` and set `DJANGO_SECRET_KEY` and `POSTGRES_PASSWORD`. To generate a key:

```bash
python3 -c "import secrets; print(secrets.token_urlsafe(50))"
```

Start the stack:

```bash
docker compose up -d --build
docker compose exec web python manage.py migrate
docker compose exec web python manage.py seed_demo
```

`seed_demo` prints a password once for the demo users. Open
<http://localhost:8000/> and sign in.

## Demo users

| Username | Role |
|---|---|
| `demo_admin` | Admin |
| `demo_sales` | Office/Sales |
| `demo_dispatcher` | Dispatcher |
| `demo_tech` | Technician |
| `demo_finance` | Finance |
| `rival_admin` | Admin of a second company, used to check data isolation |

## Everyday commands

```bash
docker compose exec web pytest                       # tests
docker compose exec web ruff check .                 # lint
docker compose exec web python manage.py check       # Django checks
docker compose exec web python manage.py createsuperuser
docker compose stop      # pause
docker compose down      # remove containers, keep the database
docker compose down -v   # remove containers AND the database
```

## Running the CI checks locally

```bash
docker compose exec web ruff check .
docker compose exec web python manage.py check
docker compose exec web python manage.py makemigrations --check --dry-run
docker compose exec web pytest
```

## Rebuilding the CSS

After adding new Tailwind classes to templates:

```bash
./tailwindcss -i app/static/src/input.css -o app/static/css/output.css --watch
```

Commit `output.css`. Do not commit the `tailwindcss` binary.

## Building this documentation locally

```bash
pip install -r requirements-docs.txt
mkdocs serve
```
