# Day 18 - Time tracking, Pipeline and demo (Tue Nov 3 - Wed Nov 4)

## Goals

- Add time and mileage tracking with location, show a sales pipeline, and be ready for the client demo.

## Built

- **Time tracking:** clock in and out, breaks, miles, location at the two button presses, weekly timesheet with overtime, CSV export, corrections with a required reason. See [Time tracking](../timetracking.md).
- **Permissions:** `time_record_own` and `time_view_all`, with a core migration.
- **Installable app:** manifest, icons and a minimal service worker so phones can install it.
- **Pipeline dashboard:** KPIs, board, weighted forecast, sources, top customers, funnel and trend. See [Pipeline](../pipeline.md).
- **Sample data:** `seed_sample_data` and `seed_time_demo`. See [Sample data](../sample-data.md).
- **Dashboard:** time cards added; the old "Unpaid invoices" placeholder removed.

## Decisions

- Release 1 of time tracking is **record and review**. Live GPS, native app, offline and payroll come later.
- "Pipeline" replaces "CRM" in the product.
- Growth follows a **core or pack** rule so the product stays trade-agnostic ([Product direction](../direction.md)).
- Search, notification and settings controls from the mock-up were left out.

## Problems and fixes

- A non-numeric location value (`nan`) crashed parsing; now rejected cleanly.
- Lead source names differing only by case were counted twice; now merged.
- The demo seed skipped users who already had shifts; it now works day by day.
- Chrome did not offer "Install"; adding the service worker fixed it.

## Result

Client demo held on Wed Nov 4. Full test suite passes (more than 360 tests).

## Next

Client call produced a clear order of work: intake templates and a public form,
accept link with a proposal, lead assignment, email-in, then plan upload and
accounting. See [Roadmap](../roadmap.md). Security hardening is scheduled right
after the demo ([Production readiness](../production-readiness.md)).
