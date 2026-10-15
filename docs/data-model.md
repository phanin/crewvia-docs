# Data model

Current state after Day 7. The rest of the Milestone 2 workflow tables are added as they are built.

```mermaid
erDiagram
    COMPANY ||--o{ USER : "has"
    COMPANY ||--o{ CUSTOMER : "owns"
    USER }o--o{ GROUP : "role"
    GROUP }o--o{ PERMISSION : "grants"
    CUSTOMER ||--o{ CONTACT : "has"
    CUSTOMER ||--o{ SITE : "has"
    CUSTOMER |o--o{ CUSTOMER : "parent company"
    CUSTOMER |o--o{ LEAD : "may be linked"
    CUSTOMER ||--o{ ESTIMATE : "receives"
    SITE ||--o{ ESTIMATE : "work at"
    LEAD |o--o{ ESTIMATE : "may start"
    ESTIMATE ||--o{ ESTIMATE_LINE : "has"

    LEAD {
        int id
        int company_id
        string name
        int customer_id
        string source
        string status
    }
    ESTIMATE {
        int id
        int company_id
        int seq
        int customer_id
        int site_id
        int lead_id
        string title
        string status
        decimal tax_rate
        date valid_until
    }
    ESTIMATE_LINE {
        int id
        int estimate_id
        int position
        string description
        decimal quantity
        decimal unit_price
    }
    COMPANY {
        int id
        string name
        string slug
        string timezone
        string currency
        bool is_active
    }
    USER {
        int id
        string username
        string email
        int company_id
        string phone
        bool is_active
    }
    CUSTOMER {
        int id
        int company_id
        string name
        string customer_type
        int parent_id
        string division
        string lead_source
        string external_id
        bool is_active
    }
    CONTACT {
        int id
        int customer_id
        string first_name
        string last_name
        bool is_primary
        bool is_invoice_contact
        bool is_active
    }
    SITE {
        int id
        int customer_id
        string name
        string address_line1
        string city
        decimal latitude
        decimal longitude
        bool is_active
    }
```

Every field is described on the [Data dictionary](data-dictionary.md).

## Base classes

| Class | Adds |
|---|---|
| `AuditModel` | `created_at`, `updated_at`, `created_by`, `updated_by` |
| `TenantModel` | Everything in `AuditModel`, plus a required `company` |
| `SequencedModel` | Everything in `TenantModel`, plus a per-company `seq` that gives records a readable number such as EST-0001 |
| `Access` | A table-less model that holds the app permission codenames |

## Rules the data follows

- Every business row has a `company`, and a user only ever sees their own company's rows.
- Contacts and sites copy their `company` from their customer, so they cannot drift apart.
- Nothing is deleted. Customers, contacts and sites are deactivated, and the history tables keep every change. The one exception is a line on a draft estimate, which can be removed; the history table still keeps it.
- Estimate numbers (EST-0001) count up separately for each company. Totals are worked out from the lines and never stored, so they cannot drift.
- A customer's parent company must be in the same company and can never create a loop.

## Planned next

WorkOrder, Visit, Invoice and Payment.
Invoices and estimates will keep Bill To, Payer, Service Location and Invoice
Contact as separate links, so a head office can pay for a branch's work. Already
in place on the customer side: `parent`, `division`, `lead_source`, site
`latitude` and `longitude`, and nullable `external_id`.
