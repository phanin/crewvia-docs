# Data dictionary

Every table and field in the database after Day 6, with what it holds and
whether it contains personal data. Milestone 2 adds the workflow tables; they
will be added here as they are built.

!!! note "Source of truth"
    The models in `app/apps/*/models.py` are the source of truth. If this page
    and the code disagree, the code is right: fix this page.

## Conventions

- Every business table (from Milestone 2) inherits `TenantModel`: a required `company` plus `AuditModel` fields. A company can never see another company's rows.
- Money is stored as decimals, never floats (Milestone 2).
- Rows are deactivated, not deleted, so history stays intact.

## `companies_company`

The tenant: one row per customer company using the platform.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `name` | text (200) | Display name | No |
| `slug` | slug, unique | URL-safe identifier | No |
| `timezone` | text (64) | Default `America/Phoenix` | No |
| `currency` | text (3) | Default `USD` | No |
| `is_active` | boolean | Inactive companies are kept, not removed | No |
| `created_at`, `updated_at` | timestamp | Set automatically | No |

## `accounts_user`

A person who signs in. Extends Django's `AbstractUser`.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `username` | text, unique | Sign-in name | Yes |
| `email` | text | Unique across the platform (enforced by the user forms) | Yes |
| `first_name`, `last_name` | text | Optional | Yes |
| `phone` | text (32) | Optional | Yes |
| `password` | text | Salted hash (PBKDF2). The plain password is never stored | Sensitive |
| `company_id` | FK to company | `PROTECT`. Null only for platform superusers | No |
| `is_active` | boolean | False = cannot sign in. Used instead of deleting | No |
| `is_staff` | boolean | Can open `/admin/` | No |
| `is_superuser` | boolean | Platform operator, no company | No |
| `last_login`, `date_joined` | timestamp | Set by Django | Yes |

A user's role is membership in a Django Group (one role per user in the
current screens).

## `auth_group`, `auth_group_permissions`, `auth_permission`

| Table | Holds |
|---|---|
| `auth_group` | The five roles: Admin, Office/Sales, Dispatcher, Technician, Finance |
| `auth_permission` | The 23 application permissions (codenames like `estimates_approve`) plus Django's own |
| `auth_group_permissions` | Which role holds which permission. Rebuilt from code by `manage.py seed_roles` |
| `accounts_user_groups` | Which users hold which role |

The matrix is on [Roles and permissions](permissions.md).

## `customers_customer`

Who the company does work for. Inherits `TenantModel`, so every row has a `company`.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `company_id` | FK to company | Set by the app, never by the form | No |
| `name` | text (200) | Customer or household name | Yes (residential) |
| `customer_type` | text | `business` or `residential` | No |
| `parent_id` | FK to customer, nullable | Parent company (for branches). Cannot form a loop | No |
| `email`, `phone` | text | Optional | Yes |
| `billing_line1`, `billing_line2`, `billing_city`, `billing_state`, `billing_postal_code` | text | Billing address, all optional | Yes |
| `division` | text (80) | Line of business, for example Security | No |
| `lead_source` | text (80) | Where the customer came from | No |
| `external_id` | text (64), nullable | Reference in another system, for later accounting sync | No |
| `notes` | text | Free text | Possible |
| `is_active` | boolean | Customers are deactivated, never deleted | No |
| `created_at`, `updated_at`, `created_by_id`, `updated_by_id` | audit | Set automatically | No |

## `customers_contact`

A person at a customer.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `customer_id` | FK to customer | `PROTECT` | No |
| `company_id` | FK to company | Always copied from the customer | No |
| `first_name`, `last_name`, `job_title` | text | | Yes |
| `email`, `phone` | text | | Yes |
| `is_primary` | boolean | Only one primary contact per customer; saving a new one clears the old | No |
| `is_invoice_contact` | boolean | Receives invoices (Milestone 2 invoices will use it) | No |
| `is_active` | boolean | | No |
| audit fields | | As above | No |

## `customers_site`

A place where work is done. A customer can have many sites.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `customer_id` | FK to customer | `PROTECT` | No |
| `company_id` | FK to company | Always copied from the customer | No |
| `name` | text (200) | For example Main office | No |
| `address_line1`, `address_line2`, `city`, `state`, `postal_code` | text | Street address | Yes (residential) |
| `latitude`, `longitude` | decimal(9,6), nullable | Validated to -90..90 and -180..180. For later maps | No |
| `access_notes` | text | Gate codes, parking, who to ask for | Possible |
| `external_id` | text (64), nullable | Reference in another system | No |
| `is_active` | boolean | | No |
| audit fields | | As above | No |

## `sales_lead`

