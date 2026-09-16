# Repository Conventions

This document defines naming, ticketing, and documentation conventions used throughout the Linux administration lab.

The goal is consistency: a reader should be able to understand what a system belongs to, what kind of work is being documented, and how important an issue is without needing prior knowledge of the lab.

## Unifying Themes

Each managed environment uses a distinct naming theme.

The theme provides a human-readable namespace: a system's name should give useful context about the environment to which it belongs.

Current examples include:

* **Task Force 27 (TF27):** Systems use naval vessel classifications or related nautical names, such as `carrier`, `cutter`, `destroyer`, `frigate`, and `corvette`.
* **Spain environment:** Systems use Spanish city names, such as `sevilla`.

The naming themes are intended to improve recognition and memorability rather than replace technical documentation.

A system's environment, platform, role, operating system, and status should still be documented explicitly.

## Environment Names

Environment names identify distinct groups of managed systems.

`Task Force 27`, commonly abbreviated `TF27`, is the primary home Linux and IT administration lab.

The word **fleet** may occasionally be used informally when referring collectively to TF27 systems. Formal technical documentation should prefer terms such as `environment`, `systems`, `hosts`, or `endpoints` where greater precision is useful.

## Hostnames

Hostnames should:

* Follow the unifying theme assigned to their environment.
* Be short enough for convenient command-line use.
* Be easy to distinguish verbally and in documentation.
* Remain stable unless there is a technical reason to rename the system.

Themed hostnames are identifiers. They should not replace conventional technical terminology for system roles, status, incidents, or administrative actions.

## Ticket Types

Tickets document work performed in the lab.

### `INC` — Incident

Incident tickets record unplanned operational problems requiring investigation, mitigation, or resolution.

Examples include:

* Unexpected system behaviour
* Hardware failures
* Network anomalies
* Service outages
* Security-related events

Incident numbering uses the format:

`INC-###`

Example:

`INC-002-carrier-power-instability.md`

### `LAB` — Lab Work

Lab tickets record planned deployments, implementations, experiments, and structured administrative work.

Examples include:

* Operating-system deployments
* Service implementations
* Planned configuration changes
* Training exercises
* Controlled experiments

Lab numbering uses the format:

`LAB-###`

Example:

`LAB-001-destroyer-deployment.md`

## Ticket Priority

Ticket priority indicates how quickly work should receive attention.

The lab uses four priority levels:

* **P1 - Urgent:** Immediate attention is required. The issue may threaten data integrity, hardware, availability, security, or other important operations.
* **P2 - High:** The issue materially affects a system or important objective and should be addressed promptly.
* **P3 - Medium:** Normal operational or administrative work that should be addressed but is not currently causing serious disruption.
* **P4 - Low:** Minor issues, cosmetic problems, documentation improvements, or work with little operational impact.

Priority reflects the urgency of response, not necessarily the technical complexity of the issue.

## Status Terminology

Formal documentation should use clear, conventional IT terminology.

Examples include:

* `Operational`
* `Maintenance required`
* `Investigating`
* `Monitoring`
* `Resolved`
* `Pending`

Status terms should describe a system's operational condition directly and should remain independent of the naming theme used by its environment.

## Documentation Principles

Repository documentation should:

* Distinguish observed facts from assumptions and hypotheses.
* Avoid claiming a root cause before evidence supports it.
* Cross-reference systems, hardware inventories, and related tickets where appropriate.
* Record significant administrative work as it occurs rather than reconstructing it entirely afterward.
* Use professional terminology understandable to readers outside the lab.
* Preserve enough personality to make environments memorable without requiring readers to understand private jokes or informal terminology.
