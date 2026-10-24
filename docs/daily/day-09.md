# Day 9 - Scheduling and dashboard (Thu Oct 15)

## Goals

- Let a dispatcher put a technician and a time on a work order.
- Make the dashboard cards show real numbers.

## Built

- **`Visit` table** in the `workorders` app. See the [Data dictionary](../data-dictionary.md).
- **Schedule page** with a week view, technician filter and a waiting list. See [Scheduling and dispatch](../schedule.md).
- **Book, edit and cancel visits**, with a clear message when a technician is already booked.
- **Work order status follows the visits:** Open to Scheduled when booked, and back when the last visit is cancelled. Cancelling a work order cancels its booked visits.
- **Work order page** lists its visits.
- **Dashboard:** leads, estimates, active work orders, visits today and my jobs today are now real counts, scoped to the company and role.
- **Menu:** Schedule opens the schedule page.
- **Change history** on visits.
- **Tests:** model rules, access by role, company separation, week grouping, the waiting list and dashboard counts.

## Decisions

- Overlap is checked in the app, not by the database. A database rule would need an extra PostgreSQL extension; the risk (two dispatchers saving the same slot in the same second) is small for now. Worth adding later if the team grows.
- Times use the project time zone (`America/Phoenix`). The company has a time zone field, but using it per request is a later step.
- Visits are never deleted. A cancelled visit stays visible on the work order.
- Office/Sales can see the schedule but not change it, matching the permission matrix.
- A visit that has started cannot be edited or cancelled from the schedule.

## Problems avoided

- A new visit needs its work order before validation runs, otherwise the overlap check cannot work. The view sets it on the form's instance first.
- The technician list is built from the company and the role, so a user from another company cannot be picked even by editing the form.

## Result

Fill in after running the checks.

## Next

Day 10: the technician's own screens, My Jobs, so they can start a visit, tick tasks and finish.
