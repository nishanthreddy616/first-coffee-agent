---
name: data-freshness
description: >-
  Assess vendor-neutral dataset and data-product freshness from read-only
  publish, load, commit, partition, catalog, lineage, and approved aggregate
  timestamp metadata. Use to find stale outputs, freshness risk, silent
  successful-but-stale pipelines, and upstream freshness propagation.
---

# Data Freshness

Assess whether expected datasets and data products are current at the end of the requested interval.

## Procedure

1. Select in-scope outputs from user input, workflow outputs, catalog metadata, monitors, or lineage metadata.
2. Prefer freshness signals in this order:
   - explicit publish timestamp and freshness service objective;
   - successful load, commit, or materialization metadata;
   - expected partition or object arrival metadata;
   - maximum approved technical ingestion timestamp from an aggregate-only query;
   - catalog modification time as a weak proxy, clearly labeled.
3. Do not use workflow success alone as proof of fresh data.
4. For each asset, calculate when supported:
   - latest safe freshness timestamp;
   - age at the assessment end time;
   - expected cadence and allowed delay;
   - freshness lag relative to the stated threshold;
   - aggregate increment for the interval and its explicit comparison baseline.
5. Normalize freshness as:
   - `FRESH`: within an explicit or evidence-backed threshold;
   - `AT RISK`: approaching the threshold or showing a supported degrading trend;
   - `STALE`: beyond the threshold;
   - `UNKNOWN`: no reliable signal or expectation.
6. Detect silent failures, including successful workflows whose outputs did not advance or whose expected aggregate increment was absent.
7. Compare upstream and downstream timestamps or counts only when dependency and measure semantics support the relationship.
8. Distinguish append increments from total snapshot counts. Never subtract snapshots unless their meaning and comparability are known.
9. Treat clock skew, timezone ambiguity, or future timestamps as data-quality findings, not fresh evidence.
10. Do not infer a freshness objective from platform defaults.

## Safety

- Prefer system-generated ingestion or publish timestamps.
- Never retrieve raw rows. Do not use a sensitive business-event timestamp unless it is explicitly approved for operational aggregation.
- Never expose exact values that could identify a person or sensitive event. Apply any organization-defined small-count suppression policy.
- Do not refresh, rebuild, optimize, repair, or alter any dataset.
- Exclude and do not reuse any PHI, PII, secret, or sensitive value returned unexpectedly.

## Output

Return:

- assessed assets and freshness signals;
- timestamp source and evidence strength;
- age, threshold, lag, increment, baseline, and normalized state when available;
- normalized findings with `DF-###` identifiers;
- upstream or downstream correlations supported by lineage;
- coverage records and unresolved timezone, cadence, or expectation gaps.

Use `UNKNOWN` rather than inventing a threshold, count, or health conclusion.
