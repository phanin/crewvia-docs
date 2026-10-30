# Day 16 - Real email and invoice fixes (Thu Oct 29)

## Goals

- Send a real invoice email from staging to a real inbox.
- Fix the rough edges found while testing.

## Built

- **Email delivery through Resend**, sending from `billing@mail.crewvia.net`. See [Email delivery](../email.md).
- **Small invoice fixes** (4 files, no migration):
    - The recipient falls back to the customer's own email when no contact is marked as the invoice contact or primary.
    - The "no recipient" warning links to the customer page.
    - The invoice list filter applies as soon as it is changed.
    - The empty "Awaiting payment" list explains that paid invoices are under Paid or All.

## Decisions

- **Not the company mailbox.** The first try was the Microsoft 365 mailbox through SMTP. Login was refused (`535 5.7.3`) because the mailbox uses multi-factor sign-in. Microsoft is also turning off password-based SMTP at the end of December 2026. Weakening the mailbox's security for app mail was rejected. A transactional email service is the right tool.
- **Sending subdomain.** Resend is set up on `mail.crewvia.net`, so its DNS records cannot interfere with the company mailbox on `crewvia.net`.
- **Free plan for now.** 3,000 emails a month, 100 a day. Revisit before the first paying client.

## Problems and fixes

- The new settings did not apply at first: the server env file still had the Microsoft lines, so the app kept contacting Microsoft. Fix: one set of email lines in the file, then `dc up -d --force-recreate web`.
- A due date of 4 days on one test invoice looked like a bug. It was an edit made on the invoice. The default is 30 days.
- A tax of 0% on one test invoice was the default (`DEFAULT_TAX_RATE=0.00`), copied from the estimate.

## Result

295 tests passing (5 new). A test email and a real invoice email, with the PDF attached, arrived in the Inbox, not spam.

## Next

Price list and customer import from a spreadsheet.
