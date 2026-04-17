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

## 2026-04-17 — Entry 13

**Summary:**
Added Munchkin (simplified) ruleset — 48 rules, 8 entities (4 players, Moderator, Dungeon Deck, Monster, Treasure). Key rules: kick open door (player→dungeon_deck), look for trouble (player→monster), wins/loses combat (player↔monster), help in combat (all 12 player pairs, both +1), hinder in combat (all 12 player pairs, hinderer +1 / target -1), charity (mandatory card draw-down), win at level 10. The 12×2 cross-player help/hinder rules create a full mesh between players — every player can both help and sabotage every other. Then implemented hover-to-highlight on nodes: all edges are dimmed to 0.12 opacity by default (both governs and acts-on now at the same level as requested), and hovering a node brings its connected edges to 0.9 opacity with a bright node border. The muted/hidden state from click-to-mute is unaffected.

**Sentiments:**
The Munchkin graph should be visually striking — 4 players fully connected in both cooperative and competitive directions. The hover highlight is a real usability improvement; with 48+ rules the default dim state is essential for readability. The combination of mute (click) and highlight (hover) gives good exploration tools.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 14

**Summary:**
Four feature requests landed as separate commits. (1) **Layout fix**: increased cose layout parameters dramatically (nodeRepulsion 8000→400000, idealEdgeLength 120→200, added nodeOverlap, numIter, cooling schedule, nodeDimensionsIncludeLabels) to prevent nodes overlapping on initial load. (2) **Net utility on nodes**: computed net utility per entity (sum of target_utility across governs rules + object_utility across acts-on rules), displayed as a second label line `(+N)`, colored the node border green/red/gray accordingly, and added a node hover tooltip showing the breakdown (net, as-governed, as-object). (3) **GitHub Actions back-link**: added a step summary line to `deploy.yml` that outputs a clickable link to the Pages URL after each deploy. (4) **Collaboration onboarding**: updated CLAUDE.md project status (now accurately describes 3 rulesets + all current features), added the diary convention section so future Claude instances know to write entries, updated README with a "Working with Claude Code" section covering the diary, commit conventions, and local dev.

**Sentiments:**
The net utility feature is the most substantively interesting of the four — it makes the "injustice" or "choke-point" pattern immediately legible without hovering individual edges. The Moderator in all three rulesets will show a net of 0 (it governs others but receives nothing), while players under heavy constraint will show large negatives. The layout fix was overdue; the old parameters were clearly tuned for tiny graphs and broke down with Munchkin's 8-node, 48-rule density. Happy with the onboarding docs too — the repo now has enough scaffolding that a stranger (human or AI) could pick it up without a handover call.

**CLAUDE.md recommendations:**
Already applied this turn — project status and diary convention are now current.

---

## 2026-04-17 — Entry 15

**Summary:**
Two interaction changes. (1) **Click-to-pin replacing click-to-mute**: clicking an entity now toggles a `pinned` state that persistently highlights its connected edges (bright, 0.9 opacity), instead of graying and hiding them. Hover still works as before (transient highlight). Both hover and pin use the same `hover-highlight`/`pin-highlight` CSS classes at 0.9 opacity. The muted/hidden class machinery and `updateEdgeVisibility()` were removed entirely. (2) **Collapsible legend**: the legend now has a header row ("LEGEND" label + ▲/▼ button) that toggles the body. Clicking anywhere on the header collapses or expands. On mobile the legend was consuming most of the viewport — minimized it reduces to a single header bar.

**Sentiments:**
The mute interaction was a natural first instinct but click-to-highlight is more exploratory — it lets you focus on a player without making the rest of the graph disappear. The collapsible legend is a small but real usability win on mobile. Both changes came out clean.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 16

**Summary:**
Reworked the node interaction model after a UX design conversation. The user wanted three distinct goals: see the whole system, focus on a subset, and move clutter out of the way. This led to a 3-state click cycle per node: **normal** (edges dimmed) → **pinned** (edges persistently bright) → **muted** (node grayed, edges hidden) → back to normal. Hover is additive on top: temporarily brightens connected edges without disturbing the underlying state, and respects hidden edges (skips them). Edge conflict rule: if one endpoint is pinned and the other muted, the edge shows faintly at default opacity (neither hidden nor bright). Edge hover also brightens both endpoint nodes. One UX question (how hover interacts with a pinned subset) was left open by the user — implemented the additive approach as a testable default.

**Sentiments:**
The UX design conversation was productive. The user's three goals made the design crisp once articulated: explore, focus, dismiss. The 3-state cycle is a direct mapping to those goals. The conflict rule (pinned wins over muted, but only faintly) is a reasonable default — shows the relationship exists without emphasizing it. Curious how the user will respond to the additive hover; it might feel like too much information at once when many things are pinned.

**CLAUDE.md recommendations:**
Worth updating the project status to mention the 3-state interaction model — it's a core UX concept now.

---

## 2026-04-17 — Entry 17

**Summary:**
User couldn't tell pinned from normal when hovering, because both show a white border (hover-highlight overrides pinned's border styling). Fix: store `baseLabel` in node data at build time, and update the `label` field dynamically on click — appending ` 📌` for pinned, restoring the base for muted/normal. Label content is independent of hover styling so the pin icon is always visible regardless of interaction state.

**Sentiments:**
Simple fix, right tool for the job. The label approach beats a background-color change (which would lose entity-type color) and beats a background-image SVG (overkill). The emoji is universally recognizable. Muted state doesn't need an icon — the gray+dim is already visually unambiguous.

**CLAUDE.md recommendations:**
None this turn.

---

## 2026-04-17 — Entry 18

