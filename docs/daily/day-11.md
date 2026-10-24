# Day 11 - Completion QA (Fri Oct 16)

## Goals

- Let the office check finished work before it counts as complete.

## Built

- **QA queue and review page.** See [Completion QA](../qa.md).
- **Pass or send back.** Passing completes the work order. Sending back needs notes, can un-tick tasks, and returns the job to In progress.
- **QA history** on the work order page, and a new `workorders_qareview` table in the [Data dictionary](../data-dictionary.md).
- **Dashboard:** Awaiting QA count. Menu: QA opens the queue. Work order page: Review work button.
- **Tests:** model rules, role access, other company, re-opening only own tasks, repeat reviews.

## Decisions

- Only Admin and Dispatcher review. Office/Sales and Finance do not.
- Sending back needs written notes so the crew knows what to fix.
- Reviews are a log, never edited, so the history of a job is complete.

## Problems avoided

- The task list on the review form is limited to the work order being reviewed, so a crafted request cannot re-open someone else's tasks.

## Result

231 tests passing (19 new in test_qa.py). Ruff and manage.py check clean. Migration workorders.0003_qareview applied.

## Next

Invoices and payment status.
