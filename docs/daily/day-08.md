# Day 8 - Work orders (Wed Oct 14)

## Goals

- Turn an approved estimate into a work order the office can hand to the crew.

## Built

- **New `workorders` app** with two tables: WorkOrder and WorkOrderItem. See the [Data dictionary](../data-dictionary.md).
- **Create from estimate:** a button on an approved estimate. It copies the customer, site, title, notes and each line as a task, without prices. See [Work orders](../workorders.md).
- **Screens:** work order list with search and status filter, detail, and edit.
- **Cancel** for open and scheduled work orders.
- **Rules:** one work order per estimate, only from approved estimates, numbers per company (WO-0001), company scoping.
- **Permissions:** uses the work order permissions from Day 3. Dispatchers can edit and cancel; Office/Sales can create.
- **Menu:** Work Orders now opens the list.
- **Change history** on work orders. Admin site is browse-only.
- **Styles:** stylesheet rebuilt.
- **Tests:** new service and view tests.

## Decisions

- Prices are not copied to the work order, so dispatchers and crews never see them.
- All six statuses are defined now; only Open and Cancelled are used until scheduling and completion are built.
- The estimate row is locked while the work order is created, so a double click cannot make two.
- A work order always has a site, because the estimate always has one.

## Problems avoided

- Creating from a draft or rejected estimate is refused with a plain message instead of an error page.
- A work order page only links to its estimate for roles that may see estimates.

## Result

Fill in after running the checks.

## Next

Scheduling and dispatch: assign a technician and a time window to a work order.
