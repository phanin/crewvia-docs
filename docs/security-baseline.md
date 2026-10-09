# Security baseline

What protects the system today, what the operator must do, and what is still
open. This is a working checklist for a small MVP. It is not a certification.

## Built into the application

| Area | Control | Status |
|---|---|---|
| Passwords | Salted PBKDF2 hashes; Django's four password validators (length, common, numeric, similarity) | Done |
| Sign-in | Every application page requires sign-in; sessions end on sign-out and when a user is deactivated | Done |
| Access control | Permissions are checked on the server for every page, by permission name, never by role name | Done |
| Tenant isolation | Querysets are limited to the signed-in user's company; a test proves company A cannot see company B | Done |
| Last Admin | The last active Admin of a company cannot be demoted or deactivated | Done |
| CSRF | Django CSRF protection on every form; state changes are POST only | Done |
| Output encoding | Django templates escape output by default | Done |
| Audit fields | `created_by` and `updated_by` on every business model; change history (`django-simple-history`) added to Milestone 2 models | Base done, history planned |
| Quality gate | CI runs lint, checks, migration check, tests and `check --deploy` on every pull request | Done |

## Production settings

| Setting | Value |
|---|---|
| `DEBUG` | `False` |
| HTTPS | Caddy terminates TLS; Django redirects any plain HTTP request |
| Cookies | `Secure` session and CSRF cookies |
| HSTS | Starts at one hour, raise to one year once stable |
| Headers | `nosniff`, `X-Frame-Options: DENY`, `same-origin` referrer policy |
| Secrets | Only in `.env.production` on the server; the file is git-ignored |
| Database | Not published to the network; reachable only from the app container |
| Container | App runs as a non-root user |

## Operator checklist (server)

- [ ] SSH with keys only; password login disabled
- [ ] Firewall allows only ports 22, 80 and 443
- [ ] Automatic security updates enabled
- [ ] `.env.production` readable only by the deploy user (`chmod 600`)
- [ ] Nightly backup running, copy stored off the server, restore tested
- [ ] Strong unique values for `DJANGO_SECRET_KEY` and `POSTGRES_PASSWORD`
- [ ] Platform superuser limited to the people who run the system

## Known gaps (accepted for the MVP, listed on purpose)

| Gap | Impact | When to address |
|---|---|---|
| No two-factor sign-in | A stolen password is enough | Before real customer data, or when a customer asks |
| No sign-in rate limiting or lockout | Password guessing is not slowed | Before production |
| Password reset emails go to the log until SMTP is set | Reset needs an operator | When a mail account exists |
| No monitoring or alerting | Outages found by users | After staging is live |
| No automated dependency update alerts | Old libraries linger | Enable Dependabot on the repository |
| No data export or deletion process | Cannot answer a data request quickly | Before production |
| Single server, no failover | An outage means downtime | Out of scope for the MVP |

## Reporting a problem

Send details to the project team directly rather than opening a public issue.
