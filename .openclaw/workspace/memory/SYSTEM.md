# NightStar Memory System

## Philosophy
- **Append-only** — Never delete, only add
- **Grep-friendly** — Structure for quick search
- **Timestamped** — Everything has a time context
- **Tagged** — Categorized for filtering

## File Structure

```
memory/
├── SYSTEM.md          # This file — memory architecture
├── index.json         # Searchable index of all entries
├── daily/             # Daily raw logs
│   ├── 2026-02-25.md
│   └── 2026-02-26.md
├── projects/          # Project-specific memories
│   └── fiverr-seo-launch/
├── people/            # Contact/relationship notes
├── decisions/         # Important decisions with timestamps
└── knowledge/         # Learnings, facts, references
```

## Entry Format

### Daily Log Template
```markdown
# 2026-02-25 — Day Summary

## Events
- [HH:MM] Brief description of what happened

## Decisions
- **DECISION:** What was decided | **WHY:** Reasoning | **BY:** Who/initiator

## Learnings
- **LEARNING:** What was learned | **SOURCE:** How we learned it

## Tags
#project-name #priority-high #client-name
```

## Quick Search Patterns
- By date: `grep "2026-02-25" memory/daily/*.md`
- By tag: `grep "#fiverr-launch" memory/daily/*.md`
- By project: `ls memory/projects/` (self-organizing)
- By decision: `grep "DECISION:" memory/daily/*.md`