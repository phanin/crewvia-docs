# Invoices and payments

A completed work order can be invoiced. The invoice copies the priced lines and tax rate from the estimate, is checked, sent, and then payments are recorded against it until the balance is zero.

## Who can do what

| Role | Invoices |
|---|---|
| Admin, Finance | View, create, edit draft, send, void, record and void payments |
| Office/Sales | View only |
| Dispatcher, Technician | No access |

## Screens

| Screen | Address | What it shows |
|---|---|---|
| Invoice list | `/invoices/` | Search, and a filter: Awaiting payment (default), All, Draft, Unpaid, Partial, Paid, Overdue, Void |
| Invoice | `/invoices/<id>/` | Details, lines, totals, paid and balance, payment history |
| Edit | `/invoices/<id>/edit/` | Issue date, due date and notes. Draft only |
| Record payment | `/invoices/<id>/payments/new/` | Amount, method, date, reference |

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
