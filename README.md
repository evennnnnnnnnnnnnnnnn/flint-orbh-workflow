# Orbh Workflow

A Flint workspace for developing and maintaining the Orbh Workflows framework.

The implementation is an independent repository at
`Workspace/Repos/orbh-workflows/`. Its remote is declared in `flint.toml` and
requires access to that repository. The framework code is not vendored into
this Flint repository.

## Restore

```bash
flint init "Orbh Workflow" --path "$HOME/Documents/Projects" --from-git https://github.com/evennnnnnnnnnnnnnnnn/flint-orbh-workflow.git --no-open
cd "$HOME/Documents/Projects/(Flint) Orbh Workflow"
flint workspace update orbh-workflows
cd Workspace/Repos/orbh-workflows
npm ci --include=dev
npm run verify
```

Node.js >=24 is required. See [workspace instructions](Mesh/%28System%29%20Flint%20Init.md)
and [framework review](Mesh/Orbh%20Workflow%20Framework%20Review.md).

The local extraction preserves the original checkout, Git history, dependencies,
and uncommitted work. A fresh clone restores the implementation remote's committed
revision; it does not reproduce those local uncommitted changes.

Workflow state and Orbh sessions remain in their original application context.
When operating an existing project, keep its original Flint working directory and
external data directory. Relocating this source checkout does not migrate them.
