---
name: review
description: Independent review of a Productflow document draft before it is shown to the user. Use after drafting any stage document, and again after revising it. Runs the reviewer subagent and the trace check.
---

# Review a draft

Run this after every draft of a stage document, before asking the user to approve.

1. **Run the trace check:** `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" trace`. Dangling references are errors to fix now. Coverage warnings for IDs defined in this stage can be fine while later stages do not exist yet.
2. **Dispatch the reviewer** with the Agent tool, `subagent_type: "productflow:reviewer"`. In the prompt give it only the facts, not your conclusion:
   - the path of the draft,
   - the path of the Stage Skill Instruction (or paste it),
   - the paths of the earlier documents and the recorded inputs in `docs/input/`,
   - for stages after the first, `ASSUMPTIONS.md` and `TRACE.md`.

   Do not tell it the draft is good, and do not summarise the draft for it.
3. **Act on the report.** Fix every real issue. If you disagree with a finding, check it against the sources before dismissing it, and record the reason. Findings about missing information go to the user as questions, not to your imagination.
4. **Re-review** after substantial changes. Stop when the reviewer returns `CLEAN` or every remaining finding is a user question or a recorded assumption.

Never show the user a draft that has not been through this skill, and never skip it because the draft looks fine.
