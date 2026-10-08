# Day 4 - User management and CI (Wed Oct 7)

## Goals

- Let a company Admin manage the users of their own company.
- Run lint, checks and tests automatically on every pull request.
- Bring this documentation up to date.

## Built

- **User management** at `/users/`: list, add, edit, deactivate and reactivate. See [User management](../user-management.md).
- **Business rules:** Admin only, company scoped, one role per user, unique email, no self role change or self deactivation, the last active Admin is protected.
- **Dashboard:** a "Team members" card for Admins, with a link to the user list.
- **Navigation:** the Users item now opens the list and highlights on user pages.
- **CI workflow** (`.github/workflows/ci.yml`): `ruff check`, `manage.py check`, `makemigrations --check`, and `pytest` against PostgreSQL 16, on every pull request.
- **Tests:** a new test module for user management covering access by role, company isolation, create, edit, deactivate, the last-Admin rule, session ending, navigation and the dashboard card.
- **Docs:** this entry, the user management page, and updates to the roadmap, technology, architecture, conventions, setup and troubleshooting pages. The site and repository addresses are now set.

## Decisions

- Users are deactivated, never deleted.
- One role per user for the MVP. A user needing two kinds of access is a later question for the client.
- The Admin sets the first password. Invitation emails wait until email sending is in scope.
- Email is unique across the platform so a password reset reaches exactly one person.

## Problems avoided

- A view whose model is `User` would have hidden the signed-in user in the page header. The views name their context variable `target_user`, and a test checks the header.

## Result

_Fill in after the first test run: number of tests passing and whether CI is green._

## Next

Wireframes and models for Milestone 2, starting with customers and sites, once the client confirms the scope in writing.
