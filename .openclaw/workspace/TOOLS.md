---
summary: "Workspace template for TOOLS.md"
read_when:
  - Bootstrapping a workspace manually
---

# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## Discord Server
- Server ID: 219209969717739520
- Channels:
  - #general: 219209969717739520
  - #inbox: 1475526565210226688
  - #tasks: 1475526620063469772
  - #ventures: 1475526672307720234
  - #research: 1475526708819005561
  - #finance: 1475526760278917171
  - #ideas: 1475526786640121866
  - #logs: 1475526832127348766

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

---

## 🧠 Model Strategy (NightStar)

### Local Models (Ollama) — Zero Cost

Running on the VPS at `localhost:11434`:

- **qwen2.5-coder:14b** — Default for heartbeats, simple tasks, file operations
  - 14.8B parameters, 9GB size
  - Good at coding, tool calling
  - **Cost: $0** — runs locally

### Cloud Models — High Capability

Available via `/model` command:

| Model | Alias | Best For |
|-------|-------|----------|
| `ollama/qwen2.5-coder:14b` | Local (Ollama) | Heartbeats, simple tasks, file ops |
| `nexos/9720977c-...` | Nexos Claude Opus 4.6 | Complex reasoning, coding, architecture |
| `nexos/cc532f6b-...` | Nexos Claude Sonnet 4.5 | Balanced speed/capability |
| `nexos/bbc37f2b-...` | Nexos Gemini 3 Flash | Image analysis, fast responses |
| `openrouter/anthropic/claude-sonnet-4-5-20251022` | Claude Sonnet 4.5 (OpenRouter) | Balanced speed/capability |
| `openrouter/google/gemini-3-flash-preview` | Gemini 3 Flash (OpenRouter) | Image analysis, fast responses |
| `openrouter/google/gemini-2.5-flash-image` | Gemini 2.5 Flash Image (OpenRouter) | Image generation, visual design |
| `openrouter/moonshotai/kimi-k2.5` | Kimi 2.5 | Fallback, general tasks |

### Quick Model Switching

```bash
# Check current model
/model status

# Switch to local (default)
/model ollama/qwen2.5-coder:14b

# Switch to powerful cloud model
/model nexos/9720977c-77de-4292-b373-0feef2915faf

# List all available
/model list
```

### Token Efficiency Strategy

| Task Type | Model | Reason |
|-----------|-------|--------|
| Heartbeats | Ollama local | $0 cost, 48× daily |
| File operations | Ollama local | Fast, no API latency |
| Simple Q&A | Ollama local | Good enough |
| Complex coding | Nexos Claude Opus | Best tool use |
| Architecture decisions | Nexos Claude Opus | Deep reasoning |
| Image analysis | Nexos Gemini Flash | Vision capabilities |
| Urgent/high-stakes | Nexos Claude Opus | Reliability |

### Ollama Management

```bash
# Check status
ollama list
curl http://localhost:11434/api/tags

# Pull new models
ollama pull llama3.3
ollama pull qwen2.5-coder:32b

# Remove models
ollama rm llama3.3
```

### Fallback Chain

If local model fails, OpenClaw falls back to:
1. `ollama/qwen2.5-coder:14b` (primary, local)
2. `openrouter/moonshotai/kimi-k2.5` (cloud fallback)
3. `nexos/9720977c-...` (Claude Opus, ultimate fallback)
