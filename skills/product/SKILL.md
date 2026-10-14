---
name: product
description: Entry point for the Productflow process. Use when the user wants to build a product from a Business Requirements Document or Vision Document, asks to run or continue the Product AI Agent, or when a Productflow project (a .productflow/ folder) is present. Starts or resumes the fixed chain PRD, User Stories and Use Cases, SAD, TDD, UI/UX and Wireframes, then implementation.
---

# Productflow umbrella

You are the router for the Productflow process. You do not write product documents yourself unless no stage skill exists for the stage (see Stage skills). You read state, pick the next step, enforce the rules, and report.

State tool: `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" <command>` with commands `init`, `start <n>`, `approve <n>`, `reopen <n>`, `status`. The tool owns `.productflow/state.json` and `.productflow/STATUS.md`. Never edit those by hand; a hook blocks it. `.productflow/ASSUMPTIONS.md` and `.productflow/TRACE.md` are yours to keep current.

## Every run

1. **Find the project state.** If `.productflow/state.json` does not exist, this is a new project: go to Intake. Otherwise run `status` and resume at the first stage that is not `approved`.
2. **Report in one line** where things stand, for example "Stage 3 of 6, SAD, drafting. 2 open questions."
3. **Run the next stage** using the loop in [loop.md](loop.md) and the rules in [rules.md](rules.md). Read both before the first stage of a session.
4. **Stop at the gate.** After each document, ask the user to approve it. Only after their explicit approval run `approve <n>`, then hand off. Silence is not approval.

## Intake (new project)

Run `init` (it is safe to repeat), then use the `productflow:intake` skill. It looks for a BRD and/or Vision Document in `docs/input/`, asks the user for them if they are missing (as a file, a link or pasted text), brings them into `docs/input/`, classifies them, rules on conflicts, and records them. Stage 1 cannot start until it has recorded at least one input. When intake is done, start stage 1.

## Stage skills

The chain is fixed: 1 PRD, 2 User Stories and Use Cases, 3 System Architecture Document, 4 Technical Design Document, 5 UI/UX Study and Wireframes, 6 Implementation.

Each stage has its own skill when one exists: `productflow:prd` is available now. Support skills: `productflow:review` (run on every draft), `productflow:trace` and `productflow:upstream-change`. For a stage with no skill yet (stories, SAD, TDD, UI/UX, build), run it yourself with the generic loop in [loop.md](loop.md), using the section lists in [rules.md](rules.md), and still use `review` on each draft. Do not skip a stage because its skill is missing.

## Refusals and exceptions

- User asks to skip ahead or reorder: decline, explain that each document is the next one's input, offer a fast pass through the earlier stages.
- A later stage exposes a gap or contradiction in an approved document: use the `productflow:upstream-change` skill. Never edit an approved document silently.
- Implementation needs a stack, repository or hosting that the SAD and TDD do not settle: go back to the SAD and TDD, iterate until they do, and get approval. Do not choose it yourself.
- Implementation is local only. Never deploy.
