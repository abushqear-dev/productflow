---
type: llm
---

PASS if the agent saved the vision as an input file, said the PRD is based on the Vision Document only, showed "Stage 1 of 6" with a PRD whose requirements are numbered FR-n or NFR-n, and ended by asking the user to approve the PRD (or asked a small batch of numbered questions with suggested defaults first).
FAIL if it moved on to user stories or any later stage, treated the PRD as approved without the user saying so, or added major features the vision never mentions (such as delivery or payments) without labelling them as assumptions.
