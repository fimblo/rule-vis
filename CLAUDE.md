# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

Early prototype. The data model is defined (schema v0.2.0), three rulesets exist (tic-tac-toe, Werewolf, Munchkin simplified), and an interactive Cytoscape.js visualizer is live on GitHub Pages. Features so far: hover-to-highlight, click-to-mute nodes, edge tooltips with per-party utility, net utility per node (border color + hover tooltip), force-directed layout.

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

A rule is a 6-tuple:
```
(rule_name, rule_type, source_entity, target_entity, source_utility, target_utility)
```

**Entity:**
```
(name, type)   # e.g., ("Bob", "person") or ("Engineering Team", "group")
```

**Rule types:** positive (option to do X), negative (may not do Y), atomic procedural (must use recipe Z), complex (combinations)

**Utility:** relative weights (e.g., 1, -1) — not absolute units. One rule per source→target pair; a rule targeting 5 entities becomes 5 tuples.

`faction` is an optional entity field (string or null) grouping entities into teams — e.g. `"village"` vs `"werewolf"`. Not yet visualized but present in data.

`object_entity` and `object_utility` are optional fields that separate *the governed entity* (who must comply) from *the object of the action* (what the rule is about). These are the same entity for agent-to-agent rules, but differ when a passive resource is involved — a shared spreadsheet, a deployment pipeline, a game board. `object_utility` captures the cost or load imposed on that resource.

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
