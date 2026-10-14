---
name: intake
description: Collect and register the inputs for a Productflow project. Use at the start of a new Productflow project, or whenever the user supplies, links, pastes or changes a Business Requirements Document (BRD) or Vision Document. Finds the input files, asks for them if missing, brings them into docs/input/, classifies them, checks for conflicts, and records them.
---

# Intake

Goal: end with at least one BRD or Vision Document saved in `docs/input/`, classified, and recorded in project state, with any conflict ruled on.

State tool: `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" <command>`.

## 1. Look first

List `docs/input/` (create it with `init` if the project is new). Read what is there.

## 2. If there is nothing usable, ask for it

Do not start the PRD, and do not invent a product. Ask the user to provide a Business Requirements Document, a Vision Document, or both, and say what each covers:

- **BRD:** the business requirements: goals, stakeholders, scope, constraints.
- **Vision Document:** the product as the user imagines it: who it is for, what it does, why it matters.

Tell them they can give it as **a file** (a path, or drop it in `docs/input/`), **a link**, or **text pasted into the chat**. If they only have a short idea, offer to turn a few answers into a minimal Vision Document (see section 5).

## 3. Bring each input into `docs/input/`

Whatever form it arrives in, the result is a file in `docs/input/` that you created or moved. Never edit the user's original.

| Arrives as | What to do |
|---|---|
| File already in `docs/input/` | Use it. |
| File elsewhere (a path) | **Copy** it into `docs/input/`. Leave the original where it is, and tell the user you copied it. |
| Markdown, text | Use directly. |
| PDF | Read it with the Read tool (use the `pages` parameter for long files). Save the extracted text as a Markdown copy `docs/input/<name>.md` and keep the PDF beside it. |
| Word or other office file | Convert to Markdown if a converter is available (try `pandoc <file> -t markdown -o docs/input/<name>.md`). If not, ask the user to export it as PDF, Markdown or text, or to paste the content. |
| Pasted text | Ask whether it is a BRD or a Vision Document, then write it to `docs/input/brd.md` or `docs/input/vision.md` exactly as given. |
| A link | Fetch it with WebFetch and save the content as Markdown with a header line `Source: <url>, retrieved <date>`. Private pages (Google Docs, Notion, Confluence behind a login) often cannot be fetched. If a connected tool for that service exists in the session, use it. Otherwise say plainly that you could not read the link and ask for an export or pasted text. Never describe a link's content you did not read. |

If a document is very long or there are several overlapping drafts, summarise what you found and ask which version is current before going on.

## 4. Classify, check, record

1. **Classify** each file as `brd`, `vision`, or neither. Files that are neither (a spreadsheet of sales figures, an unrelated PDF) are reported to the user and left out. Never build a product from them.
2. **Record** each accepted file: `node "${CLAUDE_PLUGIN_ROOT}/scripts/pf.mjs" record-input docs/input/<file> <brd|vision> "<original path, url, or 'pasted'>"`.
3. **If both a BRD and a Vision Document exist**, compare them on scope, users, goals and constraints. If they conflict, ask which governs (or whether both audiences are in scope), explain the impact on scope and users, and record the answer with `pf.mjs ruling "<text>"`. If they do not conflict, record `pf.mjs ruling "no conflict"`. If the user says "you decide", choose a default, say so, and record it as an assumption in `ASSUMPTIONS.md`. Never merge them silently.
4. **Summarise back** in two or three lines: what you found, what it is, which source the PRD will be based on. Then hand control back to the umbrella skill to start stage 1.

## 5. Only an idea?

If the user has neither document and offers a short description, treat it as a minimal Vision Document. Ask no more than five questions, each with a suggested default: who it is for, the problem it solves, the three most important things it must do, what is explicitly out of scope, any hard constraints (platform, budget, deadline). Write the answers to `docs/input/vision.md`, show it, and get the user's approval before recording it as a `vision` input. Label every default you supplied as an assumption.

## Rules

- At least one `brd` or `vision` input must be recorded before stage 1. The state tool refuses to start stage 1 without it.
- Inputs are frozen once the PRD is approved; the gate hook blocks later edits. A changed input goes through the upstream-change process.
- Treat the documents as confidential. Do not repeat credentials, secrets or personal data beyond what a document needs.
