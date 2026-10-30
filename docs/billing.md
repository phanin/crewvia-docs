# Invoices and payments

A completed work order can be invoiced. The invoice copies the priced lines and tax rate from the estimate, is checked, sent, and then payments are recorded against it until the balance is zero.

## Who can do what

| Role | Invoices |
|---|---|
| Admin, Finance | View, create, edit draft, send, email, void, record and void payments |
| Office/Sales | View only |
| Dispatcher, Technician | No access |

## Screens

| Screen | Address | What it shows |
|---|---|---|
| Invoice list | `/invoices/` | Search, and a filter that applies when changed: Awaiting payment (default), All, Draft, Unpaid, Partial, Paid, Overdue, Void. Paid invoices are not under Awaiting payment |
| Invoice | `/invoices/<id>/` | Details, lines, totals, paid and balance, payment history |
| Edit | `/invoices/<id>/edit/` | Issue date, due date and notes. Draft only |
| Record payment | `/invoices/<id>/payments/new/` | Amount, method, date, reference |
| PDF | `/invoices/<id>/pdf/` | The invoice as a one-page PDF. Anyone who can view invoices |
| Email | `/invoices/<id>/email/` | Send the PDF to the customer. Admin and Finance |

A completed work order shows **Create invoice**, or a link to its invoice. The dashboard shows **Awaiting payment** (with the amount owed and overdue count) and **Ready to invoice**.

## Life of an invoice

1. **Draft.** Created from the work order. Dates and notes can be edited.
2. **Sent.** Needs at least one line and a total above zero. Dates default to today and 30 days later. Lines are now fixed.
3. **Payments.** Each one is Unpaid, then Partial, then Paid as the total received reaches the invoice total. An invoice past its due date that is not Paid shows **Overdue**.
4. **Void.** Allowed for a draft or sent invoice with no live payments. Void the payments first if there are any.

## Rules

- A payment cannot take the total paid above the invoice total.
- A wrong payment is voided with a reason. It stays on the page, struck through, and the balance goes back up.
- Nothing is deleted. Every change is in the history.

## PDF

The PDF shows the company, bill to (the invoice contact), site, issue and due dates, lines, subtotal, tax, total, payments received, balance due and notes. A draft or void invoice is stamped as such. Voided payments are left out.

## Emailing an invoice

**Email to customer** sends the PDF as an attachment, with a short plain-text message and an optional note.

- **Recipient.** Pre-filled in this order: the customer's invoice contact, then the primary contact, then the customer's own email. If none has an email the page says so and links to the customer. Any address can be typed instead.
- **A draft is sent as part of emailing.** It becomes Sent and gets its dates, as with **Send invoice**.
- **A failed send changes nothing.** The invoice stays as it was, and the failure is recorded.
- A void invoice cannot be emailed. Office/Sales can view but not email.
- Every attempt is listed on the invoice under **Emails sent**: who, when, subject, Sent or Failed. See [Email delivery](email.md) for setup.
