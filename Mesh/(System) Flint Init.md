---
id: 5ee080d0-95de-4c1d-a1da-4676b4771766
tags:
  - "#f/init"
template: "[[tmp-f-flint_init-v0.1]]"
authors:
  - "[[@Even]]"
orbh-sessions:
  - "[[f81b7003-0a65-4575-b9be-ad891a73ce62]]"
---

# Orbh Workflow

This Flint maintains the reusable Orbh Workflows framework: a durable workflow
controller, CLI, and local operator UI. The controller owns project decisions,
artifact reviews, approval gates, and bounded progression. Orbh owns agent
sessions, execution, turn results, and telemetry. Application research and its
live data belong in the application Flint.

## Repository overview

The independent TypeScript/ESM repository is at
`Workspace/Repos/orbh-workflows/`, declared in `flint.toml`.
Read its `AGENTS.md`, `README.md`, `docs/PLAN.md`, and
`docs/ORBH-INTEGRATION.md` before working on it.

- `src/core/`: SQLite event ledger, state folding, validation, and gates.
- `src/orbh/`: public CLI/HTTP adapter, dispatch, recovery, and notifications.
- `src/cli/`: operator and agent commands, identity checks, and status.
- `src/ui/` and `ui/`: local operator server and browser UI.
- `workflows/`: declarative workflow definitions.
- `docs/`: design, integration contract, usage, and pilot evidence.

Use Node >=24. From the repository, `npm ci --include=dev` installs dependencies
and `npm run verify` runs typecheck, lint, tests, and build.

The Flint repository and implementation repository are independent Git roots.
`Workspace/Repos/` is excluded from the Flint repository. Keep runtime ledgers,
credentials, transcripts, and application artifacts out of both repositories.

## Operating existing workflows

The former checkout path and references in the Artwork Valuation Engine Flint
have been removed. Invoke the CLI and UI using this Flint's checkout path.
For existing workflows, keep their original working directory and data-directory
settings: moving code does not migrate Orbh sessions or workflow ledgers. Run a UI from an ordinary operator terminal
for operator actions; agent-started UIs remain read-only.

# Entry

- [[Orbh Workflow Framework Review]] — extraction findings, verification, and limits.
- [Framework README](../Workspace/Repos/orbh-workflows/README.md)
- [CLI usage](../Workspace/Repos/orbh-workflows/docs/USAGE.md)
- [Local UI](../Workspace/Repos/orbh-workflows/docs/LOCAL-UI.md)
