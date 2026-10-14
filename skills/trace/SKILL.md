---
name: trace
description: Keep the Productflow traceability matrix (TRACE.md) current and check ID consistency across the documents. Use after a stage document is approved, when IDs change, and before asking for approval.
---

# Traceability

IDs: requirements FR-n / NFR-n (PRD), stories US-n and use cases UC-n (stories document), components C-n (SAD), screens S-n (UI/UX study).

1. Run `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" trace`. It exits 1 on **errors**: a document references an ID its defining document does not contain. It prints **warnings** for coverage gaps: an FR not referenced in the stories, a US not referenced in later documents, a component not referenced in the TDD.
2. Fix errors in the document you are working on. Never "fix" an error by editing an approved earlier document; use the upstream-change process.
3. Update `TRACE.md` so each row links a requirement to its stories, components and screens, as far as the documents exist. Add rows for new IDs. Never renumber or reuse an ID once its document is approved.
4. Report in one line: "Trace check: 0 errors, 2 warnings (FR-4 and FR-7 not yet covered by stories)". A warning about a later stage that has not run yet is expected.
