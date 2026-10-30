# Day 13 - Demo fixes and Windows (Tue Oct 27)

## Goals

- Fix what the end-to-end demo showed.
- Make sure a second developer can run the project on Windows.

## Built

- **One estimate per lead.** Clicking Create estimate again on a lead that already has a live estimate opens that estimate instead of making a second. A lead whose estimate was rejected can get a new one.
- **Default tax rate.** New estimates start with the rate set in `DEFAULT_TAX_RATE` (in `.env`, default `0.00`). Staff can still change it on each estimate. No database change.
- **Line endings.** A `.gitattributes` file keeps shell scripts and Python files as LF, so Docker containers run them correctly when the repo is cloned on Windows.
- **Windows setup page.** See [Setup on Windows](../windows.md).

## Decisions

- **Start time left as it is.** A technician can start a visit from the day it is scheduled, not only at the booked hour. Crews often arrive early, so this stays. It can become a company setting later.
- The default tax rate is a setting, not a field on the company yet. One rate is enough until a client needs different rates per site.
- Typos seen in the demo ("recommandation", "Instalation") were typed data, not code. Nothing to fix.

## Problems avoided

- Windows can turn shell scripts into CRLF files, which fail inside Linux containers with `exec format error`. The `.gitattributes` file prevents it.
- Folder names set the Docker project name, so a developer still in a folder with the old name saw old container names. The Windows page tells them to rename the folder and recreate the database.

## Result

277 tests passing (3 new in `apps/sales/tests/test_views.py`). Ruff, check and migration check clean. No migration. Person B ran the stack on Windows.

## Next

Staging server: choose a host, point `staging.crewvia.net` at it, deploy, and test a backup restore.
