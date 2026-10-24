# Technician workflow

**My Jobs** is the technician's own view: today's visits, what is coming up, and
what to do at each one. It is built for a phone, with big buttons and one column.

## Who can do what

| Role | My Jobs |
|---|---|
| Technician | Sees and works only their own visits |
| Everyone else | No access (dispatch uses the [schedule](schedule.md)) |

A technician cannot open another technician's visit, or another company's. Those
addresses return "not found".

## Screens

| Screen | Address | What it shows |
|---|---|---|
| My jobs | `/my-jobs/` | Today, Coming up, Earlier and not finished, and the last five finished visits |
| Visit | `/my-jobs/<id>/` | Customer, site address with an Open in Maps button, access notes, the job description, crew instructions, notes for this visit, the task list |

The visit page does not show prices, and does not show customer contact details.
That is deliberate for now; say if crews need to call the customer from the app.

## A visit, step by step

1. **Start visit.** Available from the day it is scheduled. The visit becomes In progress and the work order becomes In progress.
2. **Mark done** on each task as the work gets done. Tasks can only be ticked while the visit is in progress.
3. **Finish this visit.** Add a note on what was done, then choose:
    - **The whole job is finished** (ticked): every task must be done first. The visit is Completed, any leftover booked visits are cancelled, and the work order moves to **In QA review**.
    - **Unticked:** the visit is Completed but the work order stays In progress. It then shows on the schedule's waiting list as needing another visit, and the dispatcher books it.

## What the office sees

The work order page lists each visit with its start and finish times and the
technician's note, so a dispatcher can see what happened without calling.

## Rules

- A visit cannot be completed before it is started.
- A visit cannot be started before its scheduled day.
- A finished job cannot have open tasks.
- A work order that has started can no longer be cancelled.
- Every start, tick and completion records who did it and when (history on visits and work orders).

## Not included yet

Photos and signatures, offline use, time tracking beyond start and finish,
notifying the office, and the QA review that follows (Day 11 or later).
