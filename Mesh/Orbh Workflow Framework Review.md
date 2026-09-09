---
id: c3ce8fe3-b040-4ce3-953d-0c040959a205
tags:
  - "#note"
template: "[[tmp-f-note-v0.1]]"
authors:
  - "[[@Even]]"
orbh-sessions:
  - "[[f81b7003-0a65-4575-b9be-ad891a73ce62]]"
---

# Orbh Workflow Framework Review

Reviewed 2026-09-09 for extraction into a dedicated Flint. This is a targeted
architecture and relocation review, supported by the complete automated suite;
it is not an exhaustive independent correctness audit.

## Findings

The framework was already a standalone TypeScript/ESM package. Extraction needs
no package split or new framework. It has its own Git history, lockfile, build,
CLI, browser UI, tests, and declarative workflow definition. Dependencies install
without the Orbh monorepo, model harness, or valuation engine.

The ownership boundaries are appropriate:

| Concern | Owner and implementation |
| --- | --- |
| Durable workflow state | `src/core/ledger.ts`: SQLite events, revision checks, command deduplication |
| Stage transitions and evidence | `src/core/controller.ts`, `gates.ts`, `artifacts.ts`, `schemas.ts` |
| Agent execution and exact run results | Orbh, reached through `src/orbh/cli-port.ts` and reconciliation |
| Operator and worker commands | `src/cli/main.ts` and `commands.ts` |
| Local operator UI | `src/ui/server.ts` and `ui/`, using the same controller/CLI |

Dispatch records intent before sending and reconciles uncertain outcomes instead
of assuming a missing search result means nothing launched. Approval wake attempts
are separate from the committed decision. The tests exercise stale approvals,
owner replacement, cancellation, duplicate submissions, recovery, and UI boundaries.

**Relocation defect found and fixed:** the UI entry-point guard compared the
invoked filename with the resolved module filename. Launching through the old
absolute path after replacing the directory with a symlink silently exited 0.
The guard now resolves the invoked path with `realpathSync`. A regression test
launches a symlinked entry point and verifies its startup validation executes.
A separate smoke check started the compiled UI through the old absolute path and
successfully read its HTTP configuration.

## Extraction

The existing checkout moved into this Flint at `Workspace/Repos/orbh-workflows/`.
Its original private implementation remote remains unchanged. All 81 tracked and
untracked source files matched their pre-move SHA256 values before the documented
README/startup compatibility edits. The Git HEAD and complete dirty status also
matched. Git history, local changes, dependencies, and generated output moved with
it; the original framework HEAD was `6365126`.

The Artwork Valuation Engine Flint now declares a codebase reference and a Flint
reference. Its former workspace path remains a compatibility symlink for running
processes and existing commands. Workflow ledgers, owner sessions, submitted
artifacts, and research datasets remain in their original application context.
The existing UI on port 4318 returned HTTP 200 before and after relocation.

This Flint's Git repository tracks workspace configuration and documentation;
`Workspace/Repos/`, credentials, and local runtime state are excluded. The private
implementation repository is independently versioned. Local uncommitted framework
changes remain uncommitted; a remote clone does not yet include them.

## Verification

After the compatibility fix, `npm run verify` passed:

- TypeScript typecheck and Biome checks.
- All **367 tests**, with no failures, skips, or cancellations.
- A clean TypeScript build and bundled UI assets.
- Additional compiled-UI startup through the compatibility symlink, using an
  isolated temporary state directory and preserving agent read-only identity.

No real workflow was started, approved, cancelled, or rebound by this review.
The existing server remains running; restart it normally when ready to load the
new build. Keep its original Flint working directory and external state settings.

## Existing limits

- Workers run serially per project; `maxLiveWorkers` is constrained to one.
- Native limits depend on proven runtime capabilities; unavailable cost is unknown,
  and there is no hard wall-clock execution cap.
- Actor checks and the loopback UI assume a trusted local operator environment.
- Source code location is independent of the invoking Flint context. Starting the
  same project from a different Flint does not migrate its Orbh sessions.

## Links

- [Framework](../Workspace/Repos/orbh-workflows/README.md)
- [Orbh integration contract](../Workspace/Repos/orbh-workflows/docs/ORBH-INTEGRATION.md)
- [Local UI](../Workspace/Repos/orbh-workflows/docs/LOCAL-UI.md)
