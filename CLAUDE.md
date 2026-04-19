# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

Early prototype. Schema v0.6.0. Rulesets: tic-tac-toe, Werewolf, Werewolf (Diverse Utility), Munchkin (Simplified), Teal Engineering Org. Visualizer on GitHub Pages. Features: hover-to-highlight, click-to-mute nodes, edge tooltips, net utility per node (border color + hover), force-directed layout, cohort expansion, game phase slider, calendar month slider, two-level game/variant selector, dynamic legend, markdown ruleset descriptions.

## What This Project Is

A general-purpose tool for translating human-readable rule sets into a structured format and visualizing them as a graph (nodes = entities, edges = rules). The primary motivation is organizational rules (policies, guidelines, processes), but the tool should be domain-agnostic.

**Other target use cases:**
- Board game rules (the initial test case — rules are complete, well-scoped, and easy to find)
- Organizational meeting structures
- Bottleneck/dependency mapping

**Code naming convention:** Keep names generic (e.g., `entity`, `rule`, `utility`) rather than org-specific (e.g., `employee`, `policy`). The tool should work equally well for a Monopoly rulebook and a company handbook.

## Two Planned Components

1. **Rule Translator** — converts natural language rules into structured tuples using an LLM
2. **Visualizer** — renders translated rules as an interactive 2D graph

## Core Data Model

Schema version: **0.6.0**

A rule is a tuple:
```
(rule_name, rule_type, authority_entity, subject_entity, authority_utility, subject_utility)
```

**Entity:**
```
(name, type)   # e.g., ("Bob", "person") or ("Engineering Team", "group")
```

**Field roles:**
- `authority_entity` — who issues or enforces the rule (e.g. Moderator, Rulebook, HR policy)
- `subject_entity` — who is subject to the rule; their behaviour is constrained, permitted, or required
- `affected_entity` *(optional)* — who or what is acted upon. Separates the subject (who must comply) from the passive entity the action operates on — a shared spreadsheet, a game board, another person receiving care. `affected_utility` captures the impact on them.
- `authority_utility` / `subject_utility` / `affected_utility` — relative weights (e.g. 1, −1, 0), not absolute units.

**Rule types:**
- `permission` — subject may do X
- `prohibition` — subject may not do Y
- `obligation` — subject must follow procedure Z
- `complex` — combinations (not yet used)

**Cohorts (v0.4.0):** An optional top-level `cohorts` object maps names to arrays of entity IDs. Any entity field in a rule may reference a cohort with `@cohort_name`. The visualizer expands each cohort reference into one rule per member at load time, generating IDs like `r01_werewolf`. Only one cohort reference per rule is supported (first of subject, affected, authority wins). Use cohorts to avoid repeating identical rules across multiple entities.

**Phases (v0.5.0):** An optional top-level `phases` array defines an ordered list of named phases (e.g. `["night", "day"]` or `["Q1", "Q2", "Q3", "Q4"]`). Individual rules may carry an optional `"phase"` string field matching one of those names. Rules with no `phase` field are always active. When a ruleset defines phases, the visualizer shows a slider below the header; selecting a phase dims all edges whose rule belongs to a different phase. Rules without a phase are never dimmed.

**Calendar (v0.6.0):** For rulesets anchored to a real-world calendar (e.g. org policies), use `"calendar": { "unit": "month" }` at the top level instead of `phases`. Individual rules carry an optional `"schedule": { "months": [...] }` field with a 1-indexed array of months (e.g. `[3, 6, 9, 12]` for quarter-end months). The shorthand `"months": "all"` marks a rule as active every month but still calendar-anchored (distinct from no `schedule` field = structurally always-on). When a ruleset defines `calendar`, the visualizer shows a month slider (Jan–Dec) instead of a phase slider. `phases` and `calendar` are mutually exclusive.

`faction` is an optional entity field (string or null) grouping entities into teams — e.g. `"village"` vs `"werewolf"`. Not yet visualized but present in data.

## Local Development

No build step. Run a local server to avoid browser CORS restrictions when fetching JSON:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

**GitHub Pages:** The workflow in `.github/workflows/deploy.yml` deploys on every push to `main`. To activate, go to **Settings → Pages → Source** and select **GitHub Actions**.

Sample rulesets live in `rulesets/`.

## Development Diary

A development diary is kept in `diary.md`. **Write a diary entry at the end of every conversation turn.** Each entry should include: date, turn number, summary of what happened, sentiments/observations, and any CLAUDE.md recommendations. The Stop hook in `.claude/settings.local.json` shows a systemMessage reminder after each turn.

**Always append new entries to the end of `diary.md`. Never insert before an existing entry.**

## Development Conventions

**TDD:** Failing tests may be committed — just note it in the commit message (e.g. `test: add failing test for X`). The red-green cycle is encouraged but not required.

**Commit style:** Semantic commits, subject line under 80 characters. Commits should be small and tell a story — no large all-in-one commits.

Common prefixes: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`

## Design Decisions Already Made

- Utility stays as relative weights, not time/money units — avoids over-engineering
- Entities can be people, groups, or even other rules (referenced by name)
- Procedural rules are considered costlier for target entities than positive/negative rules
- Visualization: read-only first (pan/zoom/rotate), utility summation toggle (opposing rules cancel), utility editing is a future feature
- Formal grammar (Backus-Naur) is on the horizon but not required for ideation phase
