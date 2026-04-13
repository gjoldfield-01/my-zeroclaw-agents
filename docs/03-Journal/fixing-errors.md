# Troubleshooting Journal

*What broke, why, and how it was fixed.*

---

## Phase 1 — Initial Setup

**Status:** ✅ Resolved

### Issues
- Node runtime error: `Unexpected token '?'`
- OpenRouter errors: `402 Payment Required`, `401 Unauthorized`
- Config warnings: invalid keys (`model_settings`)

### Root Cause
- Outdated Node version (v12) — incompatible with modern JS
- Invalid/expired OpenRouter API key
- Token limits too high for available credits

### Fix
- Installed NVM → upgraded to Node v24
- Replaced OpenRouter API key
- Adjusted token usage and cleaned config

### Outcome
✅ ZeroClaw runs
✅ LLM working reliably

---

## Phase 2 — Telegram Integration

**Status:** ✅ Resolved

### Issues
- 404 API error
- No messages received
- Confusion between gateway vs channel

### Root Cause
- Incorrect bot token (copied wrong value)
- Telegram not properly added as a channel

### Fix
- Regenerated token via BotFather
- Used correct commands:

```bash
zeroclaw channel add telegram
zeroclaw channel bind-telegram <code>
```

### Outcome
✅ Telegram bot connected and receiving messages

---

## Phase 3 — MCP Setup (LinkedIn Tools)

**Status:** ⚠️ Partial

### Issues
1. Duplicate `[mcp]` config blocks
2. `command not found` / empty command
3. MCP connected but tools unusable

### Root Cause
- TOML misconfiguration (duplicate sections)
- MCP binary not in PATH (NVM environment issue)
- MCP not authenticated — no LinkedIn token

### Fix
- Cleaned config to single `[mcp]` block
- Fixed PATH / Node environment

### Outcome
✅ MCP server connected
✅ Tools visible (6 tools)
❌ Not usable yet (auth missing)

---

## Phase 4 — Agent Build (Local Success Only)

**Status:** ⚠️ Partial

### Issue
- Module not found error

### Root Cause
- File saved as `.py` instead of `.js`

### Fix
- Renamed file correctly

### Outcome
✅ Agent works locally
❌ Not integrated into ZeroClaw

---

## Phase 5 — Agent Architecture / Delegation Failure

**Status:** ❌ Unresolved

### Issue
- Unknown config key: `routes` / `tools`

### Root Cause
- Using ZeroClaw v0.1.7 — new agent architecture not supported

### Outcome
❌ Delegation not working
❌ Agent not callable by system

---

## Phase 6 — Environment & Auth Separation

**Status:** ⚠️ Partial

### Issues
- "LinkedIn tools not initializing"
- Env vars not visible in daemon runtime

### Root Cause
- Daemon running in different environment than shell
- Env vars not injected into systemd process

### Fix
- Added `.env` file
- Injected into systemd service

### Outcome
✅ LLM + env confirmed working
⚠️ MCP still not auto-loading

---

## Phase 7 — LinkedIn OAuth (Main Blocker)

**Status:** ❌ Unresolved

### Issues
1. Redirect URI mismatch
2. OAuth flow never completed
3. No access token generated

### Root Cause
- Mismatch between LinkedIn app settings and OAuth request URL
- OAuth callback ≠ ZeroClaw server

### Fix (identified, not yet completed)
- Correct redirect URI: `http://localhost:3000/auth/linkedin/callback`
- Complete OAuth flow: get code → exchange for access_token

### Outcome
❌ No LinkedIn access token
❌ MCP cannot authenticate
❌ Posting not possible

---

## Phase 8 — Token & Performance Issue

**Status:** ⚠️ Identified, not fixed

### Issue
- Very high token usage (~22k tokens per session)

### Root Cause
- Entire system context sent on each call: memory, MCP schemas, conversation history

### Fix (not yet implemented)
- Separate agents / reduce context per agent
- Avoid exposing all tools to main agent

---

## Phase 9 — Switching LinkedIn MCP

**Status:** (To be completed — notes to add)

*Planned update: document the switch to a different LinkedIn MCP server, the reasoning behind it, and the outcome.*

---

## Root Cause Summary — The Big 4

| # | Issue | Impact |
|---|-------|--------|
| 1 | **Version Mismatch** — ZeroClaw v0.1.7 doesn't support new agent architecture | Broke delegation completely |
| 2 | **Environment Separation** — shell ≠ daemon | MCP + API keys not consistently available |
| 3 | **Configuration Errors** — duplicate TOML blocks, wrong tokens | Repeated setup failures |
| 4 | **LinkedIn OAuth** — redirect mismatch, flow never completed | No access token = no LinkedIn functionality |

---

## Final System State

**Working:**

- ZeroClaw daemon
- OpenRouter (LLM)
- Telegram bot
- Node environment
- MCP (visible, 6 tools)
- Agent (local only)

**Not Working:**

- LinkedIn authentication
- MCP runtime integration
- Agent delegation

---

> **Clean one-line diagnosis:** Everything works except LinkedIn authentication — and that's the single point of failure stopping the full system.
