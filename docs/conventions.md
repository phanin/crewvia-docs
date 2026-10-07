# Conventions

## Git

- `main` is production, `develop` is staging, work happens on short-lived
  `feature/...` branches and merges through pull requests into `develop`.
- Every pull request is reviewed by the other developer.
- Never commit `.env`, the `tailwindcss` binary, or secrets.
- Commit migration files.

## Code

- Every business model inherits `TenantModel`.
- Every list and detail view of a business model uses `CompanyScopedMixin`.
- Check permissions per action (`core.estimates_approve`), never by role name.
- Money uses `DecimalField`, never floats.
- Status changes go through model methods (for example `estimate.approve()`),
  not scattered view code.
- HTMX views render a full page normally and a partial when `request.htmx` is true.

## Tests

- Tests live next to the app in `tests/`.
- Shared fixtures are in `app/conftest.py` (companies, roles, `make_user`).
- Any new tenant model needs an isolation test.

## Definition of done

Works on staging, matches acceptance criteria, passes the role-based check,
and docs are updated.
