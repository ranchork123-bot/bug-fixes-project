# TRIED FIXES — Hubtique OS
## CHECK THIS FILE BEFORE TRYING ANY FIX. If it's here with ❌ — DO NOT TRY AGAIN.

---

## 🚫 FIX GRAVEYARD — Never try these again
These wasted the most commits. Do not repeat under any circumstances.

- [Fix name] — tried [N] times — reason it never works: 
- [Fix name] — tried [N] times — reason it never works: 

---

## Fix #: [short title]
**Problem it was trying to solve:** 
**File changed:** [filename, line number]
**What we changed:**
[paste the code change here]

**Result:** [ ] ❌ Failed  [ ] ⚠️ Partial  [ ] ✅ Worked
**Why it failed / what went wrong:** 
**What to do instead:** 

---

## Fix #: [short title]
**Problem it was trying to solve:** 
**File changed:** 
**What we changed:**
[paste the code change]

**Result:** [ ] ❌ Failed  [ ] ⚠️ Partial  [ ] ✅ Worked
**Why it failed:** 
**What to do instead:** 

---
## Fix #1 — fixTruncatedJSON + 3-layer JSON parse
**Bug:** BUG #3 — JSON truncation crash
**File:** CometBrowser.tsx  **Line:** Step 3 parse block + new function after repairAndParseJSON
**What was changed:** Added fixTruncatedJSON() function; updated parse block to try sanitize→truncFix→repair
**Result:** [ ] Not yet tested
**Why:** Closes open strings from cut-off LLM responses before structural repair

## Fix #2 — consecutiveLlmGen counter
**Bug:** BUG #2 — llm_generate infinite loop
**File:** CometBrowser.tsx  **Line:** loop variables + before step 5
**What was changed:** Added consecutiveLlmGen counter; after 3 consecutive → force navigate to unvisited task URL or read_body
**Result:** [ ] Not yet tested
**Why:** Same-action guard misses parameterless actions — separate counter needed
