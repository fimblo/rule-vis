# rule-vis Development Diary

---

## 2026-04-17 — Entry 1 (backfill: project inception)

**Summary:**
The project was introduced via `prompt-system-visualisation.md`. The vision: translate natural language rules into structured tuples, then visualize them as a graph to help organizations identify patterns, contradictions, and inefficiencies in their rule sets. The data model was outlined: a rule is a 6-tuple `(rule_name, rule_type, source_entity, target_entity, source_utility, target_utility)`. Rule types defined: positive, negative, atomic procedural, complex. Utility kept as relative weights (not absolute units). The document ended with a question about work slicing and whether the waterfall approach was risky.

**Sentiments:**
The problem is genuinely interesting — the "tragedy of the commons" framing for organizational rules is a sharp observation. The data model is clean and minimal. Excited by the scope: it's ambitious but tractable. The ideation phase feels healthy — the author is thinking out loud and open to challenge.

**CLAUDE.md recommendations:**
None yet — CLAUDE.md did not exist at this point.

---

## 2026-04-17 — Entry 2 (backfill: CLAUDE.md and use-case scope)

**Summary:**
Created `CLAUDE.md` to document the project for future Claude Code sessions. The user clarified that the tool should be **domain-agnostic**: the primary use case is organizational rules, but board games are the intended first test case (rules are complete and easy to find), and other use cases include meeting structures and bottleneck mapping. CLAUDE.md was updated to reflect this, with explicit guidance to keep code naming generic (e.g., `entity`, `rule`, not `employee`, `policy`).

**Sentiments:**
The board game pivot as a starting point is pragmatic and smart. It de-risks the data model before tackling messier real-world rule sets. The domain-agnostic framing is a good design constraint to lock in early. Collaboration feels efficient — the user thinks in product terms naturally.

**CLAUDE.md recommendations:**
Already applied: added board game as initial test case, listed other use cases, added naming convention guidance.

---

## 2026-04-17 — Entry 3 (backfill: work slicing Q&A)

**Summary:**
Answered the user's two-role question from the design document. As a **product person**: recommended leading with a hand-crafted JSON ruleset to validate the data model, then building the visualizer against static data before building the translator — because the riskiest assumption is whether visualization actually surfaces insights, not whether translation is possible. As a **software engineer**: recommended against committing to BNF grammar now; use a JSON schema as a lightweight grammar, prompt the LLM to emit conforming JSON directly, and let the board game data reveal when a formal grammar becomes necessary (likely when complex/nested rules appear). Key reframe: translator and visualizer are decoupled if the schema is the stable interface between them — that's interface-first, not waterfall.

**Sentiments:**
The user responded warmly to the viz-spike-first recommendation. Good sign — they're willing to reorder their mental model based on risk reasoning. The project is moving from ideation to first concrete artifact. Energy is good.

**CLAUDE.md recommendations:**
Consider adding a note that the JSON schema for the rule tuple is the primary interface contract between the translator and visualizer components — worth documenting once it stabilizes.

---
