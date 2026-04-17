# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

This project is in the **ideation/pre-implementation phase**. The only artifact is `prompt-system-visualisation.md`, a design document describing the vision. No build system, dependencies, or source code exist yet.

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

## Development Conventions

**TDD (red-green):** Write a failing test first, commit it (title must note it fails, e.g. `test: add failing test for X`), then make it pass in a follow-up commit.

**Commit style:** Semantic commits, subject line under 80 characters. Commits should be small and tell a story — no large all-in-one commits.

Common prefixes: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`

## Design Decisions Already Made

- Utility stays as relative weights, not time/money units — avoids over-engineering
- Entities can be people, groups, or even other rules (referenced by name)
- Procedural rules are considered costlier for target entities than positive/negative rules
- Visualization: read-only first (pan/zoom/rotate), utility summation toggle (opposing rules cancel), utility editing is a future feature
- Formal grammar (Backus-Naur) is on the horizon but not required for ideation phase
