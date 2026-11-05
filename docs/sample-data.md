# Sample data

Two management commands fill a company with realistic data. They are used for demos
and, later, to give a new customer a starting point.

## `seed_sample_data`

```bash
docker compose exec web python manage.py seed_sample_data --company <company-slug> --pack security
```

Adds about 10 customers, 13 estimates and 16 or more leads to the named company.

- Customers are marked **"Sample data"** so they can be found and deactivated.
- **Idempotent:** running it twice does not duplicate anything.
- `--pack` chooses the trade. Only `security` exists today.

### Adding a pack

A pack is data, not code: customer names, typical line items with prices, lead
sources and work notes for one trade. Add a new entry to the packs table in the
command, give it a short name, and run with `--pack <name>`. Nothing else changes.
This follows the *core or pack* rule in [Product direction](direction.md).

## `seed_time_demo`

```bash
docker compose exec web python manage.py seed_time_demo --company <company-slug>
```

Adds recent finished shifts for technicians, and clocks in people who are not already
on the clock, so **On the clock now** shows something. It skips days that already
have a shift, so it is safe to run again.

## Cautions

- Run against **staging or local only**, never a company with real data.
- Demo technician accounts created by the seed get a password printed once; change or deactivate them before sharing the server.
