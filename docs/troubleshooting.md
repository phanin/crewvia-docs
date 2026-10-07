# Troubleshooting

Problems met during the build, and their fixes.

| Symptom | Cause | Fix |
|---|---|---|
| `git init -b main` fails with `unknown switch b` | Old git (before 2.28) | `git init`, then `git checkout -b main` |
| `Permission denied (publickey)` on push | No SSH key on GitHub | Use HTTPS with a personal access token, or add an SSH key |
| `Invalid username or token` on HTTPS push | Account password used, or a stale Keychain entry | Clear the cached credential and paste a personal access token as the password |
| `No module named 'apps.core'` on startup | Container started before the files existed | `docker compose restart web` |
| Browser shows nothing, `curl` says connection reset | The dev server crashed at startup | Read `docker compose logs web --tail 40`, fix, then restart `web` |
| `failed to resolve host 'db'` | The database container is not running or on a different network | `docker compose up -d`; if needed `docker compose down` then `up -d` (no `-v`) |
| Pages look unstyled | `output.css` is missing | Build the CSS (see Local setup) and commit it |
| Dashboard shows an old placeholder | A zip merge kept the old template | Overwrite the template file explicitly |
| Superuser sees every menu item | Superusers have all permissions | Sign in as a demo user to see role menus |
| Custom user model change fails mid-project | `migrate` was run before enabling the custom User | `docker compose down -v` and migrate again (development only) |
| `UserWarning: No directory at /app/staticfiles/` | `collectstatic` not run | Harmless in development |

## Habits that prevent most of these

- After adding an app or changing settings, run `docker compose restart web`.
- When the browser shows nothing, check `docker compose logs web --tail 10` first.
- Never use `down -v` unless you mean to wipe the database.
