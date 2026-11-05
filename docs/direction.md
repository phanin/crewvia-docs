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

## Design rule: core or pack

Every new capability is placed with one test: **would two different trades plausibly use
it?** If yes, it is core. If it only means something in one trade, it is a pack: data,
templates and a few screens that the core calls.

Features asked for by customers are checked the same way. The generic capability goes in
the core; the trade-specific part goes in the pack.

| Capability needed | Core (generic) | Pack (trade-specific) |
|---|---|---|
| Intake questions when a lead arrives | Configurable intake templates per service type | The questions for each trade |
| Estimate from customer plans | File upload and an extraction step | What to extract (devices and cable runs, equipment and ducting) |
| Purchase orders from stock | Inventory, stock levels, purchase orders | The item catalogue and price lists |
| Proposal, acceptance, deposit, commission | Proposal templates, customer acceptance link, configurable deposit and commission rules | Default wording and percentages |
| Lead routing | Manual and round-robin assignment rules | Nothing |
| Accounting | An integration layer, with QuickBooks Online as the first target | Nothing |

### Guardrails

1. Trade words (device, cable, unit, zone) belong in templates and data, never in table or
   field names or in code.
2. Intake templates come first: they are the mechanism that makes the product configurable.
3. Any AI extraction step takes its prompt and expected output as configuration per trade.
4. Product and documentation use neutral wording. Customer names stay out of the product
   and the public docs.
5. Sample data is a swappable pack (`seed_sample_data --pack`), not built-in assumptions.
6. A second trade is the test. Before a new feature is called core, ask what an HVAC or
   plumbing company would have to change to use it. The answer should be "nothing but data".

### Where the product stands against this rule

Built and trade-neutral today: customers and sites, leads and estimates with free-form
lines, work orders, scheduling, technician workflow, QA, invoices and payments, time and
mileage, and the pipeline view. The sample data and the demo wording lean toward security,
which is acceptable because they are packs. The next features (intake templates,
acceptance link, inventory, plan extraction) are where the rule needs the most attention.

## What stays the same for every trade

- One company never sees another's data.
- Permissions are per action; roles are bundles of permissions.
- Nothing is deleted; every change has history.
- A person approves anything that goes to a customer or changes money.

## Integrations

Accounting is planned through the nullable `external_id` fields, not built yet. A
customer portal and online payments come after the manual flow has been proven with real
use. Each is added behind the same permissions and history as the rest.
