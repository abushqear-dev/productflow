---
name: prd
description: Stage 1 of Productflow. Writes the Product Requirements Document (PRD) from the registered Business Requirements Document and/or Vision Document. Use when the umbrella skill reaches stage 1, or when the user asks to write or revise the PRD for a Productflow project.
---

# Stage 1: Product Requirements Document

Run this stage with the loop in `${CLAUDE_PLUGIN_ROOT}/skills/product/loop.md` and the rules in `${CLAUDE_PLUGIN_ROOT}/skills/product/rules.md`. Read both first.

## Before you start

- Stage 1 needs at least one recorded input. If `docs/input/` has none, stop and use the `productflow:intake` skill. The state tool refuses to start stage 1 without one.
- Read **every** recorded input in full (`state.json` lists them). If both a BRD and a Vision Document exist, read the recorded ruling and follow it. Do not re-decide it.
- Run `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" start 1`.

## Steps

1. **Stage Skill Instruction.** Fill in `${CLAUDE_PLUGIN_ROOT}/templates/stage-instruction.md` for this product. Show it, short.
2. **Draft** `docs/01-prd.md` from `${CLAUDE_PLUGIN_ROOT}/templates/prd.md`. Rules for content:
   - State at the top which source it is based on (BRD, Vision, or both).
   - Every FR-n and NFR-n names where it comes from: an input file and section, a user answer, or an assumption ID. If you cannot name a source, it is an assumption: label it and add it to `.productflow/ASSUMPTIONS.md`. Do not add features the inputs never mention.
   - A requirement with no input sentence or user answer behind it, only an assumption, gets priority `proposed` and is also listed under "Proposed additions" in section 5. Keep those few and keep the rest within what the inputs say.
   - IDs are FR-1, FR-2, ... and NFR-1, ... in order, never reused.
   - Write requirements a tester could check. Replace "fast" with a measure, or ask.
3. **Review.** Use the `productflow:review` skill on the draft, and fix what it finds.
4. **Ask**, once, in a numbered batch, only what the inputs do not answer and that blocks a section or changes scope. Each question: the question, why it matters, your suggested default.
5. **Approve.** Show the PRD and ask: "Do you approve the PRD? Reply approve, or tell me what to change." On approval run `pf.mjs approve 1`, set the PRD's `open` assumptions to `accepted` (not `confirmed`) in `.productflow/ASSUMPTIONS.md`, then `productflow:trace` to seed `.productflow/TRACE.md`, then say "Handing off to Stage 2: PRD".

## Done when

Every required section is complete, every item in the inputs is covered or listed as out of scope, nothing contradicts the governing input, and every remaining unknown is asked or recorded as an assumption.
