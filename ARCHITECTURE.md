# Architecture and implemented boundaries

Adapted from canonical `architecture/ANALYTICAL_SYSTEM_DESIGN.md` and the project source/release documentation; this is a newly authored summary.

The CPU-first pipeline harmonizes historical NYC EMS into borough/time panels, evaluates an EMS-only historical mean with temporal splits, adds empirical intervals and drives bounded scenario/reliability policy outputs. React consumes aggregate product contracts. NOAA is contextual rather than a retained forecast feature. Census and vulnerability joins are not completed components. Capacity is hypothetical; no live dispatch, staffing optimizer, clinical inference or causal engine is implemented.

The public-friendly Mermaid diagram is in the [README](README.md). It depicts implemented components, not proposed future infrastructure.

[Return to the case study](README.md).
