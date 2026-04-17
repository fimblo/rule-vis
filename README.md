# rule-vis

An experiment in visualizing rule sets as graphs.

Organizations, games, and other systems operate under sets of rules — policies, guidelines, processes, game mechanics. Over time these accumulate and interact in ways that are hard to reason about from plain text alone. This tool translates human-readable rules into a structured format and renders them as an interactive graph, with the goal of making patterns, contradictions, and concentrations of constraint visible.

**Current state:** early prototype. A tic-tac-toe ruleset is included as sample data. The graph shows entities as nodes and rules as directed edges, colored by how the rule affects the target (green = beneficial, red = constraining, gray = neutral).

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
