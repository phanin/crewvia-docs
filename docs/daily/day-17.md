# Day 17 - Documentation brought up to date (Mon Nov 2)

## Goals

- Make the documentation match what is built, and record where the product is going.

## Built

- **Renamed to Crewvia** in every page that still used the old name (Day 1 keeps it as history).
- **Rewrote** the home page and roadmap to show real status: staging live, whole core workflow built, what comes next.
- **Updated** architecture (all apps, outgoing email, planned agent app), technology (reportlab, Resend, Oracle Cloud, GoDaddy), deployment (current staging, no open decision, new problems), setup, security baseline, data model and data dictionary (billing and email tables), QA, conventions and troubleshooting.
- **Acceptance checklist** ticked where covered, with a new Milestone 2 walk from lead to payment.
- **New pages:** [Product direction](../direction.md), [Agent layer](../agents.md), [Production readiness](../production-readiness.md).

## Decisions

- The site is written to be safe if public: technical direction only, no pricing, market or client details.
- Open items stay visibly open (backup restore, rotation, two-factor) instead of being hidden.

## Result

Site builds with no broken links.

## Next

Customer import and price list, then the first agent.
