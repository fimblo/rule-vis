# rule-vis

An experiment in visualizing rule sets as graphs.

Organizations, games, and other systems operate under sets of rules — policies, guidelines, processes, game mechanics. Over time these accumulate and interact in ways that are hard to reason about from plain text alone. This tool translates human-readable rules into a structured format and renders them as an interactive graph, with the goal of making patterns, contradictions, and concentrations of constraint visible.

**Current state:** early prototype. Three rulesets are included (tic-tac-toe, Werewolf, Munchkin simplified). The graph shows entities as nodes and rules as directed edges. Edge color reflects target utility (green = beneficial, red = constraining, gray = neutral). Node border color shows net utility across all rules. Hover an entity to see its utility breakdown; click to mute it and hide its edges.

## Try it

Live on GitHub Pages: https://fimblo.github.io/rule-vis

Or locally:

```bash
git clone git@github.com:fimblo/rule-vis.git
cd rule-vis
python3 -m http.server 8000
# open http://localhost:8000
```

## How this is built

This project is developed collaboratively between a human and Claude Code (an AI assistant). The design decisions, tradeoffs, and reasoning behind the work are logged in [diary.md](diary.md) — one entry per session. If you want to understand how we work or are thinking about joining in, that's the place to start.

The development approach in short: small semantic commits, TDD where applicable, and a strong preference for validating ideas early over building in the abstract.

## Working with Claude Code on this project

If you clone this repo and open it with Claude Code, it will read `CLAUDE.md` automatically — that file contains the data model, conventions, and project context needed to contribute without re-explaining everything.

A few things to know:
- **Read the diary first.** `diary.md` captures the reasoning behind decisions that aren't obvious from the code.
- **Claude writes diary entries automatically.** It is instructed to append an entry at the end of every session. The Stop hook in `.claude/settings.local.json` reinforces this.
- **Semantic commits, small and focused.** Claude handles commits; see `CLAUDE.md` for the convention.
- **No build step.** Run `python3 -m http.server 8000` and open `localhost:8000`.
