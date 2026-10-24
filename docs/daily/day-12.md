# Day 12 - Invoices and payments (Wed Oct 21)

## Goals

- Bill for completed work and track what has been paid.

## Built

- **Invoices** created from a completed work order, copying lines and tax. See [Invoices and payments](../billing.md).
- **Send, void and edit** with the rules for each state.
- **Payments** with method, date and reference. Status moves Unpaid, Partial, Paid. Overdue is flagged.
- **Dashboard:** Awaiting payment (amount owed, overdue) and Ready to invoice. Menu: Invoices. Work order page: Create invoice.
- **Data dictionary:** `billing_invoice`, `billing_invoiceline`, `billing_payment`.
- **Tests:** about 30, covering money maths, status changes, overpayment, void rules, role access, other company, and the one-live-invoice constraint.

## Decisions

- Totals and payment status are calculated, not stored, so they cannot drift.
- Finance and Admin handle money. Office/Sales can look but not change.
- Payments are voided with a reason, never deleted.
- Lines are copied, so a later estimate change cannot alter a sent invoice.

## Problems avoided

- Two people clicking Create invoice at once: the work order row is locked and a database constraint backs it up.
- Two payments entered at once cannot overpay: the invoice row is locked while the balance is checked.
- Adding invoice counts to the dashboard changed what Finance sees, so one older dashboard test had to be updated to match.

## Result

261 tests passing (30 new in test_billing.py). One existing dashboard test updated because Finance now sees invoice counts. Ruff, check and migration check clean. Migration billing.0001_initial applied.

## Next

Production deployment and handover.
