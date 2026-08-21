# ☕ First Coffee Agent

First Coffee Agent is a lean, instruction-based morning operational check for data engineers.

It uses approved read-only tools to inspect the previous 24 hours, summarize workflow and ingestion health, check freshness and incremental counts, investigate abnormal conditions, and produce a standalone coffee-themed HTML brief.

It is domain-, vendor-, platform-, and assistant-neutral. The workflow applies wherever compatible tools can expose operational metadata. It does not require a particular orchestrator, data platform, cloud, industry, or AI assistant.

This repository contains instructions only: one orchestration profile, five skills, and architecture documentation. It has no application runtime, scheduler, deployment code, bundled connector, or platform-specific configuration.

## What it checks

- Workflows, pipelines, and jobs
- Failures, cancellations, retries, unfinished runs, missed schedules, and unusual duration
- Ingestion, file or object, batch, checkpoint, and load metadata
- Freshness, late-arriving data, and silent successful-but-stale outputs
- Aggregate incremental row, file, event, or byte counts when safely available
- Catalog, dependency, and lineage metadata
- Downstream technical impact and root-cause evidence

The default interval is the previous 24 hours. Request a different interval when needed.

## Safety first

First Coffee Agent is read-only. It must not:

- Write to production systems
- Restart, retry, start, stop, cancel, or repair jobs
- Change schedules, configurations, schemas, permissions, checkpoints, alerts, or infrastructure
- Expose sensitive or regulated data, credentials, raw records, full paths, job parameters, SQL text, or complete logs
- Treat missing observability as evidence of health

Reports contain sanitized, aggregated operational metadata. Root-cause conclusions use only CONFIRMED, PROBABLE, or POSSIBLE.

These are behavioral safeguards, not a technical permission or compliance boundary. Configure every tool connection and data-platform identity with least-privilege, read-only access. Expose only the operational metadata and monitoring capabilities needed for the check, and apply your organization's access, suppression, audit, retention, and disclosure controls.

## Included files

    first-coffee-agent/
    ├── .github/
    │   ├── agents/
    │   │   └── first-coffee-agent.agent.md
    │   └── skills/
    │       ├── pipeline-health/SKILL.md
    │       ├── ingestion-check/SKILL.md
    │       ├── data-freshness/SKILL.md
    │       ├── root-cause/SKILL.md
    │       └── morning-report/SKILL.md
    ├── docs/
    │   └── ARCHITECTURE.md
    ├── README.md
    ├── LICENSE
    └── .gitignore

These files are portable Markdown instructions. Copy or adapt them to the instruction and skill locations supported by your chosen assistant.

## Setup

1. Add or adapt the instruction and skill files in the project where the morning check will run.
2. Configure approved read-only tool connections. Use MCP when it is supported by your chosen assistant, and keep credentials out of this repository.
3. Give the connected identity read-only access to the required operational metadata, catalog, lineage, and monitoring sources.
4. Verify that the exposed tools and identity cannot perform production mutations.
5. Optionally record the critical workflows, datasets, freshness expectations, owners, and timezone that deserve priority in the report.
6. Start a morning check.

The repository deliberately does not configure tool connections. It adapts to the approved tools already available in the active environment.

## Your first morning

    Connect approved read-only tools
            ↓
    Identify critical workflows, datasets, and freshness expectations
            ↓
    Ask First Coffee for a morning check
            ↓
    Pipeline health · ingestion · freshness · root-cause evidence
            ↓
    ☕ Privacy-safe HTML morning briefing

The agent can discover runs and datasets exposed by the connected tools. For the most useful production briefing, provide a small priority map with critical workflows and datasets, expected freshness, timezone, and an owner or escalation path. Do not include raw records, credentials, or sensitive identifiers.

## Run it manually

Start with:

    first coffee

Other useful prompts include:

    morning check
    what happened overnight?
    check the last 24 hours
    first coffee for the last 8 hours

If your assistant supports named agents, select or invoke First Coffee Agent using that assistant's normal mechanism, then send one of the prompts above.

The workflow applies:

1. Pipeline health
2. Ingestion checks
3. Freshness and incremental-count checks
4. Root-cause analysis when abnormalities exist
5. HTML report generation

## Output

The result is a standalone first-coffee-report-YYYYMMDD-HHMMSSZ.html file with:

- An overall operational status
- Priority findings
- Data coverage and visibility gaps
- Workflow, ingestion, freshness, and volume summaries
- Root-cause confidence and supporting evidence
- Read-only suggested next checks

The report uses inline CSS and no JavaScript, remote assets, forms, or trackers. Generated reports are ignored by Git to reduce accidental disclosure. If the environment cannot create and then persist or attach the artifact, the agent returns the complete HTML in one fenced block.

## Integrations

First Coffee Agent depends on capabilities, not brands. Useful read-only tools may provide run history, ingestion or file metadata, dataset update history, safe aggregate counts, catalog and lineage metadata, and sanitized operational diagnostics.

Any compatible toolset can be used. When a capability is unavailable, the report marks it NOT OBSERVED. The agent never invents data or reports an all-clear result from incomplete visibility.

## Architecture

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## License

Licensed under the [MIT License](LICENSE).
