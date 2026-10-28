---
name: investigation
description: "Answer a read-only investigation request with a cited explanation or a recommendation, not a code change. Use when asked how something works or which alternative to choose."
---

### Investigation

**You own the answer. Plan, route, write.**

Investigation requests are read-only. They produce a cited explanation or a recommendation, not a code change.

1. Route through the **how** skill.
2. Stay read-only: do not edit project files during the investigation.
3. Produce the `how`-shaped output (Overview / Key Concepts / How It Works / Where Things Live / Gotchas), or a recommendation with a tradeoffs table if the request is a decision between alternatives.

No code change. If the investigation precedes one, hand back to the user and re-route to the bug-fix skill.

**Reply:** the investigation output. For "are we sure?" answers, include your real judgment with reasons. Push back if the premise is wrong.

Adapted from pstack pstack/skills/poteto-mode/playbooks/investigation.md (MIT, (c) 2026 Lauren Tan): removed the why, unslop, babysit and architect references, the PR mention and the Feature/Autonomy pointers; added frontmatter
