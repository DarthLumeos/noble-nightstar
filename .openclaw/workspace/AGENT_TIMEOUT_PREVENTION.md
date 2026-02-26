# Agent Timeout Prevention — NightStar Operations Guide

## Problem Statement
Subagent sessions (Verde, Aurea, etc.) have hard timeout limits:
- **30-60 minutes** total runtime per session
- **Inactivity timer** kills idle sessions
- **Tool hang** — long-running operations (image gen, web search) can exceed limits

This caused:
- Aurea: Thumbnail renders stalled, session expired before files generated
- Verde: Gig copy generation took too long, session killed mid-task

---

## Prevention Strategies

### 1. Task Chunking (Primary Fix)
**Old Pattern:** "Write complete gig copy (all sections)"  
**New Pattern:** 
- Task 1: "Write gig title only"
- Task 2: "Write gig description only"
- Task 3: "Write package descriptions"

**Max chunk size:** 10-15 minutes of work

### 2. Progress Heartbeats
**Rule:** Agents must send status update every **10 minutes**  

Example:
```
[Minute 0] Starting task...
[Minute 10] Progress: 50% — titles done, working on descriptions
[Minute 20] Finalizing — QA in progress
```

This keeps session alive and provides visibility.

### 3. Tool Timeout Handling
**Long operations:** Image generation, large web searches, file processing

**Pattern:**
```
Attempt operation with 5-minute timeout
If timeout:
  - Retry once
  - If still failing: Save progress, report partial, request chunked approach
```

### 4. Session Self-Awareness
Agents should track their own runtime:
```
Runtime check: If >25 minutes, save state and request continuation
```

---

## Implementation Checklist

### For Workflow Design (Me/Noble)
- [ ] Break complex deliverables into <15-min chunks
- [ ] Specify heartbeat checkpoints in task prompts
- [ ] Set retry logic for long-running tools

### For Agent Prompts
- [ ] Include "Send progress update every 10 minutes" in instructions
- [ ] Add runtime self-check logic
- [ ] Specify save-state behavior on timeout risk

### For Monitoring
- [ ] Track subagent session durations
- [ ] Flag tasks approaching 25-minute mark
- [ ] Auto-chunking suggestions for repeat offenders

---

## Quick Reference: Task Chunking Rules

| Task Type | Max Duration | Chunking Strategy |
|-----------|--------------|-------------------|
| Writing (copy) | 10 min | Section-by-section (title → desc → packages) |
| Research | 15 min | Source-by-source or query-by-query |
| Image generation | 10 min | One image at a time, not batches |
| QA review | 10 min | Document-by-document |
| Data processing | 15 min | Chunk by record count or file size |

---

## Recovery Protocol

If timeout occurs:
1. **Assess:** What was completed before timeout?
2. **Recover:** Retrieve any saved outputs from session history
3. **Respawn:** Create new session with narrower scope
4. **Continue:** Pick up from last checkpoint