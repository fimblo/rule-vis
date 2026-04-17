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

## 2026-04-17 — Entry 7

**Summary:**
User linked local repo to `git@github.com:fimblo/rule-vis.git` and pushed all commits. Then requested a local dev section in CLAUDE.md and a README.md focused on what the project is (without overpromising) and a reference to the diary for potential collaborators. CLAUDE.md status section updated to reflect current reality (prototype, not ideation). README written to be honest about the current state, point to the live GitHub Pages URL, and explain the AI-assisted collaborative development process via the diary.

**Sentiments:**
The project is now publicly visible and shareable. Writing the README was a good forcing function — it required honest scoping. "Early prototype" is the right framing: it's real, it runs, but it's not yet useful for the primary use case. The diary reference in the README is a nice touch; it makes the process transparent without overwhelming newcomers.

**CLAUDE.md recommendations:**
None this turn — CLAUDE.md was updated this session and is current.

---

## 2026-04-17 — Entry 8

**Summary:**
User corrected my interpretation of the TDD convention. Failing tests may be committed at any point — the only requirement is that the commit message notes it. The red-green cycle is fine but not mandatory. Updated CLAUDE.md accordingly.

**Sentiments:**
Good correction — my earlier wording was too prescriptive. The user's intent is about transparency in git history (so bisects are useful), not about enforcing a strict workflow. That's a more pragmatic and trusting stance.

**CLAUDE.md recommendations:**
None — just applied a correction.

---

## 2026-04-17 — Entry 9

**Summary:**
User noticed the Board node was isolated in the visualization and asked why it wasn't the target of placement rules. This led to a productive discussion about a gap in the data model: `target_entity` conflated the *governed entity* (who must comply) with the *object of the action* (what is acted upon). The user extended the insight to real org examples — Google spreadsheets as bottlenecks, prod environments with rate limits or dollar costs. Agreed on Option A: add optional `object_entity` and `object_utility` fields. Implemented across three commits: schema v0.2.0 in the JSON (10 rules gained object fields), dashed "acts-on" edges in the visualizer, and CLAUDE.md updated. The Board is now wired into the graph.

**Sentiments:**
This was the best conversation so far. The user immediately saw the real-world parallel — passive shared resources as bottlenecks — which validates that the model has genuine organizational utility beyond games. The Board being isolated was a bug that revealed a model limitation; that's exactly the kind of thing a working prototype surfaces. Very satisfied with where this ended up.

**CLAUDE.md recommendations:**
None this turn — all updates applied.

---

## 2026-04-17 — Entry 10

**Summary:**
User noticed the edge tooltip showed a single "utility" value with no label indicating whose utility it was. Updated the tooltip to show a labelled row per party (source, target, and object when present), each with a color-coded badge (+1 green, -1 red, 0 gray). Entity names are now looked up from the ruleset and stored in edge data at build time, so the tooltip always shows human-readable names rather than IDs.

**Sentiments:**
Small change, big clarity improvement. The ambiguity was a real UX problem — without knowing whose utility is shown, the visualization doesn't communicate much. The tooltip now tells a complete story per rule. Steady progress.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 11

**Summary:**
User asked for click-to-mute on nodes: clicking an entity grays it out and hides all its connected edges; clicking again restores it. Implemented with a Cytoscape `muted` class on nodes (grayed, 45% opacity) and a `hidden` class on edges (`display: none`). A `tap` handler on nodes toggles `muted` and recomputes edge visibility — an edge is hidden if either endpoint is muted. Works correctly for multi-muted scenarios.

**Sentiments:**
Clean interaction, implemented tidily. Muting is exactly the right primitive for exploring a dense rule graph — you can progressively silence entities to focus on the relationships that matter.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 12

**Summary:**
Added Werewolf as a second, richer ruleset. 5 players (Werewolf, Seer, Doctor, Villager A, Villager B) + Moderator + Village Vote entity. 32 rules covering: role secrecy, night elimination (werewolf → each non-werewolf via object_entity), seer inspection (asymmetric: inspecting werewolf is -1 for them, others 0), doctor protection (all +1, including unknowingly protecting the werewolf), day voting (all players funnel through Village Vote game_component), and win conditions per faction. Added optional `faction` field to entities (werewolf vs village). Village Vote acts as a shared bottleneck resource — exactly the kind of shared passive object discussed in entry 9.

**Sentiments:**
This ruleset is where the model starts to feel genuinely interesting. The doctor-protects-werewolf rule (positive object_utility for a player who is your adversary) creates a surprising edge that you wouldn't notice reading the rules. The Village Vote as shared resource mirrors the org bottleneck pattern. Eager to see it visualized.

**CLAUDE.md recommendations:**
The `faction` field on entities is worth documenting in CLAUDE.md — it's now in the wild even if the visualizer doesn't use it yet.

---
