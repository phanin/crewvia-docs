# Day 7 - Leads and estimates (Wed Oct 14)

## Goals

- Add the sales steps that come before a job: leads, then estimates with priced lines.

## Built

- **New `sales` app** with three tables: Lead, Estimate and EstimateLine. See the [Data dictionary](../data-dictionary.md).
- **Screens:** lead list and form, estimate list, detail, form, and line forms. See [Leads and estimates](../sales.md).
- **Status life cycle:** draft, sent, approved, rejected, with the allowed moves enforced in the model, not only in the buttons.
- **Per-company numbers** (EST-0001) through a small shared base class, `SequencedModel`, that work orders reuse on Day 8.
- **Permissions:** uses the lead and estimate permissions from Day 3. Send and approve are separate.
- **Menu:** Leads and Estimates now open real pages.
- **Change history** on all three tables.
- **Admin site:** browse and inspect only.
- **Styles:** stylesheet rebuilt.
- **Tests:** new model and view tests, including role access, company separation, locked estimates and the life cycle.
- **Day 6 result line** filled in: 89 tests passing.

## Decisions

- Totals are calculated from the lines, never stored.
- A sent estimate is locked. Changing it means reopening it first.
- A site is required on every estimate, because a work order needs somewhere to go.
- Approving is Admin only for now, matching the permission matrix. Easy to open to Office/Sales later.
- A lead can exist without a customer. Creating an estimate from it asks for the link first.
- Lines are the one thing that can be removed (on a draft only). Their history is kept.

## Problems avoided

- Two people saving an estimate at once could take the same number. The number has a unique constraint and the save retries.
- A site from a different customer could be chosen. The model checks it, not only the form.

## Result

Fill in after running the checks.

## Next

Day 8: work orders created from approved estimates.
