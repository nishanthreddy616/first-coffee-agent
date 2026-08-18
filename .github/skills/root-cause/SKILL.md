---
name: root-cause
description: >-
  Investigate abnormal workflow, ingestion, and freshness findings with
  vendor-neutral read-only evidence. Use to correlate incidents, build
  technical timelines, assess lineage-backed impact, test competing
  hypotheses, and assign CONFIRMED, PROBABLE, or POSSIBLE RCA confidence.
---

# Root Cause

Investigate only abnormal findings produced by the preceding assessments. If no abnormal finding exists, return `RCA not required` and do not query additional tools.

## Procedure

1. Cluster findings that share a resource, upstream dependency, failure class, or overlapping time.
2. For each cluster, construct a sanitized timeline containing:
   - last known healthy state;
   - first abnormal signal;
   - relevant upstream and downstream events;
   - retries or recovery;
   - latest observed state.
3. Use the least-privileged available evidence:
   - run and attempt metadata;
   - sanitized error class or code;
   - load, checkpoint, and manifest metadata;
   - aggregate count changes;
   - publish or freshness timestamps;
   - catalog, dependency, and lineage metadata;
   - read-only change history when safely exposed.
4. Form explicit, testable hypotheses. Record evidence that supports and contradicts each one.
5. Reject hypotheses that conflict with verified timestamps, dependencies, or system state.
6. Distinguish the initiating cause from symptoms, retries, and downstream consequences.
7. Use lineage to identify technical impact. If lineage is absent or incomplete, mark impact as unassessed rather than inferring it.
8. Merge duplicate symptoms into one incident and cross-reference the original finding identifiers.

## Confidence labels

Use exactly one of these labels for each supported cause hypothesis:

- `CONFIRMED`: authoritative evidence directly demonstrates the causal event, affected resource, and matching time, with no material contradictory evidence. Timing correlation or a generic error alone is insufficient.
- `PROBABLE`: multiple consistent signals support the hypothesis, or one strong signal aligns with a verified dependency and timeline, but the causal link is not directly demonstrated.
- `POSSIBLE`: the hypothesis fits limited or indirect evidence and requires validation; credible alternatives may remain.

If the evidence does not support even a plausible hypothesis, do not force a label. Report `Root cause undetermined` and list the missing evidence. Never express confidence as a percentage.

## Safety

- Do not retrieve raw rows, raw logs, SQL text, payloads, secrets, or sensitive values. Paraphrase evidence and retain only safe codes and references.
- Do not restart or retry jobs, replay ingestion, alter configurations, repair data, acknowledge incidents, or perform any platform write.
- Limit suggested next steps to read-only validation, safe evidence collection, and human-owner handoff. State that no remediation was executed.
- Do not infer patient, clinical, customer, legal, or financial impact from technical metadata.

## Output

For each incident cluster, return:

- an `RC-###` incident identifier and related finding identifiers;
- current disposition and a sanitized technical timeline;
- leading cause hypothesis and its allowed confidence label, or `Root cause undetermined`;
- supporting and contradicting evidence;
- verified or unassessed downstream technical impact;
- competing hypotheses, unknowns, and coverage limits;
- safe read-only follow-up or owner handoff.
