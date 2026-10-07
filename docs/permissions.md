# Roles and permissions

Roles are Django Groups. Each role is a bundle of permissions defined in
`app/apps/core/permissions.py`. Code checks permissions (for example
`core.estimates_approve`), never role names. After changing the matrix, run
`python manage.py seed_roles`.

!!! note "To confirm with the client"
    QA review (Dispatcher and Admin) and estimate approval (Admin only) are
    working assumptions.

## Matrix

| Permission | Admin | Office/Sales | Dispatcher | Technician | Finance |
|---|:-:|:-:|:-:|:-:|:-:|
| `dashboard_view` - view dashboard | ✔ | ✔ | ✔ | ✔ | ✔ |
| `users_manage` - manage users | ✔ |  |  |  |  |
| `customers_view` - view customers | ✔ | ✔ | ✔ |  | ✔ |
| `customers_edit` - create and edit customers | ✔ | ✔ |  |  |  |
| `sites_view` - view sites | ✔ | ✔ | ✔ |  |  |
| `sites_edit` - create and edit sites | ✔ | ✔ |  |  |  |
| `leads_view` - view leads | ✔ | ✔ |  |  |  |
| `leads_edit` - create and edit leads | ✔ | ✔ |  |  |  |
| `estimates_view` - view estimates | ✔ | ✔ |  |  |  |
| `estimates_edit` - create and edit estimates | ✔ | ✔ |  |  |  |
| `estimates_send` - send estimates | ✔ | ✔ |  |  |  |
| `estimates_approve` - approve estimates | ✔ |  |  |  |  |
| `workorders_view` - view work orders | ✔ | ✔ | ✔ |  | ✔ |
| `workorders_create` - create work orders | ✔ | ✔ |  |  |  |
| `workorders_edit` - edit work orders | ✔ |  | ✔ |  |  |
| `schedule_view` - view schedule | ✔ | ✔ | ✔ |  |  |
| `schedule_manage` - assign and reschedule technicians | ✔ |  | ✔ |  |  |
| `jobs_view_own` - view own assigned jobs | ✔ |  |  | ✔ |  |
| `jobs_update_own` - update own assigned jobs | ✔ |  |  | ✔ |  |
| `qa_review` - review completion QA | ✔ |  | ✔ |  |  |
| `invoices_view` - view invoices | ✔ | ✔ |  |  | ✔ |
| `invoices_edit` - create and edit invoices | ✔ |  |  |  | ✔ |
| `payments_record` - record payments | ✔ |  |  |  | ✔ |

## Roles

| Role | Intended use |
|---|---|
| Admin | Company owner or manager. Everything, including user management. |
| Office/Sales | Customers, sites, leads, estimates, creating work orders. |
| Dispatcher | Scheduling and assigning technicians, QA review. |
| Technician | Own assigned jobs only. No pricing or invoices. |
| Finance | Invoices and payments, read access to customers and work orders. |
