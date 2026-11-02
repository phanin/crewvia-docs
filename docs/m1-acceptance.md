# Milestone 1 acceptance checklist

A checklist the client can tick against a working demo. Each row says how to
check it. Map each row to the Milestone 1 description in the agreement before
sharing, and add or remove rows so the two match.

Ticked rows are covered by automated tests and have been used on staging. Rows 9 and
10 are still open: the backup script exists, but the nightly run and a restore test
on staging have not been done.

## Foundation

| # | Item | How to verify | Done |
|---|---|---|---|
| 1 | Application runs on a staging server over HTTPS | Open the staging address; the browser shows a valid certificate | [x] |
| 2 | Sign in, sign out, password reset | Sign in as each demo user; sign out; request a reset | [x] |
| 3 | Companies are separate | Sign in as users of two companies; neither sees the other's users | [x] |
| 4 | Five roles with different menus | Sign in as Admin, Office/Sales, Dispatcher, Technician, Finance; menus differ | [x] |
| 5 | Admin manages users | Add, edit, deactivate and reactivate a user; the last Admin is protected | [x] |
| 6 | Dashboard | Shows the role-appropriate summary, including team members for Admins | [x] |
| 7 | Direct access is refused | As a Technician, open `/users/`; a "No access" page appears | [x] |

## Quality and operations

| # | Item | How to verify | Done |
|---|---|---|---|
| 8 | Automated tests pass | CI shows a green check on the latest pull request | [x] |
| 9 | Database backup runs | `backups/` holds a recent file | [ ] |
| 10 | Backup can be restored | Restore on staging and confirm users are back | [ ] |
| 11 | Secrets are not in the repository | `.env.production` is absent from the repo | [x] |

## Documentation

| # | Item | Where | Done |
|---|---|---|---|
| 12 | Architecture and technology | Overview section of the docs site | [x] |
| 13 | Data model and data dictionary | Overview section | [x] |
| 14 | Roles and permissions matrix | Overview section | [x] |
| 15 | Setup and deployment guides | Guides section | [x] |
| 16 | Security baseline | Overview section | [x] |

## Milestone 2 - one job from lead to payment

Walk one job through the whole workflow on staging. Each step should work without help.

| # | Step | How to verify | Done |
|---|---|---|---|
| 17 | Add a lead and link a customer with a site | Leads, then Add lead | [x] |
| 18 | Create, price, send and approve an estimate | Estimate shows lines, tax and total | [x] |
| 19 | Create the work order | Work order carries tasks but no prices | [x] |
| 20 | Book a technician | Schedule shows the visit; a double booking is refused | [x] |
| 21 | Technician starts, ticks tasks and finishes | My Jobs, as the technician | [x] |
| 22 | QA passes the job | QA queue, then the work order is Completed | [x] |
| 23 | Create the invoice | Draft with the estimate's lines and tax | [x] |
| 24 | Open the PDF | Totals match the invoice | [x] |
| 25 | Email it | Arrives in the inbox with the PDF; Emails sent shows Sent | [x] |
| 26 | Record part, then full payment | Status moves Partial, then Paid; list filter Paid shows it | [x] |
| 27 | Void a wrong payment | Balance goes back up; the payment stays, struck through | [x] |
| 28 | Another company sees none of it | Sign in as `rival_admin` | [x] |

## Not part of Milestone 1

Customers, estimates, work orders, scheduling and invoices belong to Milestone 2.

## Sign-off

| Role | Name | Date |
|---|---|---|
| Client | | |
| Developer | | |
