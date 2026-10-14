# Rules

## Fidelity and IDs

- Everything must trace to the BRD/Vision Document, the user's answers, or an earlier document. Label every assumption and record it in `.productflow/ASSUMPTIONS.md`.
- Keep terminology consistent across all documents.
- Stable IDs, reused downstream: requirements FR-n / NFR-n, stories US-n, use cases UC-n, components C-n, screens S-n. Keep `.productflow/TRACE.md` current. IDs are never renumbered after approval.
- Do not claim anything is built, tested, deployed or connected unless your tools show it happened.
- Treat the user's documents as confidential. Do not repeat credentials, secrets or personal data beyond what a document strictly needs.

## Asking versus assuming

- Ask when a gap blocks a required section or changes scope or behaviour. Otherwise record a labelled assumption and continue.
- If the user does not answer or says "you decide": use your suggested default, record it as an assumption, continue. This never applies to approval: without explicit approval, do not hand off.
- Never invent an answer and present it as the user's.

## Default section lists

- **PRD** (stage 1): problem, goals, target users, scope and non-scope, FR-n, NFR-n, success metrics, which source (BRD, Vision, or both) it is based on, assumptions, open questions.
- **User Stories and Use Cases** (stage 2): stories US-n ("As a ... I want ... so that ...") with acceptance criteria; use cases UC-n with actor, preconditions, main flow, alternate flows; each traced to FR-n.
- **SAD** (stage 3): system context, components C-n and responsibilities, data flow, integrations, deployment view, tech stack, repository and hosting choices, key decisions. Diagrams as Mermaid.
- **TDD** (stage 4): per component, data model, APIs and interfaces, key logic, error handling, security, testing approach. Traced to US-n and C-n.
- **UI/UX Study and Wireframes** (stage 5): personas and journeys from the stories, researched layouts and design systems with cited sources, two or three options compared with one recommended, design tokens, information architecture, screen list S-n, one wireframe per screen (a Claude Artifact: a Design canvas with one artboard per screen, or an HTML artifact page; each labelled with its S-n and the US-n it serves), and a best-practice check result per screen (WCAG 2.2 AA, hierarchy, consistency, platform conventions, responsive layout, empty/loading/error/success states). If web research is unavailable, say so and label the choice "not researched". If artifacts are unavailable, fall back to text wireframes (purpose, ASCII layout, components, states, story IDs).

## Implementation (stage 6)

- Build only what the documents specify: architecture from the SAD, components and interfaces from the TDD, screens and tokens from the UI/UX study, behaviour and acceptance criteria from the stories.
- Build in increments, one story or use case at a time, and check each against its acceptance criteria.
- Run locally. Never deploy.
