# Production readiness

What must be true before real customer data goes in. Staging is for testing with demo
data; production is a separate deployment of the same code.

Tick items as they are done. Status shown is as of Mon Nov 2.

## Data safety

- [ ] Nightly backup scheduled
- [ ] A backup restored into a scratch database and checked
- [ ] Backups copied off the server
- [ ] Backup retention and who can read backups decided
- [ ] A way to export a company's data, and to remove a person's data on request

## Security

- [ ] Production secrets generated fresh and never shared in chat or tickets
- [ ] Secret key and database password rotation routine
- [ ] Two-factor sign-in for Admin users
- [ ] Sign-in rate limiting or lockout
- [ ] `DJANGO_HSTS_SECONDS` raised to one year
- [ ] Dependency update alerts enabled on the repository
- [ ] Platform superuser accounts limited and reviewed

## Email

- [x] Domain verified with the email provider
- [x] A real invoice email delivered to an inbox
- [ ] Production API key, separate from staging
- [ ] Provider plan matches volume (free plan is 100 emails a day)
- [ ] DMARC record in place and reports read
- [ ] Password reset email tested end to end

## Operations

- [ ] Uptime monitoring and an alert to a person
- [ ] Cost alert on the hosting account
- [ ] A deploy and rollback rehearsed, with releases tagged
- [ ] Log retention and where to read logs
- [ ] A named person on call and what to do in an outage

## Product

- [ ] Customer import and price list, so a company can start with its own data
- [ ] Tax rate set correctly for the company
- [ ] Customer-facing pages: terms of service and privacy policy
- [ ] Business name and ownership between the developers settled in writing
- [ ] A short onboarding checklist for a new company

## Quality

- [x] Automated tests pass in CI on every pull request (295 tests)
- [x] One job walked from lead to payment on staging ([acceptance checklist](m1-acceptance.md))
- [ ] The same walk done by someone who did not build it
