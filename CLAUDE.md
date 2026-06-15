# CLAUDE.md

## Project Purpose

Evaluate human-written code and produce a clean re-implementation. Final output is a PM-ready fork.

## Workflow

1. Human code goes into `human_original/` — never modify it after submission.
2. Write evaluation findings in `evaluation/`.
3. Implement clean version in `new_implementation/`.
4. Write handoff docs in `docs/`.

## Branching

- Branch from `development` using `feature/<topic>`.
- PR into `development`, not `main`.
- `main` is only updated when a full cycle is complete and PM-ready.

## Commit Style

- Short, imperative subject line (e.g. `add evaluation summary for module X`)
- Commit related files only — do not bundle unrelated changes.

## What Not To Do

- Do not modify files in `human_original/` after they are placed there.
- Do not implement logic before evaluation is complete.
- Do not merge directly to `main` mid-cycle.
