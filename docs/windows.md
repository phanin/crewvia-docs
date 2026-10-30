# Setup on Windows

The project runs in Docker, so Windows works the same as macOS once Docker is ready.

## One-time setup

1. Turn on virtualization in the BIOS if it is off.
2. Install **Docker Desktop** with the **WSL2** backend.
3. Install **Git**.
4. Recommended: install Ubuntu from the Microsoft Store (WSL) and work inside it. The project is faster there and every command below works unchanged.

## Get the code

Clone inside WSL, for example in `~/crewvia`, not under `C:\Users\...`:

```bash
git clone <repository-url> crewvia
cd crewvia
cp .env.example .env
```

The folder name sets the Docker project name, so keep it `crewvia`.

## Start and prepare

```bash
docker compose up --build -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py seed_roles
docker compose exec web python manage.py seed_demo
docker compose exec web pytest
```

Open <http://localhost:8000/>. The test run should end with all tests passing.

## PowerShell instead of WSL

Most commands are the same. Applying a patch folder differs:

```powershell
Copy-Item -Recurse -Force "$HOME\Downloads\<patch>\*" .
```

## Common problems

| Symptom | Cause and fix |
|---|---|
| `exec format error` or `no such file` for a `.sh` file | Windows changed line endings. Make sure `.gitattributes` is present, then clone again. |
| Docker will not start | Virtualization is off, or WSL2 is not installed. |
| Page loads without styling | Static files were not copied. Rebuild with `docker compose up --build`. |
| `relation "django_session" does not exist` | The database is new and empty. Run `migrate` and the seed commands. |
| Port 8000 or 5432 already in use | Stop the other program, or change the port in `docker-compose.yml`. |
| Old name in Docker Desktop | Rename the folder, run `docker compose up --build`, then remove the old containers, image and volume. |