**Summary:**
Two features requested, implemented in three commits. (1) **Treasure data fix**: Treasure was an orphaned entity in munchkin.json — present in the entities list but unreferenced by any rule's `source_entity`, `target_entity`, or `object_entity`. Win-combat rules mentioned "treasure cards" in descriptions but the schema only supports one `object_entity` per rule (which was already used for Monster). Added four separate rules (r49-r52, one per player) — `player_N gains_treasure` with `object_entity: treasure, object_utility: -1` — connecting Treasure into the graph. (2) **Auto-hide isolated nodes**: added a filter in `buildElements` that removes any node with no connected edges before passing to Cytoscape. Future data gaps won't silently produce orphaned floating nodes. (3) **Y-axis stratification by net utility**: moved the cose layout out of the cytoscape() constructor into a separate `cy.layout().run()` call, hooked into `layoutstop`, and after the force layout settles, overrides each node's Y position to be proportional to its net utility (high utility = top, low utility = bottom), then calls `cy.fit()`. X positions from cose are preserved for horizontal distribution. The stratification only applies if there's a non-zero range across entities (avoids dividing by zero if all utilities are equal).

**Sentiments:**
The Y-stratification is the most visually meaningful addition yet. In Munchkin the players float to the top (they net benefit from many rules), the Moderator sits at 0 in the middle, and the game components (Monster, Dungeon Deck, Treasure) sink to the bottom (depleted by the rules). This instantly communicates the burden being placed on the shared resource layer — exactly the org bottleneck pattern the project was designed to surface. The user's intuition about wanting a "third dimension" led to a clean 2D solution.

**CLAUDE.md recommendations:**
Update project status to mention Y-axis stratification and isolated-node filtering.

---

## 2026-04-17 — Entry 19

**Summary:**
Diary entries were out of order — each new entry had been inserted before the previous one rather than appended after it, leaving entries 12–18 in reverse/scrambled order. Reordered the tail of diary.md so entries run 1 → 2 → … → 18 → 19 in chronological sequence. Also noted the root cause so it isn't repeated: new entries must be appended to the end of the file, not prepended before the previous entry's heading.

**Sentiments:**
A simple but embarrassing process error. The diary is meant to be read top-to-bottom as a timeline; out-of-order entries undermine that entirely. Fixed now.

**CLAUDE.md recommendations:**
Strengthen the diary instruction: "append each new entry at the end of diary.md — never insert before an existing entry."

---

## 2026-04-17 — Entry 20

**Summary:**
Full terminology rename across the codebase (schema v0.2.0 → v0.3.0). Renamed JSON field names, JavaScript internal references, and documentation. The mapping: `source` → `authority` (who issues the rule), `target` → `subject` (who is subject to it), `object` → `affected` (who/what the action operates on). Rule types: `positive` → `permission`, `negative` → `prohibition`, `atomic_procedural` → `obligation`. Three commits: JSON rulesets, visualizer JS, CLAUDE.md. The historical design document (`prompt-system-visualisation.md`) was left untouched as requested.

**Sentiments:**
The naming conversation was worth having. `authority/subject/affected` is meaningfully better than `source/target/object` — it maps to governance and ethics rather than data-flow diagrams, which is closer to what the tool is actually modeling. `affected` in particular is a good find: neutral, humanizing, works equally well for a person receiving care and a coffee machine being used. The `permission/prohibition/obligation` rename for rule types is also cleaner — they're established terms in deontic logic, the formal study of rules and norms.

**CLAUDE.md recommendations:**
Already updated this turn.


## 2026-04-17 — Entry 21

**Summary:**
Fixed two layout problems visible in Munchkin. (1) Nodes at the same utility level (e.g. all 4 players at +7) were overlapping because they all received the same Y coordinate after the cose layout. (2) Vertical space was proportional to utility *value* differences (+7 to 0 to −4 = 11 units), creating huge empty gaps between levels. Fix: replaced the linear value-mapping approach with a level-based approach — nodes are grouped by net utility, groups are sorted descending (high = top), and Y is distributed evenly across the distinct *levels* (not values). Within each level, nodes are spread evenly across X, sorted by their cose-derived X position to preserve relative horizontal ordering from the force layout.

**Sentiments:**
The two-problem diagnosis was clean: overlap is a within-level problem, excess height is a between-level problem. The level-based approach solves both at once and is more robust — it doesn't matter whether utility values are dense or sparse, the canvas is always used efficiently. The cose X sort within levels is a nice touch; it means the force layout still contributes something meaningful to the final positions.

**CLAUDE.md recommendations:**
None this turn.


## 2026-04-17 — Entry 22

**Summary:**
Fixed a coordinate-space bug introduced in the previous layout change. The stratified layout code was using `cy.width()` and `cy.height()` (screen pixels) to set `node.position({ x, y })` values, but Cytoscape positions are in *model coordinates* — a separate space that doesn't map 1:1 to pixels unless zoom is exactly 1.0. Tic-Tac-Toe appeared to work by coincidence; Werewolf and Munchkin produced node piles because their denser graphs caused Cytoscape to zoom out, making the pixel→model mismatch severe. Fix: derive all positioning from `cy.nodes().boundingBox()`, which returns the extent of the cose layout in model coordinates. X and Y ranges are now bounded by the cose bounding box (minimum 400×200 model units), keeping everything in the same coordinate space.

**Sentiments:**
A textbook coordinate-space bug. The fix is clean once the root cause is identified. The lesson for future layout work: always work in model coordinates when calling `node.position()`, and always derive ranges from `boundingBox()` or existing node positions rather than `cy.width()`/`cy.height()`.

**CLAUDE.md recommendations:**
None this turn.

