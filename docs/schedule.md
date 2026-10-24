# Scheduling and dispatch

A **visit** is a booked time for one technician to work on a work order. The
schedule page shows the week at a glance and lists the work orders still waiting
for a visit.

## Who can do what

| Role | See the schedule | Book, change and cancel visits |
|---|---|---|
| Admin | Yes | Yes |
| Dispatcher | Yes | Yes |
| Office/Sales | Yes | No |
| Finance, Technician | No | No |

These follow the existing [permissions](permissions.md); no new permissions were added.

## Screens

| Screen | Address | What it does |
|---|---|---|
| Schedule | `/schedule/` | One week, Monday to Sunday, grouped by day. Previous, This week and Next buttons. Filter by technician. A side list shows work orders waiting for a visit: open ones, and in-progress ones that need another |
| Schedule a visit | button on the schedule or on a work order | Pick a technician, start, end and optional notes |
| Edit or cancel a visit | on the work order | Only while the visit is still Scheduled |

## Rules

- **Technicians only.** The technician list holds active users with the Technician role in your company.
- **No double booking.** A technician cannot have two overlapping active visits. Back-to-back is fine. A cancelled visit frees the slot. The form tells you which work order is in the way.
- **End after start.**
- **Work order status follows.** Booking a visit makes an Open work order Scheduled. Cancelling the last booked visit makes it Open again. Cancelling a work order cancels its booked visits.
- **Only some work orders can be booked:** Open, Scheduled, or In progress (for a job that needs a second visit). Work orders in QA review, completed or cancelled cannot.
- **Locked once started.** A visit that is in progress, completed or cancelled cannot be edited or cancelled here.
- **Company scoped.** Another company's visits return "not found".
- **Times** are entered and shown in local time (`America/Phoenix` for now; a per-company time zone is planned).

## Dashboard

The cards on the dashboard now show real numbers, limited to your company and to
what your role can see:

| Card | Counts |
|---|---|
| Open leads | New, contacted and estimate-sent leads |
| Pending estimates | Drafts and sent estimates |
| Active work orders | Open, scheduled, in progress and in QA review |
| Visits today | All visits today, whoever they belong to |
| My jobs today | Your own visits today |

The Unpaid invoices card stays a placeholder until invoices are built.

## Not included yet

A drag-and-drop calendar, recurring visits, travel time and routing, notifying the
technician, and a per-company time zone.
