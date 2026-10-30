# Day 15 - Invoice PDF and email (Wed Oct 28)

## Goals

- Let finance open an invoice as a PDF and email it to the customer from the app.

## Built

- **PDF.** An invoice opens as a one-page PDF (`/invoices/<id>/pdf/`): company, bill to, site, dates, lines, totals, payments received, balance due, notes. A draft or void invoice is marked as such.
- **Email.** **Email to customer** on the invoice sends the PDF as an attachment (`/invoices/<id>/email/`). The recipient is pre-filled from the customer's invoice contact. A short note can be added.
- **Email history.** Every attempt is logged in a new table, `billing_invoiceemail`, with the address, subject, status (Sent or Failed) and error. The invoice page lists them under **Emails sent**.
- Emailing a draft also sends the invoice, in one step.
- New library: `reportlab` (PDF drawing).

See [Invoices and payments](../billing.md) and [Email delivery](../email.md).

## Decisions

- **A failed send changes nothing.** The email goes out inside the same step that marks the invoice sent. If the mail server refuses it, the invoice stays a draft, and the failure is logged separately so it can be seen.
- Emailing a void invoice is not allowed.
- Only roles that can edit invoices (Admin, Finance) can email. Anyone who can view an invoice can open the PDF.
- Until an email provider is set, the app writes emails to the server log instead of sending them.

## Problems and fixes

- **Wrong base.** The first version of this work was built on an out-of-date copy of the repository and would have replaced the real billing code. It was caught when `makemigrations` proposed removing fields. The change was undone (`migrate billing 0001`, stash), and rebuilt as an additive patch on the real files. Rule from now on: check the real files in the repository before changing an area, and ship only new or changed files.
- **Test run scope.** Running `pytest apps/billing` alone misses shared fixtures and shows fixture errors. Run the full `pytest -q`.

## Result

290 tests passing. One migration (`billing/0002_invoiceemail`). Deployed to staging: `/invoices/1/pdf/` renders correctly, with totals, tax and payments.

## Next

Make real email work on staging.
