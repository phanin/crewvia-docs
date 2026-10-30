# Email delivery

Crewvia sends email from the app, starting with invoices. Email goes through **Resend**, a service for app email, not through a staff mailbox.

## How it works

1. A person clicks **Email to customer** on an invoice.
2. The app builds the message with the invoice PDF attached and hands it to Resend over SMTP.
3. If Resend accepts it, the attempt is logged as **Sent**. If not, nothing changes on the invoice and the attempt is logged as **Failed**, with the error.
4. Replies go to the sender's own address (Reply-To), not to the sending address.

| | |
|---|---|
| Sending address | `Crewvia <billing@mail.crewvia.net>` (from `DEFAULT_FROM_EMAIL`) |
| Sending domain | `mail.crewvia.net`, a subdomain, so it cannot affect the company mailbox on `crewvia.net` |
| Provider | Resend, region US East. SMTP host `smtp.resend.com`, port 587, user `resend`, password is an API key |
| DNS | Records for the subdomain were added in GoDaddy (DKIM, SPF and return path) and show as verified in Resend |

## Settings

In the server env file ([Staging server](staging.md)):

```text
DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.resend.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=resend
EMAIL_HOST_PASSWORD='<API key>'
DEFAULT_FROM_EMAIL=Crewvia <billing@mail.crewvia.net>
```

Keep one set of these lines. If a name appears twice, the last one wins. Use a separate API key for each environment, with sending access only.

Without these, locally and in tests, email goes to the console or memory instead of being sent.

## Test

```bash
dc exec web python manage.py sendtestemail you@example.com
```

No output means it was accepted. A traceback shows the provider's error.

## Cost

Resend's free plan allows 3,000 emails a month and 100 a day. The Pro plan removes the daily limit. Check the current prices before relying on them.

## Why not the company mailbox

The mailbox is on Microsoft 365 and is protected with multi-factor sign-in, which a password-only app login cannot pass. Microsoft is also switching off password-based SMTP for new use at the end of December 2026, with final removal planned for later. Turning security off to send app mail was not accepted.

## Troubleshooting

| Symptom | Cause and fix |
|---|---|
| `535 5.7.3 Authentication unsuccessful` from `outlook.com` | The app is still using the Microsoft mailbox, or multi-factor sign-in blocks it. Check the env file for duplicate or old lines, then `dc up -d --force-recreate web` |
| `535 Authentication failed` from Resend | Wrong or deleted API key, or the username is not exactly `resend` |
| Domain not verified in Resend | DNS records missing or typed with the domain doubled. In GoDaddy the name is only the part before `.crewvia.net` |
| Message arrives in spam | New sending domain. Check DKIM and SPF show verified, add a DMARC record, send a few real messages over time |
| Warning on the email page: no contact | The customer has no invoice contact, primary contact or email. Add a contact on the customer page and mark it as the invoice contact |
| "The email could not be sent, so nothing was changed" | The provider refused it. The error is stored on the failed attempt in the admin site (Invoice emails) |
