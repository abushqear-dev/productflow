# Productflow

A Claude Code plugin that runs the whole product-building process: from a Business Requirements Document (BRD) and/or Vision Document, through five approved documents, to a prototype running locally.

1. PRD
2. User Stories and Use Cases
3. System Architecture Document (SAD)
4. Technical Design Document (TDD)
5. UI/UX Study and Wireframes
6. Implementation (local only, never deployed)

Each document needs your explicit approval before it becomes the next stage's input.

## Install (no cloning)

You need [Claude Code](https://code.claude.com) v2.1 or later. This repository is also a plugin marketplace, so Claude Code installs it straight from GitHub.

**In your terminal:**

```bash
claude plugin marketplace add abushqear-dev/productflow
claude plugin install productflow@productflow-plugins
```

**Or inside a Claude Code session:**

```
/plugin marketplace add abushqear-dev/productflow
/plugin install productflow@productflow-plugins
```

Check it loaded with `claude plugin list`; it should show `productflow@productflow-plugins` as enabled. Restart the session (or run `/reload-plugins`) if the skills do not appear.

**Update:** `claude plugin marketplace update productflow-plugins`, then `claude plugin update productflow@productflow-plugins`.
**Remove:** `claude plugin uninstall productflow@productflow-plugins`, then `claude plugin marketplace remove productflow-plugins`.

## Use it

1. Make a folder for your product and open Claude Code in it (`claude`).
2. Run `/productflow:product`.
3. Provide your BRD and/or Vision Document. You can give them as a **file** (a path, or drop it in `docs/input/`), a **link**, or **pasted text**. If you have not given any, the agent asks. It copies or creates the files in `docs/input/` for you.
4. Answer its questions. At the end of each stage, read the document and reply `approve`, or say what to change.
5. Stages 2 to 6 are still run with the generic loop; their dedicated skills are on the roadmap below.

To pick up later, run `claude` in the same folder and `/productflow:product` again. It resumes from the first stage that is not approved. `/productflow:status` shows where you are.

## What is in the plugin

| Skill | Job |
|---|---|
| `product` | The only entry point. Reads state, runs the next stage, enforces order and approval. |
| `intake` | Collects the inputs (file, link or text), brings them into `docs/input/`, classifies them, rules on conflicts. |
| `prd` | Stage 1: writes the PRD. |
| `review` | Independent review of every draft by a separate `reviewer` subagent, plus the trace check. |
| `trace` | Keeps `TRACE.md` current and checks IDs (FR, NFR, US, UC, C, S) across documents. |
| `upstream-change` | Handles a gap found in an approved document: stop, ask, reopen, re-check. |
| `status` | Shows where the project stands. |

State lives in files in your project: `.productflow/state.json` (source of truth), `STATUS.md` (generated), `docs/input/`, `docs/0N-*.md`, `ASSUMPTIONS.md`, `TRACE.md`, `app/`.

A hook enforces the rules: a stage cannot be written before the previous one is approved, approved documents and the inputs cannot be edited silently, and `app/` stays locked until all five documents are approved.

## Develop it locally

```bash
git clone https://github.com/abushqear-dev/productflow
claude --plugin-dir ./productflow
claude plugin validate ./productflow
```

Evals for the plugin are in `evals/` (run with `claude plugin eval .`). The state tool is `scripts/pf.mjs`.

## Roadmap

1. Manifest, umbrella skill, state, gate hook (done)
2. Templates, `intake`, `prd`, `review`, `trace`, `upstream-change`, first three evals (done)
3. Stages 2 to 5 as skills, design-researcher subagent, wireframes as Claude Artifacts
4. Stage 6, wired to Superpowers, with browser verification
5. Stage 0 (vision and market research), remaining evals, fast mode
