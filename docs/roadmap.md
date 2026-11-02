# Roadmap

Status as of Mon Nov 2. Ticked items are built and running on staging.

## Milestone 1 - Foundation

- [x] Repository, Docker Compose, Django and PostgreSQL scaffold
- [x] Company, custom User, tenant and audit base
- [x] Login, logout, password reset
- [x] Role groups and permission matrix
- [x] Role-based navigation and dashboard shell
- [x] Tailwind styling
- [x] Public documentation site
- [x] User management screens
- [x] CI workflow with ruff, Django checks, migration check and pytest
- [x] Production stack files (Docker, HTTPS proxy, backup and restore scripts)
- [x] Documentation set
- [x] Staging server live with HTTPS (Oracle Cloud, `staging.crewvia.net`)
- [ ] Backup restore tested, nightly backup scheduled and copied off the server
- [ ] Milestone 1 demo and acceptance sign-off

## Milestone 2 - Core workflow

- [x] Customers, contacts and sites
- [x] Leads and estimates (line items, status lifecycle)
- [x] Work orders (created from approved estimates)
- [x] Scheduling and dispatch (assign technician and time window)
- [x] Technician workflow (mobile-first)
- [x] Completion QA (pass or send back)
- [x] Invoices and payment status (Unpaid, Partial, Paid, Overdue)
- [x] Invoice PDF and email with history of what was sent
- [x] Real outgoing email through Resend
- [ ] Production deployment and handover

## Next: make it usable with real data

Work planned next, in this order:

| Item | Why |
|---|---|
| Customer import from a spreadsheet (preview first, skip duplicates) | A company cannot start by typing every customer |
| Price list with a picker on estimate lines | Faster, more consistent estimates |
| Tax rate per company or site instead of one server setting | Work crosses tax areas |
| Backup restore test, nightly backup, off-server copy | Safe before real data |
| Rotate staging secrets, scheduled from now on | Good habit before production |
| Email: second environment key, Pro plan when the daily limit is near | Resend free plan is 100 a day |

## After that: the agent layer

Software that prepares work for a person to approve: chasing late payments, drafting
quotes, pre-checking QA, suggesting dispatch. Designed in [Agent layer](agents.md);
not built yet. It needs the real data above first.

## Later phases (not in current scope)

| Idea | Notes |
|---|---|
| Second trade pack (HVAC) | Needs equipment records and recurring service plans. See [Product direction](direction.md) |
| Notifications by email and SMS | Needs a background job queue |
| Live GPS map and routing | Site coordinates and visit times are stored from the start |
| Accounting integration | Nullable `external_id` fields are planned |
| Customer portal | Customers stay separate from users so a portal login can be linked |
| Online payments | After the manual flow is proven with real use |
| Two-factor sign-in, sign-in rate limiting | Before real customer data. See [Security baseline](security-baseline.md) |
| Data export and deletion | Before production |
| Offline technician mode | Significant extra effort |
| Multi-division views | `division` field planned on key records |

Done since this list was first written: PDF invoices and email sending.