An enquiry that may turn into an estimate.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `company_id` | FK to company | Set by the app | No |
| `name` | text (200) | Who is asking, or what the enquiry is called | Yes |
| `customer_id` | FK to customer, nullable | Needed before an estimate can be created. `PROTECT` | No |
| `email`, `phone` | text | Optional | Yes |
| `source` | text (80) | How they found the company | No |
| `description` | text | What they need | Possible |
| `status` | text | `new`, `contacted`, `quoted`, `won`, `lost` | No |
| audit fields | | As above | No |

## `sales_estimate`

A priced proposal for a customer's site. Inherits `SequencedModel`.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `company_id` | FK to company | Always copied from the customer | No |
| `seq` | integer | Counts up per company; shown as `EST-0001`. Unique with `company_id` | No |
| `customer_id` | FK to customer | `PROTECT` | No |
| `site_id` | FK to site | Must belong to the same customer. `PROTECT` | No |
| `lead_id` | FK to lead, nullable | The enquiry it came from | No |
| `title` | text (200) | | No |
| `status` | text | `draft`, `sent`, `approved`, `rejected` | No |
| `tax_rate` | decimal (5,2) | Percent, 0 to 100 | No |
| `valid_until` | date, nullable | | No |
| `notes` | text | Shown to the customer | Possible |
| `sent_at`, `decided_at` | datetime, nullable | Set by status changes, not editable | No |
| audit fields | | As above | No |

Subtotal, tax and total are calculated from the lines each time. They are not stored.

## `sales_estimateline`

One priced line on an estimate.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `estimate_id` | FK to estimate | `PROTECT` | No |
| `company_id` | FK to company | Always copied from the estimate | No |
| `position` | integer | Display order, set automatically | No |
| `description` | text (300) | | No |
| `quantity` | decimal (8,2) | At least 0.01 | No |
| `unit_price` | decimal (10,2) | Zero or more | No |
| audit fields | | As above | No |

## `workorders_workorder`

The job to be done. Created from an approved estimate. Inherits `SequencedModel`.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `id` | bigint | Primary key | No |
| `company_id` | FK to company | Always copied from the customer | No |
| `seq` | integer | Per company; shown as `WO-0001`. Unique with `company_id` | No |
| `estimate_id` | one-to-one to estimate | One work order per estimate. `PROTECT` | No |
| `customer_id`, `site_id` | FK | Copied from the estimate. `PROTECT` | No |
| `title` | text (200) | Copied from the estimate, editable | No |
| `description` | text | Copied from the estimate notes, editable | Possible |
| `instructions` | text | Notes for the crew | Possible |
| `priority` | text | `low`, `normal`, `high`, `urgent` | No |
| `status` | text | `open`, `scheduled`, `in_progress`, `qa_review`, `completed`, `cancelled`. Only open and cancelled are used so far; the rest are set by scheduling and completion work | No |
| audit fields | | As above | No |

## `workorders_workorderitem`

A task on the work order, copied from an estimate line. Prices are not copied.

| Field | Type | Notes | Personal data |
|---|---|---|---|
| `work_order_id` | FK to work order | `PROTECT` | No |
| `company_id` | FK to company | Always copied from the work order | No |
| `position` | integer | Same order as the estimate | No |
| `description` | text (300) | | No |
| `quantity` | decimal (8,2) | | No |
| `is_done` | boolean | Used by the technician workflow later | No |
| audit fields | | As above | No |

## Change history tables

`customers_historicalcustomer`, `customers_historicalcontact`,
`customers_historicalsite`, `sales_historicallead`, `sales_historicalestimate` and
`sales_historicalestimateline` and `workorders_historicalworkorder` (from `django-simple-history`) keep one row for every
create, change and deactivation, with the user who did it and the time. They are
read in the admin site under each record's History button.

## Django framework tables

`django_session` (signed-in sessions, expire automatically), `django_admin_log`
(changes made in `/admin/`), `django_content_type`, `django_migrations`.

## Base classes (no table of their own)

| Class | Adds |
|---|---|
| `AuditModel` | `created_at`, `updated_at`, `created_by`, `updated_by` (both nullable, set on save) |
| `TenantModel` | Everything in `AuditModel` plus a required `company` |
| `Access` | A table-less model that carries the permission codenames |

## Personal data summary

| Data | Where | Why | Who can see it |
|---|---|---|---|
| Name, email, phone, username | `accounts_user` | Sign-in, contact | Admins of the same company |
| Customer and contact names, emails, phones, addresses | `customers_*` | Doing the work and billing | Roles with customer or site access in the same company |
| Lead names, emails, phones | `sales_lead` | Following up enquiries | Roles with lead access in the same company |
| Password hash | `accounts_user` | Authentication | Nobody through the app |
| Sign-in time | `accounts_user.last_login` | Security | Admins of the same company (admin site) |

Work orders, estimates and invoices arrive next and will be added here.

## Planned fields for Milestone 2

`division`, `lead_source`, site `latitude` and `longitude`, nullable
`external_id` for accounting sync, and separate Bill To, Payer, Service
Location and Invoice Contact links on estimates and invoices.
