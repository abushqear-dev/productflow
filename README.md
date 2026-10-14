# Productflow

A Claude Code plugin that runs the whole product-building process: from a Business Requirements Document and/or Vision Document, through five approved documents, to a prototype running locally.

1. PRD
2. User Stories and Use Cases
3. System Architecture Document (SAD)
4. Technical Design Document (TDD)
5. UI/UX Study and Wireframes
6. Implementation (local only, never deployed)

Each document needs your explicit approval before it becomes the next stage's input.

## Status

Step 1 of the roadmap: manifest, umbrella skill, state files, approval-gate hook. The stage skills (`prd`, `stories`, `sad`, `tdd`, `uiux`, `build`), the independent reviewer and the traceability checker come next. Until they exist, the umbrella skill runs each stage with its generic loop.

## Try it locally

```bash
claude --plugin-dir /path/to/this/repo
```

Then, in the project folder you want to build in, put a BRD and/or Vision Document in `docs/input/` and run `/productflow:product`.

## How it works

- **`/productflow:product`** is the only entry point. It reads state, runs the next stage, and enforces the rules.
- **State is files.** `.productflow/state.json` (source of truth), `STATUS.md` (generated), `docs/0N-*.md` (approved documents), `ASSUMPTIONS.md`, `TRACE.md`.
- **A hook blocks skipping.** Writing `docs/0N-*.md` is refused until stage N-1 is approved, approved documents cannot be edited silently, and `app/` is locked until all five documents are approved.
- **`scripts/pf.mjs`** is the state tool: `init`, `start <n>`, `approve <n>`, `reopen <n>`, `status`.

## Roadmap

1. Manifest, umbrella skill, state, gate hook (this step)
2. Templates, `review`, `trace`, `intake`, `prd` (first vertical slice, with evals)
3. Stages 2-5 and the design-researcher subagent
4. Stage 6, wired to Superpowers, with browser verification
5. Stage 0 (vision and market research), remaining evals, fast mode
