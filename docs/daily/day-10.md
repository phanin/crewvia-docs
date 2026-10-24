# Day 10 - Technician workflow (Thu Oct 15)

## Goals

- Give technicians their own phone-friendly screens to do the work and hand it over.

## Built

- **My Jobs** list and visit page. See [Technician workflow](../technician.md).
- **Start a visit, tick tasks, complete a visit** with a note, and say whether the whole job is finished.
- **Hand-off:** a finished job moves to In QA review; a partial visit leaves the work order In progress and puts it on the schedule's waiting list.
- **Three new fields on visits:** started, completed and the technician's note. See the [Data dictionary](../data-dictionary.md).
- **Dispatch can book a second visit** on a work order that is in progress.
- **Work order page** shows when each visit started and finished and the technician's note.
- **Dashboard:** My jobs today links to My Jobs. Menu: My Jobs opens the list.
- **Tests:** model rules, access (own visits only, other company, other roles), the full day in the browser, partial visits, and hand-off.

## Decisions

- A technician sees only their own visits. Another technician's visit returns "not found", not "forbidden", so it does not confirm that it exists.
- Prices and customer contact details are not shown to technicians.
- Finishing the job needs every task done. A partial visit is how a technician says "more work needed".
- A visit can be started from its scheduled day, not before.
- Finishing a job cancels any visits still booked for it.
- Completion QA (pass or fail, reopen) is the next step; until then a work order waits in QA review.

## Problems avoided

- Starting a visit saves it first, which briefly re-evaluates the work order's scheduled state. The work order is set to In progress afterwards so it ends in the right place.
- Ticking a task checks that the task belongs to the visit's own work order, so a crafted request cannot tick tasks on someone else's job.

## Result

212 tests passing. Ruff and manage.py check clean.

## Next

Completion QA: a dispatcher reviews work in QA review, passes it or reopens it, then invoices and payment status.
