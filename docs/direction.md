# Product direction

How Crewvia is meant to grow from one trade to several, without becoming several
separate products. This page is about design, not dates.

## Core and trade packs

Most of a field service business is the same whatever the trade: customers and sites,
estimates, jobs, scheduling, a technician in the field, QA, invoices and payments. That
is the **core**, and it is what is built today.

What differs between trades is a small set of things. These form a **pack**:

| In the pack | Security example | HVAC example |
|---|---|---|
| Price list and units | Cameras, panels, labor | Equipment, refrigerant, labor |
| Task templates for a job | Install checklist | Seasonal tune-up checklist |
| Records kept at a site | Devices, zones | Units, filters, model and serial numbers |
| Recurring work | Monitoring and inspection renewals | Maintenance plans |
| Terms and wording | Install, commission | Service call, tune-up |

The rule is that **trade logic lives in the pack, never in the core**. A new trade
should need data and a few screens, not changes to invoices or scheduling.

## Order of work

1. **Finish the core for real use**: customer import, price list, tax per company or site, backups tested.
2. **Security pack** on the core, used by a real company.
3. **Agent layer** on the core (see [Agent layer](agents.md)), so it helps every trade.
4. **HVAC pack**: equipment at a site and recurring service plans are the two real additions.

## What stays the same for every trade

- One company never sees another's data.
- Permissions are per action; roles are bundles of permissions.
- Nothing is deleted; every change has history.
- A person approves anything that goes to a customer or changes money.

## Integrations

Accounting is planned through the nullable `external_id` fields, not built yet. A
customer portal and online payments come after the manual flow has been proven with real
use. Each is added behind the same permissions and history as the rest.
