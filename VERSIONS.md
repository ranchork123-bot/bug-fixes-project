# VERSIONS — Hubtique OS
## MOST IMPORTANT FILE — Save every time something works, even partially!

---

## ⭐ BEST VERSION TO START FROM
**Label:** [e.g. "scheduler working, manifest broken"]
**Commit:** [paste hash from GitHub]
**How to go back:** Click the commit on GitHub → copy files from there

---

## VERSION HISTORY

### Version [N] — [short label]
**Commit hash:** 
**Date:** 
**Status:** [ ] ✅ Fully working  [ ] ⚠️ Partially working  [ ] ❌ Broken

**What works:**
- 

**What doesn't work:**
- 

**Files changed in this version:**
- 

**Why we moved away from this version:**

---

### Version [N] — [short label]
**Commit hash:** 
**Date:** 
**Status:** [ ] ✅ Fully working  [ ] ⚠️ Partially working  [ ] ❌ Broken

**What works:**
- 

**What doesn't work:**
- 

**Files changed:**
- 

**Why we moved away:**

---

## ❌ BROKEN VERSIONS — DO NOT GO BACK TO THESE

### Broken Version — Commit: [hash]
**Why broken:** 
**Already tried:** 
**Do not retry**

━━━ PASTE INTO VERSIONS.md ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# VERSIONS — Hubtique OS

---

## ⭐ BEST VERSION TO START FROM
**Label:** "v14.1 — skip-nav + stale registry fixed, popup fix applied"
**Commit:** [paste hash after you commit today's files]
**How to go back:** Click the commit on GitHub → copy files from there

---

## VERSION HISTORY

### Version 2 — skip-nav + stale registry + popup fix (current)
**Commit hash:** [paste after commit]
**Date:** 2026-05-13
**Status:** ⚠️ Partially working — popup fix applied, needs test

**What works:**
- Extension loads (v14.1)
- Agent navigates correctly
- resolveLabel finds correct elements (skip-nav fix)
- Registry always fresh before click/type
- Pollinations fallback AI works
- autoDismissPopups runs before every snapshot

**What doesn't work:**
- Amazon geo popup fix not yet confirmed by test
- JSON parse crash at step 28-29 (LLM truncated, maxTokens too low)
- KEY-SYNC: real API keys (Groq, OpenRouter etc) never reach extension

**Files changed in this version:**
- src/task-executor.js (BUG #1 + #2)
- content.js (BUG #3)
- background.js (BUG #4)

---

### Version 1 — baseline v14.1 (before this session)
**Commit hash:** [unknown — not committed to tracking repo]
**Date:** before 2026-05-13
**Status:** ❌ Broken

**What worked:**
- Extension loaded
- Basic navigation

**What didn't work:**
- Agent looped on Amazon search (skip-nav bug)
- 0 elements after popup (geo popup bug)
- Stale registry caused click failures
- JSON parse crash at step 28

**Why we moved away:** All above bugs

---

## ❌ BROKEN VERSIONS — DO NOT GO BACK TO THESE
- Any version using root/rotation.js — dead code, always use src/rotation.js
- Any version with scheduler polling < 15s — Chrome throttles it

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
