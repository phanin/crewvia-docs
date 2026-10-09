# Milestone 1 acceptance checklist

A checklist the client can tick against a working demo. Each row says how to
check it. Map each row to the Milestone 1 description in the agreement before
sharing, and add or remove rows so the two match.

## Foundation

| # | Item | How to verify | Done |
|---|---|---|---|
| 1 | Application runs on a staging server over HTTPS | Open the staging address; the browser shows a valid certificate | [ ] |
| 2 | Sign in, sign out, password reset | Sign in as each demo user; sign out; request a reset | [ ] |
| 3 | Companies are separate | Sign in as users of two companies; neither sees the other's users | [ ] |
| 4 | Five roles with different menus | Sign in as Admin, Office/Sales, Dispatcher, Technician, Finance; menus differ | [ ] |
| 5 | Admin manages users | Add, edit, deactivate and reactivate a user; the last Admin is protected | [ ] |
| 6 | Dashboard | Shows the role-appropriate summary, including team members for Admins | [ ] |
| 7 | Direct access is refused | As a Technician, open `/users/`; a "No access" page appears | [ ] |

## Quality and operations

| # | Item | How to verify | Done |
|---|---|---|---|
| 8 | Automated tests pass | CI shows a green check on the latest pull request | [ ] |
| 9 | Database backup runs | `backups/` holds a recent file | [ ] |
| 10 | Backup can be restored | Restore on staging and confirm users are back | [ ] |
| 11 | Secrets are not in the repository | `.env.production` is absent from the repo | [ ] |

## Documentation

| # | Item | Where | Done |
|---|---|---|---|
| 12 | Architecture and technology | Overview section of the docs site | [ ] |
| 13 | Data model and data dictionary | Overview section | [ ] |
| 14 | Roles and permissions matrix | Overview section | [ ] |
| 15 | Setup and deployment guides | Guides section | [ ] |
| 16 | Security baseline | Overview section | [ ] |

## Not part of Milestone 1

Customers, estimates, work orders, scheduling and invoices belong to Milestone 2.

## Sign-off

| Role | Name | Date |
|---|---|---|
| Client | | |
| Developer | | |
