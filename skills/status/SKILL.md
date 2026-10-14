---
name: status
description: Show where a Productflow project stands. Use when the user asks for status, progress, or what stage they are on in a Productflow project.
---

Run `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" status` and report the result in one or two lines: the current stage, its status, and what the user needs to do next. If there is no `.productflow/` folder, say this is not a Productflow project and offer to start one with `/productflow:product`.
