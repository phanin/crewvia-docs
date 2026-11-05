# Pipeline

A read-only dashboard over leads and estimates, built to answer "what is coming in
and what is it worth?". It replaces the term "CRM" in the product on purpose: it is a
view of the sales pipeline, not a separate contact system.

**Access:** needs both `leads_view` and `estimates_view` (Admin and Office/Sales).
Page: `/pipeline/`.

## What it shows

| Block | Meaning |
|---|---|
| KPI tiles | Open pipeline value, weighted forecast, win rate, leads this period |
| Board | Leads and estimates grouped by stage |
| Forecast | Weighted value of open estimates |
| Lead sources | Where leads come from (case-insensitive, blanks grouped) |
| Top customers | By approved and open estimate value |
| Mix and funnel | Share by stage and conversion down the funnel |
| Trend | Estimates created over recent months |

## How the forecast is weighted

The forecast is a simple, visible rule, not a prediction.

| Estimate status | Weight |
|---|---|
| Draft | 25% |
| Sent | 50% |
| Approved | 100% |

Declined and expired estimates count for nothing. Weights live in one place in code
and will become a company setting once the client sets their own.

## Design notes

- Totals are computed from estimate lines when the page loads; nothing is stored.
- Charts are drawn with plain HTML and inline styles, so there is no chart library and no extra front-end build.
- Data is scoped to the signed-in user's company like every other screen.

## Later

Targets per salesperson, lead assignment and conversion by owner arrive with the
intake work on the [Roadmap](roadmap.md).
