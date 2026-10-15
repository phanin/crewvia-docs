# Roadmap

## Milestone 1 - Foundation (target Oct 17)

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
- [x] Documentation set (architecture, data model, data dictionary, permissions, security baseline, setup, deployment)
- [x] Milestone 1 acceptance checklist drafted
- [ ] Staging server live with HTTPS and a tested backup restore
- [ ] Milestone 1 demo and acceptance

## Milestone 2 - Core workflow (target Nov 1)

- [x] Customers, contacts and sites
- [x] Leads and estimates (line items, status lifecycle)
- [ ] Work orders (created from approved estimates)
- [ ] Scheduling and dispatch (assign technician and time window)
- [ ] Technician workflow (mobile-first)
- [ ] Completion QA (pass or fail, reopen on failure)
- [ ] Invoice and payment status (Unpaid, Partial, Paid)
- [ ] Production deployment and handover

## Later phases (not in current scope)

| Idea | Notes |
|---|---|
| Live GPS map and routing | Site coordinates and visit times are stored from the start |
| AI recommendations and next actions | Needs enough real data first |
| Accounting integration | Nullable `external_id` fields are planned |
| Customer portal | Customers stay separate from users so a portal login can be linked |
| Online payments, PDF invoices | After the manual flow is proven |
| Email and SMS notifications | Needs a background job queue |
| Offline technician mode | Significant extra effort |
| Multi-division views | `division` field planned on key records |
