# Crewvia

Crewvia is a field service management (FSM) web application. It follows a job from
the first customer contact to a paid invoice: customers, sites, leads and estimates,
work orders, scheduling, the technician's visit, completion QA, and invoicing with
PDF and email. It is built first for a security systems installer, as one trade pack
on a platform meant to serve other field trades later.

## Where it stands

| Area | State |
|---|---|
| Milestone 1 - Foundation | Built. Staging backup restore still to be tested |
| Milestone 2 - Core workflow | Built end to end, more than 360 automated tests passing |
| Beyond Milestone 2 | Time tracking with location and mileage, Pipeline dashboard, installable app, sample data |
| Staging | Live at `https://staging.crewvia.net` with HTTPS and real outgoing email |
| Production | Not yet. See [Production readiness](production-readiness.md) |
| Next | Lead intake and proposals, then real-data readiness (import, price list), then the [agent layer](agents.md) |

## The workflow

```mermaid
flowchart LR
    C[Customer] --> S[Site]
    S --> E[Lead / Estimate]
    E --> W[Work order]
    W --> D[Schedule and dispatch]
    D --> T[Technician workflow]
    T --> Q[Completion QA]
    Q --> I[Invoice]
    I --> M[PDF and email]
    M --> P[Payments]
```

## Where to start

- New to the project: [Local setup](setup.md), then [Architecture](architecture.md).
- Looking for a library or tool: [Technology and libraries](technology.md).
- Crew hours and location: [Time tracking](timetracking.md). Sales view: [Pipeline](pipeline.md). Demo data: [Sample data](sample-data.md).
- Who can do what: [Roles and permissions](permissions.md).
- Running it on a server: [Staging server](staging.md) and [Deployment](deployment.md).
- Email: [Email delivery](email.md).
- Where the product is going: [Product direction](direction.md) and [Agent layer](agents.md).
- What happened and when: [Build log](daily/index.md).
- What is next: [Roadmap](roadmap.md).

!!! note "Scope"
    Milestone 1 and 2 scope is defined by the project agreement. Longer-term
    ideas are listed on the [Roadmap](roadmap.md) and do not expand that scope.
