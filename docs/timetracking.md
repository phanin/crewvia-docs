# Time tracking

Technicians clock in and out from their phone. Office staff review the week, fix
mistakes and export to a spreadsheet. This is **release 1 of time tracking: record
and review**. Live GPS, payroll and offline use come later (see below).

## What a technician does

1. Opens **My Hours** and presses **Clock in**. The phone is asked for its location once.
2. Optionally starts and ends a break.
3. Presses **Clock out**, enters miles driven and an optional note.

Only one shift can be open per technician. The database enforces this, so two taps
or two devices cannot create two open shifts.

## What the office does

**Timesheets** shows one week at a time: hours per technician and per day, breaks,
miles, overtime (hours above 40 in the week), scheduled hours from visits, and time
spent on jobs (from visit start and finish). A **CSV** export opens in a spreadsheet;
cells that could be read as formulas are escaped.

A shift can be corrected by staff who may manage the schedule. A **reason is
required** (up to 100 characters) and is stored in the change history with who made
the change and when. Shifts are never deleted.

## Location

- Location is read **only when Clock in or Clock out is pressed**. Nothing runs in the background.
- If the phone refuses or has no signal, the shift is still recorded and shows "no location".
- Each point stores latitude, longitude and accuracy in metres, and links to a map.
- Wording for staff (what is collected, when, who sees it) must be agreed before real use. See [Production readiness](production-readiness.md).

## Permissions

| Permission | Roles | Purpose |
|---|---|---|
| `time_record_own` | Admin, Technician | Clock in and out, see own hours |
| `time_view_all` | Admin, Dispatcher, Finance | See every technician's timesheet and export |

Editing a shift uses `schedule_manage` (Admin, Dispatcher). See [Roles and permissions](permissions.md).

## Not in release 1

| Item | Why later |
|---|---|
| Live GPS and a map of where the crew is | Needs a native app; a web page cannot track in the background |
| Offline clock in | Needs a native app or a local queue |
| Payroll rules, pay rates, approval chain | Rules differ by company and need the client's decisions |
| Geofencing (auto clock in at a site) | Depends on live location |

## Installing on a phone

The app is a **progressive web app**. It has a manifest and icons, so Chrome offers
**Install app** and iPhone offers **Add to Home Screen**, and it opens full screen.
The service worker is network-only: it exists to make the app installable and shows a
plain offline message when there is no connection. It does not cache data.

## Demo data

`seed_time_demo` fills in recent shifts so the timesheet screens have something to
show. See [Sample data](sample-data.md).
