# The stage loop

Every document stage runs the same loop. Stage 6 reuses it with an implementation instruction in place of a document.

1. **Read the stage input.** Stage 1: BRD and/or Vision Document. Stage 2: the PRD. Stage 3: the User Stories and Use Cases. Stage 4: the SAD. Stage 5: the TDD. Stages 2-5 also use every earlier document plus the original BRD/Vision.
2. **Write a Stage Skill Instruction.** A short brief specific to this product, built only from the inputs. It states: the document's purpose; the sections it must contain; what it carries forward from earlier documents; product-specific focus areas; the checks the finished document must pass. Do not add requirements the inputs do not support.
3. **Draft the document** at the stage's file path, following the instruction. Run `start <n>` first.
4. **Iterate.** Review the draft against the instruction and every earlier document, then revise. Stop when every required section is complete, every item from the stage input is covered, nothing contradicts an earlier document, and every remaining unknown is asked or recorded as an assumption.
5. **Ask the user, if needed.** One numbered batch per stage. For each question: the question, why it matters, your suggested default. Ask only what the inputs do not already answer. Fold answers in and iterate again.
6. **Ask for approval.** Show the document and ask: "Do you approve <document>? Reply approve, or tell me what to change." Changes requested: revise and ask again.
7. **Hand off.** Only after explicit approval: run `approve <n>`, then say "Handing off to Stage N+1: <document>". After stage 5 say "Document set complete: starting implementation."

## Response format for each stage

1. Header: "Stage N of 6: <document name>".
2. The Stage Skill Instruction (short).
3. The document.
4. Assumptions: what you assumed and why. Also add each to `.productflow/ASSUMPTIONS.md`.
5. Open questions, if any.
6. The approval request.

Stage 6 reports instead: what was built, how to run it locally, which US-n it covers and passes, what is left, assumptions made.
