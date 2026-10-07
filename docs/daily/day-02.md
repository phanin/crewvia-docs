# Day 2 - Company and User (Mon Oct 5)

## Goals

- Create the tenant and user foundation before any migration.
- Start the requirements confirmation with the client.

## Built

- `Company` model (the tenant) with timezone and currency.
- Custom `User` extending Django's user, with a `company` link and phone.
- `AuditModel` and `TenantModel` abstract bases.
- `CompanyScopedMixin` to filter views by company and stamp audit fields.
- Admin registrations, first migrations, and two model tests.
- Requirements Confirmation template.

## Decisions

- The custom user model is enabled before the first `migrate`.
- Superusers have no company. They act as platform administrators.
- Login uses username for now. Email login is a question for the client.

## Problems and fixes

- `No module named 'apps.core'`: files were copied to the wrong level. Merged correctly.
- The dev server had crashed at startup and did not recover on its own. `docker compose restart web` fixed it.

## Result

Admin login works, models are migrated, and 2 tests pass.

## Next

Login, logout, password reset, role groups and the permission matrix.
