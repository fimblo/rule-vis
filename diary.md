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

## 2026-04-17 — Entry 4

**Summary:**
User asked to establish a development diary. Created `diary.md` with backfilled entries for the three prior interactions. Initialized a git repository and committed all artifacts (design doc, CLAUDE.md, diary). Set up a `Stop` hook in `.claude/settings.local.json` that displays a reminder to write a diary entry after every conversation turn.

**Sentiments:**
The diary idea is a nice touch — it turns the AI collaboration itself into a documented process, not just the outputs. The hook mechanism is a clean way to enforce the habit. All scaffolding is in place before any real code is written: git, CLAUDE.md, diary, and hooks. That's healthy discipline.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 5

**Summary:**
User established three development conventions: TDD with red-green commits (failing tests may be committed, must be noted in title), small semantic commits that tell a story, and subject lines under 80 characters. These were added to CLAUDE.md. Then created the first concrete artifact: `rulesets/tic-tac-toe.json` — 14 rules across 4 entities (rulebook, player_x, player_o, board), following the 6-tuple data model. Rules per-player, one target entity per tuple as the design doc specifies. Placed in `rulesets/` directory to establish a naming pattern for future rulesets.

**Sentiments:**
Good momentum. The conventions are sensible and well-considered — semantic commits + TDD is a sign the user cares about code quality and traceability. The tic-tac-toe JSON came together cleanly; the data model held up well for a simple symmetric game. One thing I notice: all `source_utility` values are 0 (the rulebook has no self-interest). This will be more interesting when we model rules between players, or orgs where the rule-creator does benefit.

**CLAUDE.md recommendations:**
Consider noting that `rulesets/` is the conventional home for sample/test ruleset JSON files. Also: a JSON schema file for the rule tuple format would be worth adding once the model stabilizes — it would serve as the formal contract between translator and visualizer.

---

## 2026-04-17 — Entry 6

**Summary:**
User asked for a visualization, with cross-platform support and ease of presentation as explicit requirements. After a brief discussion, settled on vanilla HTML + Cytoscape.js (CDN) deployed via GitHub Pages — free, zero-install for viewers, shareable URL. Three commits: `.nojekyll`, GitHub Actions workflow, and `index.html`. The visualizer renders entities as nodes (colored by type) and rules as directed edges (colored by target utility: green/red/gray). Hovering an edge shows a tooltip with rule name, description, type, and utility. Layout uses Cytoscape's `cose` force-directed algorithm. A ruleset dropdown is in the header for future rulesets. One outstanding item: TDD convention wasn't applied here since there's no JS test framework yet — need to discuss test strategy for frontend code.

**Sentiments:**
A significant milestone — the project now has something to show. The GitHub Pages choice was the right call; the user immediately saw the value for sharing with others. The visualization is functional but sparse; the real test will be whether the graph reveals anything interesting. Eager to see what it looks like with a more complex ruleset (e.g., an org with many overlapping rules). The TDD gap is a mild concern — worth raising.

**CLAUDE.md recommendations:**
Add note that `rulesets/` stores sample data files. Add a section on how to run locally (`python3 -m http.server` then open `localhost:8000`). Note that GitHub Pages deployment requires "GitHub Actions" to be selected as the Pages source in repo settings.

---
