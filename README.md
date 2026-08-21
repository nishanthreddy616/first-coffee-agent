# ☕ First Coffee Agent

First Coffee Agent is a lean, GitHub Copilot custom agent for a data engineer's morning operational check.

It uses whatever read-only MCP and data-platform tools are available to inspect the previous 24 hours, summarize workflow and ingestion health, check freshness and incremental counts, investigate abnormal conditions, and produce a standalone professional coffee-themed HTML brief.

This repository contains instructions only: one agent profile, five skills, and architecture documentation. It has no application runtime, scheduler, deployment code, bundled connector, or platform-specific configuration.

## What it checks

- workflows, pipelines, and jobs;
- failures, cancellations, retries, unfinished runs, missed schedules, and unusual duration;
- ingestion, file or object, batch, checkpoint, and load metadata;
- freshness, late-arriving data, and silent successful-but-stale outputs;
- aggregate incremental row, file, event, or byte counts when safely available;
- catalog, dependency, and lineage metadata;
- downstream technical impact and root-cause evidence.

The default interval is the previous 24 hours. Request a different interval in the prompt when needed.

## Safety first

First Coffee Agent is read-only. It must not:

- write to production systems;
- restart, retry, start, stop, cancel, or repair jobs;
- change schedules, configurations, schemas, permissions, checkpoints, alerts, or infrastructure;
- expose PHI, PII, secrets, raw sensitive rows, full paths, job parameters, SQL text, or complete logs;
- treat missing observability as evidence of health.

Reports contain sanitized, aggregated operational metadata. Root-cause conclusions use only `CONFIRMED`, `PROBABLE`, or `POSSIBLE`.

These are behavioral safeguards, not a technical permission boundary or a claim of HIPAA compliance. Configure every MCP server and data-platform identity with least-privilege, read-only permissions. Expose only the minimum metadata and monitoring capabilities needed for the check, and apply your organization's access, suppression, audit, retention, and disclosure controls.

## Repository layout

```text
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
```

## Setup

1. Add these files to the repository where the agent will run and merge them into its default branch.
2. Configure the desired MCP or data-platform connections in the GitHub Copilot environment. Do not store credentials in this repository.
3. Give the connected identity read-only access to the required operational metadata, catalog, lineage, and monitoring sources.
4. Verify that the exposed tools and identity cannot perform production mutations.
5. Select **First Coffee Agent** in GitHub Copilot and start the morning check.

The repository deliberately does not configure MCP servers. It adapts to the tools already available in the active Copilot environment.

## Run it manually

Where the Copilot surface supports named-agent mentions, use:

```text
@first-coffee-agent first coffee
```

Other prompts include:

```text
@first-coffee-agent morning check
@first-coffee-agent what happened overnight?
@first-coffee-agent check the last 24 hours
@first-coffee-agent first coffee for the last 8 hours
```

Invocation controls vary across Copilot surfaces. If `@first-coffee-agent` is not available, select **First Coffee Agent** from the agent picker or use the surface's `/agent` command, then send `first coffee`. The profile is marked user-invocable and cannot be selected automatically by another agent. See GitHub's [custom agent configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration) and [Agent Skills guide](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills).

The agent applies:

1. pipeline health;
2. ingestion checks;
3. freshness and incremental-count checks;
4. root-cause analysis when abnormalities exist;
5. HTML report generation.

## Output

The result is a standalone `first-coffee-report-YYYYMMDD-HHMMSSZ.html` file with:

- an overall operational status;
- priority findings;
- data coverage and visibility gaps;
- workflow, ingestion, freshness, and volume summaries;
- root-cause confidence and supporting evidence;
- read-only suggested next checks.

The report uses inline CSS and no JavaScript, remote assets, forms, or trackers. Generated reports are git-ignored to reduce accidental disclosure. If the environment cannot create and then persist or attach the artifact, the agent returns the complete HTML in one fenced block.

## Integrations

First Coffee Agent depends on capabilities, not vendors. Useful read-only tools may provide run history, ingestion or file metadata, dataset update history, safe aggregate counts, catalog and lineage metadata, and sanitized operational diagnostics.

For example, a Databricks MCP connection may expose several of these capabilities, but Databricks is not required and no Databricks-specific behavior is built into the agent or skills. Any platform with equivalent read-only capabilities can be used.

When a capability is unavailable, the report marks it `NOT OBSERVED`. The agent never invents data or reports an all-clear result from incomplete visibility.

## Architecture

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## License

Licensed under the [MIT License](LICENSE).
