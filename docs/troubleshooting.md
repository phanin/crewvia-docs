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
| Push rejected: `refusing to allow a Personal Access Token to create or update workflow` | Token lacks the `workflow` scope | Add `workflow` to the token, or add the workflow file in the GitHub web editor |
| GitHub Pages is unavailable for the repository | Pages needs a public repo on the Free plan | Publish the docs from a separate public repository |
| Lost the generated demo password | `seed_demo` prints it once | `python manage.py changepassword <user>` |
| Edit-user page shows the wrong name in the header | A view context variable named `user` hid the signed-in user | Use `context_object_name = "target_user"` |
| `makemigrations` warns `failed to resolve host 'db'` | The database container is not running | `docker compose up -d`, then retry |

## Found on staging and in billing

| Symptom | Cause | Fix |
|---|---|---|
| New code not running after a deploy | `up -d` without `--build` | `git pull && dc up -d --build` |
| Changed `.env.production`, no effect | Container not recreated | `dc up -d --force-recreate web` |
| `535 5.7.3 Authentication unsuccessful` from `outlook.com` | App still pointed at the Microsoft mailbox, or multi-factor blocks password login | One set of `EMAIL_*` lines pointing at Resend, then recreate `web`. See [Email delivery](email.md) |
| `sendtestemail` prints nothing | That means it succeeded | Check the inbox, spam, and the Resend logs |
| "No invoice contact with an email address" | Customer has no contact or email set | Add a contact, tick invoice contact, or fill the customer email |
| Paid invoice missing from the invoice list | Default filter is Awaiting payment | Choose Paid or All |
| Invoice tax is 0% | `DEFAULT_TAX_RATE` is `0.00`, copied from the estimate | Set the rate in `.env.production`, or edit the estimate |
| `pytest apps/billing` shows fixture-not-found errors | A single-folder run misses the shared `conftest.py` | Run the full `pytest -q` |
| `makemigrations` proposes removing fields | The code copied in came from an out-of-date base | Stop, undo, rebuild on the current files. Ship only new or changed files |
| `git push` says `Total 0` | Files were staged but not committed | `git commit`, then `git push` |
| Gunicorn `Control server error: Permission denied: '/home/app'` | Control socket cannot be created for the non-root user | Harmless |

## Habits that prevent most of these

- After adding an app or changing settings, run `docker compose restart web`.
- When the browser shows nothing, check `docker compose logs web --tail 10` first.
- Never use `down -v` unless you mean to wipe the database.
- Before changing an area, work from its current files, and change only what is needed.
- Never paste secrets into chat or tickets. If you do, rotate them.
