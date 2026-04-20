# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Project Is

A domain-agnostic tool for translating rule sets into structured data and visualizing them as an interactive graph (nodes = entities, edges = rules). Two planned components: a **Rule Translator** (LLM-based) and a **Visualizer** (Cytoscape.js, live on GitHub Pages).

**Naming convention:** Keep names generic (`entity`, `rule`, `utility`) — not domain-specific (`employee`, `policy`). The tool should work equally for a Monopoly rulebook and a company handbook.

## Schema (v0.6.0)

**Rule fields:** `rule_name`, `rule_type` (permission / prohibition / obligation), `authority_entity`, `subject_entity`, `authority_utility`, `subject_utility`. Optional: `affected_entity` + `affected_utility` for the passive target of the action. Utilities are relative weights, not absolute units.

**Cohorts:** Top-level `cohorts` maps names to entity ID arrays. Reference with `@cohort_name` in any entity field. Only one cohort per rule expands (first of subject / affected / authority wins). Rules with two cohort fields will have the second left unresolved — acts-on edges to unresolved cohorts are silently dropped.

**Phases:** Top-level `phases` array for ordered game phases. Rules carry an optional `"phase"` field. Visualizer shows a phase slider.

**Calendar:** Top-level `"calendar": { "unit": "month" }` for org rulesets. Rules carry `"schedule": { "months": [...] }` (1-indexed) or `"months": "all"` (active every month, calendar-anchored; distinct from no `schedule` = structurally always-on). Visualizer shows a Jan–Dec slider. Mutually exclusive with `phases`.

**Entity fields:** `id`, `name`, `type` (open string — `authority`, `team`, `person`, `group`, `process`, etc.), optional `faction`.

## Local Development

```bash
python3 -m http.server 8000  # open http://localhost:8000
```

Rulesets live in `rulesets/`. No build step.

## Development Diary

A diary is kept in `diary.md`. **Write an entry at the end of every conversation turn** (date, turn number, summary, sentiments, CLAUDE.md recommendations). The Stop hook in `.claude/settings.local.json` shows a reminder.

**Always append to the end. Never insert before an existing entry.**

## Development Conventions

**Commits:** Semantic, subject line ≤80 chars, small and story-telling. Prefixes: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`. Failing tests are OK to commit — note it in the message.

## Design Decisions

- Utilities are relative weights (not time/money) — avoids over-engineering
- Visualization is read-only first; utility editing is a future feature
