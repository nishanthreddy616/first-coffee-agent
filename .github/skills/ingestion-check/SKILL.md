---
name: ingestion-check
description: >-
  Assess vendor-neutral ingestion health using read-only load, manifest,
  checkpoint, file or object, and aggregate count metadata. Use to detect
  missing or late arrivals, partial loads, duplicates, rejection spikes,
  stalled checkpoints, backlogs, schema events, and abnormal increments.
---

# Ingestion Check

Assess inbound data movement within the requested interval without inspecting raw records or file contents.

## Procedure

1. Discover expected feeds, batches, files, messages, or loads from available schedules, manifests, checkpoints, catalogs, or user-provided expectations.
2. Retrieve only operational metadata such as:
   - sanitized source and target identifiers;
   - expected and observed arrival times;
   - batch, manifest, or load identifiers;
   - aggregate object, byte, and record counts;
   - load outcome;
   - checkpoint position or backlog size;
   - aggregate accepted, rejected, quarantined, and deduplicated counts;
   - schema version or contract event.
3. Detect:
   - expected arrivals that are missing or late;
   - partial or incomplete loads;
   - duplicate or replayed batches;
   - rejection or quarantine anomalies;
   - stalled checkpoints or growing backlogs;
   - schema or contract drift;
   - zero or abnormal incremental counts.
4. Select count expectations in this order:
   - explicit contract, schedule, or service objective;
   - a comparable documented or historical baseline for the same feed and cadence;
   - the immediately preceding equivalent window;
   - otherwise mark the expectation `UNKNOWN`.
5. Compare like-for-like windows. Account for cadence, weekday, seasonality, and known late-arrival tolerance only when the evidence supports them.
6. Do not call a zero increment abnormal unless a nonzero increment was expected.
7. Use aggregate, interval-bounded count queries only when the operation is verifiably read-only and cannot return raw rows.
8. Correlate ingestion anomalies with workflow findings through safe identifiers, declared dependencies, and timestamps.

## Safety

- Do not open or sample file contents or retrieve message bodies, record payloads, raw rejected rows, full storage paths, or sensitive field values.
- Do not reproduce filenames or object keys that may contain sensitive identifiers; aggregate and label them safely.
- Do not group counts by people, patient identifiers, or sensitive attributes. Apply any organization-defined small-count suppression policy.
- Do not advance checkpoints, replay batches, move files, repair loads, or change ingestion configuration.
- Exclude and do not reuse any PHI, PII, secret, or sensitive value returned unexpectedly.

## Output

Return:

- assessed feeds and comparison baselines;
- arrival, load, backlog, rejection, and incremental-count summaries;
- normalized findings with `IN-###` identifiers;
- cross-references to related pipeline findings;
- coverage records and missing expectations.

Distinguish `no events observed` from `unable to observe events`.
