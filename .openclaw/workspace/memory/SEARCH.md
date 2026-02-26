# Memory Search Guide

## Quick Commands

### Find by Date
```bash
# Today's entries
find memory/daily -name "$(date +%Y-%m-%d).md" -exec cat {} \;

# Specific date
cat memory/daily/2026-02-25.md
```

### Find by Tag
```bash
# All entries tagged #fiverr-launch
grep -r "#fiverr-launch" memory/daily/

# Multiple tags
grep -r "#priority-high.*#fiverr-launch" memory/daily/
```

### Find Decisions
```bash
grep -r "DECISION:" memory/ | head -20
```

### Find Projects
```bash
ls -la memory/projects/
```

## Semantic Search (Future)
Once Supermemory is installed:
```bash
openclaw skill supermemory search "what was the Fiverr pricing strategy"
```