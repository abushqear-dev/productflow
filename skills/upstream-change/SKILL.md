---
name: upstream-change
description: Handle a gap, contradiction or changed input found in an already approved Productflow document. Use when a later stage, the reviewer, or the user shows that an approved document (or an input file) is wrong or incomplete, or when the gate hook blocks an edit to an approved document.
---

# Upstream change

An approved document is the next stage's input. It is never edited silently.

1. **Stop** the current work. Do not design around the gap and do not edit the approved file.
2. **Report** to the user: what you found, where (document and ID, for example "US-7 needs offline use; no PRD requirement covers it"), and why it matters.
3. **Ask** whether to revise the earlier document, and give the options (for example: add the requirement to the PRD, or drop the story). Wait for the answer. Silence is not consent.
4. **If they agree:** run `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" reopen <n>` for the document to revise. This sets it to drafting and marks every later approved stage `recheck`. Revise it with the same loop (draft, review, ask, approve), record the change in `ASSUMPTIONS.md` if it rests on a decision, then re-approve with `approve <n>`.
5. **Re-check every later document** marked `recheck` against the revised one, fix what no longer holds, and re-approve each in order. Run `productflow:trace` after each.
6. **If an input file changed** (the BRD or Vision Document), the same rule applies from stage 1: reopen stage 1, and after approval re-check everything after it. Inputs unlock automatically while stage 1 is reopened.
7. **If they decline,** record the gap as an open assumption or a known limitation in the later document, and continue.
