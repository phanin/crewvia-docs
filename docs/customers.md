# Customers, contacts and sites

The first Milestone 2 screens. They are where every estimate, work order and
invoice will start.

## Who can do what

| Role | Customers and contacts | Sites |
|---|---|---|
| Admin | View, add, edit, deactivate | View, add, edit, deactivate |
| Office/Sales | View, add, edit, deactivate | View, add, edit, deactivate |
| Dispatcher | View | View |
| Finance | View | Not shown |
| Technician | No access | No access |

These follow the existing [permissions](permissions.md); no new permissions were added.

## Screens

| Screen | Address | What it does |
|---|---|---|
| Customer list | `/customers/` | Search by name, email, phone or ID. Filter Active, Inactive or All. 25 per page |
| Customer detail | `/customers/<id>/` | Details, contacts, sites, branches, parent company |
| Add and edit customer | `/customers/new/`, `/customers/<id>/edit/` | The customer form |
| Contacts | under a customer | Add, edit, deactivate. One primary contact per customer |
| Sites | under a customer | Add, edit, deactivate. Optional latitude and longitude |

## How it behaves

- **Company scoped.** A user cannot see, open or change another company's customers, contacts or sites. Their addresses return "not found".
- **Parent company.** A customer can belong to a larger customer, for example a branch of a bank. The choices only include your own active customers, and a customer can never be its own ancestor.
- **Primary contact.** Marking a contact as primary clears the previous primary contact.
- **Invoice contact.** A flag for who receives invoices. Milestone 2 invoices will use it.
- **Deactivate, never delete.** Inactive records are greyed out and hidden from the default list. History is kept.
- **Change history.** Every create and change is recorded with the user and time. Platform admins can read it in the admin site under History.
- **Platform superusers** (no company) are refused on these pages and use `/admin/`.

## Demo data

For local development only:

```bash
docker compose exec web python manage.py seed_demo
docker compose exec web python manage.py seed_demo_customers
```

This adds five customers to Demo Security (including a parent and branch pair
and a household) and one to Rival Alarm Co, so you can see company separation.
Running it again does nothing if the company already has customers.

## Not included yet

Importing customers from a file, merging duplicates, and customer-facing
access. Estimates, work orders and invoices link to these records next.
