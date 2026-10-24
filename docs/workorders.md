# Work orders

A work order is the job itself. It is created from an approved
[estimate](sales.md) and carries what the crew needs, without the prices.

## Who can do what

| Role | Work orders |
|---|---|
| Admin | View, create from an estimate, edit, cancel |
| Office/Sales | View, create from an estimate |
| Dispatcher | View, edit, cancel |
| Finance | View |
| Technician | No access to this list. Uses [My Jobs](technician.md) instead |

These follow the existing [permissions](permissions.md); no new permissions were added.

## Screens

| Screen | Address | What it does |
|---|---|---|
| Work order list | `/work-orders/` | Search by title, customer or site. Filter by status (Active by default) |
| Work order detail | `/work-orders/<id>/` | Job details, site access notes, crew instructions, visits, task list |
| Edit | `/work-orders/<id>/edit/` | Title, description, instructions, priority |
| Create | button on an approved estimate | One click, one time |

## How it behaves

- **Only approved estimates.** The Create work order button appears on an approved estimate that has no work order yet. Draft, sent and rejected estimates are refused.
- **One per estimate.** Clicking twice, or two people at once, still makes one work order.
- **What is copied.** Customer, site, title and the estimate notes as the description. Each estimate line becomes a task with its quantity. Prices stay on the estimate.
- **Numbers.** WO-0001, WO-0002 and so on, separately for each company.
- **Editing.** While open, scheduled or in progress. Not once completed or cancelled.
- **Cancelling.** Only open or scheduled work orders. Nothing is deleted.
- **Site access notes** (gate codes, parking) show only to roles that can view sites.
- **Company scoped.** Another company's work orders return "not found".
- **Change history** on work orders.

## Status

All six statuses exist from the start so later work needs no rebuild.

| Status | Set by |
|---|---|
| Open | Creating the work order, or cancelling its last booked visit |
| Scheduled | Booking a visit ([Scheduling and dispatch](schedule.md)) |
| In progress | The technician starting a visit ([Technician workflow](technician.md)) |
| In QA review | The technician finishing the whole job |
| Completed | Completion QA (later) |
| Cancelled | A dispatcher or admin, while open or scheduled. Booked visits are cancelled with it |

## Not included yet

Completion QA,
photos and signatures, and invoicing from a completed work order.
