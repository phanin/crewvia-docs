# Day 6 - Customers, contacts and sites (Fri Oct 9)

## Goals

- Start Milestone 2 with the records every later screen depends on.

## Built

- **New `customers` app** with three tables: Customer, Contact and Site. See the [Data dictionary](../data-dictionary.md).
- **Screens:** customer list with search and status filter, customer detail with contacts and sites, add and edit forms, and deactivate or reactivate for all three. See [Customers, contacts and sites](../customers.md).
- **Permissions:** uses the customer and site permissions from Day 3. Sites are hidden from roles without site access.
- **Rules:** company scoping, one primary contact, no parent loops, coordinate ranges, company copied from the customer.
- **Change history** on all three tables through `django-simple-history`.
- **Admin site:** browse and inspect only; records are created in the app so company and audit fields are always set.
- **Menu:** Customers now opens the list.
- **`seed_demo_customers`** command for local demo data.
- **Styles:** stylesheet rebuilt for the new screens.
- **Tests:** 34 new tests across models and views.

## Decisions

- Customers, contacts and sites are deactivated, never deleted.
- The customer model already carries `parent`, `division`, `lead_source` and `external_id`, so later phases do not need a rebuild.
- Bill To, Payer and Invoice Contact stay separate on invoices (Milestone 2), not on the customer.
- Contacts and sites take their company from their customer in the model itself, so no code path can mismatch them.
- The database migration is generated on the developer machine and committed, so it matches the installed Django exactly.

## Problems avoided

- A view context variable named `user` would hide the signed-in user, as on Day 4. These views use `customer`.
- Checkboxes inherited the full-width text input style; the contact form gives them their own.

## Result

Fill in after running the checks.

## Next

Estimates and leads, then work orders created from approved estimates.
