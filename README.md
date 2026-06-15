# Agentno_test

A structured workflow for evaluating human-written code and producing a clean re-implementation — designed to be handed off to a project manager as a ready-to-use fork.

---

## Workflow Overview

```
Human code submitted
        │
        ▼
 human_original/        ← raw code as provided, untouched
        │
        ▼
   evaluation/          ← analysis: quality, issues, patterns, summary
        │
        ▼
new_implementation/     ← clean re-implementation based on evaluation
        │
        ▼
     docs/              ← final reports, README, PM-ready documentation
```

---

## Directory Structure

| Folder | Purpose |
|---|---|
| `human_original/` | Raw human code, copied in as-is. Never modified. |
| `evaluation/` | Analysis documents: code quality summary, identified issues, patterns found. |
| `new_implementation/` | Clean re-implementation based on evaluation findings. |
| `docs/` | Project documentation, final reports, and PM-ready summaries. |

---

## How to Submit Human Code

See [`human_original/SUBMISSION_TEMPLATE.md`](human_original/SUBMISSION_TEMPLATE.md) for instructions on how to provide code for evaluation.

---

## Branching Strategy

- `main` — stable, PM-ready state
- `development` — active work branch
- `feature/*` — individual feature or task branches, PR'd into `development`

---

## Next Steps

1. Place human code in `human_original/` following the submission template.
2. Run evaluation and document findings in `evaluation/`.
3. Implement the clean version in `new_implementation/`.
4. Write final report in `docs/` and merge to `main`.
