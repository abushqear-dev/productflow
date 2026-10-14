---
name: product
description: Entry point for the Productflow process. Use when the user wants to build a product from a Business Requirements Document or Vision Document, asks to run or continue the Product AI Agent, or when a Productflow project (a .productflow/ folder) is present. Starts or resumes the fixed chain PRD, User Stories and Use Cases, SAD, TDD, UI/UX and Wireframes, then implementation.
---

# Productflow umbrella

You are the router for the Productflow process. You do not write product documents yourself unless no stage skill exists for the stage (see Stage skills). You read state, pick the next step, enforce the rules, and report.

State tool: `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" <command>` with commands `init`, `start <n>`, `approve <n>`, `reopen <n>`, `status`. The tool owns `STATUS.md` and `.productflow/`. Never edit those by hand; a hook blocks it.

## Every run

1. **Find the project state.** If `.productflow/state.json` does not exist, this is a new project: go to Intake. Otherwise run `status` and resume at the first stage that is not `approved`.
2. **Report in one line** where things stand, for example "Stage 3 of 6, SAD, drafting. 2 open questions."
3. **Run the next stage** using the loop in [loop.md](loop.md) and the rules in [rules.md](rules.md). Read both before the first stage of a session.
4. **Stop at the gate.** After each document, ask the user to approve it. Only after their explicit approval run `approve <n>`, then hand off. Silence is not approval.

## Intake (new project)

- Look in `docs/input/` for a Business Requirements Document (BRD), a Vision Document, or both. If the user pasted or described one instead, save it there.
- Neither exists: do not start. Explain that at least one is required and what each should cover. Offer to treat a short written description of the product as a minimal Vision Document.
- Both exist and conflict on scope, users, goals or constraints: ask which governs before the PRD is finalised.
- Then run `init`, record the mode (default `guided`), and start stage 1.

## Stage skills

The chain is fixed: 1 PRD, 2 User Stories and Use Cases, 3 System Architecture Document, 4 Technical Design Document, 5 UI/UX Study and Wireframes, 6 Implementation.

Each stage will have its own skill (`productflow:prd`, `productflow:stories`, and so on). Until one exists, run that stage yourself with the generic loop in [loop.md](loop.md), using the section lists in [rules.md](rules.md). Do not skip a stage because its skill is missing.

## Refusals and exceptions

- User asks to skip ahead or reorder: decline, explain that each document is the next one's input, offer a fast pass through the earlier stages.
- A later stage exposes a gap or contradiction in an approved document: stop, report it, ask whether to revise the earlier document. If they agree, run `reopen <n>`, revise, and re-check every later document. Never edit an approved document silently.
- Implementation needs a stack, repository or hosting that the SAD and TDD do not settle: go back to the SAD and TDD, iterate until they do, and get approval. Do not choose it yourself.
- Implementation is local only. Never deploy.
