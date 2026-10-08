# Day 3 - Auth and roles (Tue Oct 6)

## Goals

- Login, logout and password reset.
- Role groups and a permission matrix.
- Role-based navigation and a dashboard shell.

## Built

- Login, logout and a full password reset flow with styled templates.
- `apps/core/permissions.py`: permissions and the role matrix for Admin, Office/Sales, Dispatcher, Technician and Finance.
- `seed_roles` (create or refresh roles) and `seed_demo` (two companies and one user per role, development only).
- Navigation driven by permissions, with a context processor.
- App shell (`base.html`), dashboard with permission-gated stat cards, 403 and 404 pages.
- Tailwind v4 standalone build producing `output.css`.
- 20 tests: auth, roles, menus per role, and a company isolation check.
- This documentation site (MkDocs + Material), published with GitHub Pages from a separate public repository.

## Decisions

- Permissions are checked as `core.<name>`, never by role name.
- QA review sits with Dispatcher and Admin. Only Admin approves estimates. Both are to be confirmed with the client.
- Roles live in code and are applied with `seed_roles`.

## Problems and fixes

- `migrate` failed with `failed to resolve host 'db'` because the database container was not running. `docker compose up -d` fixed it.
- The site was unreachable because the dev server had stopped. Restarting `web` fixed it.
- The page was unstyled until `output.css` existed, and the dashboard showed an old template after a merge kept the old file.
- Pushing the docs workflow was rejected until the personal access token had the `workflow` scope.
- The generated `seed_demo` password is shown only once. A lost password is reset with `changepassword`.

## Result

All 20 tests pass. Signing in as the Technician demo user shows only Dashboard and My Jobs and one card, as the matrix says.

## Next

User management screens, CI with ruff and pytest, then the Milestone 2 wireframes.
