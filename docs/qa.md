# Completion QA

When a technician finishes the whole job, the work order moves to **In QA review**.
A dispatcher or admin then checks it and either passes it or sends it back.

## Who can do what

| Role | Completion QA |
|---|---|
| Admin, Dispatcher | Review, pass and send back |
| Everyone else | No access |

## Screens

| Screen | Address | What it shows |
|---|---|---|
| QA queue | `/qa/` | Work orders waiting for review, oldest first |
| Review | `/qa/<id>/` | Crew notes from each completed visit, earlier reviews, the task list, and the decision form |

The dashboard shows an **Awaiting QA** count for roles that can review, and the
work order page has a **Review work** button while it is in QA review.

## Deciding

- **Pass and complete.** The work order becomes **Completed**. Notes are optional.
- **Send back.** Notes are required. The work order returns to **In progress** and shows on the schedule's waiting list, so a dispatcher can book another visit. Tick any task that was not done well to un-tick it for the crew.

Every decision is saved with who made it and when, and shown on the work order page.

## Rules

- Only a work order in QA review can be reviewed. Anything else returns "not found".
- Another company's work order returns "not found".
- Only the work order's own tasks can be re-opened.
- A work order can be sent back and reviewed again as many times as needed.
- Completed is final for now. Invoices are the next step.
