# ERRORS — Hubtique OS
## How to use: Add a new bug at the top. Mark status as you fix it.

---

## BUG #: [short title]
**File:** [which file, which line]
**Error message:** [paste exact error from console]
**What I was doing:** [what action caused it]
**Root cause:** [why it happens]
**Fix to apply:** [what to change]
**Affects other files:** [yes/no — which ones]
**Status:** [ ] Applied  [ ] Tested  [ ] Fixed ✓

---
━━━ PASTE INTO ERRORS.md ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# ERRORS — Hubtique OS

---

## BUG #1 — Skip-nav element resolved instead of search input
**File:** src/task-executor.js — resolveLabel() ~line 701
**Error message:** Agent clicks "@e2 [button] main content →#skippedLink" / "Search, alt, forward slash" repeatedly — loops 30 steps without typing
**What I was doing:** Navigate to Amazon → agent tries to find search input
**Root cause:** resolveLabel() matched innerText of skip-navigation landmark elements ("Search, alt, forward slash") which have those words in their text, resolving to the wrong element instead of the actual <input>
**Fix applied:** Restrict innerText matching to INPUT/TEXTAREA/short BUTTONs only; skip role=navigation and aria-hidden elements
**Affects other files:** No
**Status:** ✅ Fixed — src/task-executor.js updated

---

## BUG #2 — Stale element registry causes click/type loops
**File:** src/task-executor.js — clickStep() line 771, typeStep() line 790
**Error message:** Failed: "@eN [button] ..." not found for click — Recovery: refreshing element registry (but loop continues)
**What I was doing:** Any click or type after a page change or failed click
**Root cause:** `if (!ctx._page_registry)` guard meant registry was only fetched once per task run. After any page change or failed click recovery the registry was stale but reused, so every subsequent click resolved to wrong/missing elements
**Fix applied:** Removed the guard — always call getSnapshotStep() before every click and type
**Affects other files:** No
**Status:** ✅ Fixed — src/task-executor.js updated

---

## BUG #3 — Amazon geo-location popup blocks element detection (0 elements)
**File:** content.js — __omni_snapshot__()
**Error message:** Got page (N chars, 0 elements) — agent loops clicking "Search" which resolves to nothing
**What I was doing:** Agent navigates to amazon.com from Rotterdam IP — Amazon shows "Deliver to Netherlands" geo popup
**Root cause:** Modal overlay covering page caused isVisible() to return false for all underlying elements. Registry built with 0 interactive elements. Agent had nothing to click.
**Fix applied:** Added autoDismissPopups() function that runs before every snapshot — clicks "Dismiss"-type buttons only when a modal/overlay is detected. Made __omni_snapshot__ async with 350ms settle wait after dismiss.
**Affects other files:** background.js (getContent + getSnapshot handlers must await async snapshot)
**Status:** ✅ Fixed — content.js + background.js updated

---

## BUG #4 — background.js getContent/getSnapshot not awaiting async snapshot
**File:** background.js — getContent handler line 883, getSnapshot handler line 953
**Error message:** Silent — result would be a Promise object instead of registry data, causing empty index
**What I was doing:** content.js __omni_snapshot__ made async (needed for popup dismiss settle)
**Root cause:** execInTab callbacks were not async and did not await __omni_snapshot__()
**Fix applied:** Changed both execInTab callbacks to async () => and added await before window.__omni_snapshot__()
**Affects other files:** No
**Status:** ✅ Fixed — background.js updated

---

## BUG #5 — JSON parse crash at position 932 (LLM response truncated)
**File:** src/agent-planner.js — parseJSON() line 406
**Error message:** AI error: Expected ',' or '}' after property value in JSON at position 932 (line 1 column 933)
**What I was doing:** llm_generate step returning result at step 28-29
**Root cause:** LLM response was truncated mid-JSON — likely max_tokens limit too low (800) for complex multi-step tasks. parseJSON() has no truncation recovery.
**Fix applied:** Not yet fixed — next priority
**Affects other files:** src/agent-planner.js generatePlan() maxTokens param
**Status:** ❌ Still broken — next session

---

## BUG KEY-SYNC — API keys never reach extension (root cause of all AI failures)
**File:** src/pages/ApiVault.tsx saves to Supabase. Extension reads from chrome.storage.local. Never synced.
**Error message:** All LLM providers exhausted / 401 / provider skipped silently
**Root cause:** ApiVault.tsx → Supabase postgres. Extension rotation.js → chrome.storage.local. Zero connection between them.
**Fix applied:** Not yet fixed
**Affects other files:** src/rotation.js, background.js, all AI calls
**Status:** ❌ Still broken — highest priority after BUG #5

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
━━━ ADD TO ERRORS.md ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## BUG #6 — Skip-nav in snapshot causes LLM to click wrong elements
**File:** content.js — add() function ~line 158
**Error:** Agent clicks "Search, alt, forward slash" for 30 steps
**Root cause:** add() had no filter — skip-nav anchor links and keyboard
hint elements were included in the snapshot the LLM reads. LLM copied
the @eN ref directly, bypassing resolveLabel entirely.
**Fix:** Added isSkipNav() guard at top of add() — filters aria-hidden,
#anchor hrefs, and labels matching keyboard shortcut patterns.
**Status:** ✅ Fixed — content.js updated

## BUG #7 — Pollinations model name changed, maxTokens too low
**File:** src/agent-planner.js lines 128, 494
**Error:** All AI providers failed / JSON parse crash at position 932
**Root cause:** Pollinations renamed 'openai-large' → 'openai'. maxTokens=800
truncated LLM JSON mid-response causing parse crash.
**Fix:** model → 'openai', maxTokens 800 → 2000
**Status:** ✅ Fixed — agent-planner.js updated

CONFIRMED BUG: Three bugs, priority order

BUG #1 — KEY-SYNC (root cause of "No API keys configured")
File: ApiVault.tsx  Line: wherever key is saved to Supabase
Root cause: saveApiKey() writes to Supabase only; extension scoreProvider
reads chrome.storage.local; the two never meet
Fix: After Supabase save, also send key to extension via
chrome.runtime.sendMessage({action:"saveApiKey", ...})
NEED FILE: ApiVault.tsx — please upload it

BUG #2 — llm_generate infinite loop (26 calls, no break)
File: CometBrowser.tsx  Step 4 same-action guard
Root cause: guard checks hasParam (label/url/value); llm_generate has none
→ actionKey = "llm_generate:step5/6/7..." always unique → guard never fires
Fix: separate consecutiveLlmGen counter, break+force navigate after 3

BUG #3 — JSON truncation crash (steps 4, 29)
File: CometBrowser.tsx  Step 3 parse block
Root cause: LLM response cut off mid-token; sanitizeRawJSON/repairAndParseJSON
both require well-formed strings — neither closes an open quote
Fix: add fixTruncatedJSON() between sanitize and repair paths

LINKED FILES TO TEST AFTER: rotation.js (Bug 1 only), agent-planner.js (none)


## BUG #8 — Stale tab inherited between tasks
**File:** background.js line 1693 — run_agent handler
**Error:** New task starts on previous task's page (Amazon) instead of navigating fresh
**Root cause:** activeAgentTabId never cleared between runs — agent reuses whatever tab was open
**Fix:** await chrome.storage.local.remove('activeAgentTabId') after _loopGuard.reset()
**Status:** ✅ Fixed
## BUG #9 — Reddit posts not rendered when read_body fires
**File:** background.js — waitForPageSettle() line 582
**Error:** read_body returns only nav skeleton, posts are empty
**Root cause:** waitForPageSettle fires on status=complete (HTML load)
but Reddit is React-rendered — posts appear 3-4s later
**Fix:** JS_HEAVY_HOSTS list — Reddit/Twitter get +3500ms extra settle
**Status:** ✅ Fixed

## BUG #10 — llm_generate loops 28 times with no page content
**File:** src/task-executor.js — llmGenerateStep() line 819
**Error:** LLM hallucinates data, says "please provide page body"
**Root cause:** llm_generate prompt never included ctx.page_body —
LLM had no actual content to extract from
**Fix:** Auto-inject ctx.page_body into prompt if present
**Status:** ✅ Fixed
━━━ ADD TO ERRORS.md ━━━━━━━━━━━━━━━━━━━━━━━━━

## BUG #13 — context.page_body never set — root cause of all llm_generate failures
**File:** background.js — executePlanWithFullStack() line 519
**Error:** llm_generate always says "I don't see page content" across ALL tasks
**Root cause:** executePlanWithFullStack called executeStep(step, context) but
never merged the returned saved values back into context. page_body from
read_body was returned but immediately discarded. ctx.page_body was always
undefined when llm_generate ran.
**Fix:** After each step, merge raw.saved + raw.body/text/navigated into context
**Status:** ✅ Fixed — universal, fixes all read+extract tasks
## BUG #1 — KEY-SYNC: Extension has no API keys
**File:** ApiVault.tsx  **Line:** saveApiKey handler
**Error:** Extension: No API keys configured — agent cannot call LLM
**Root cause:** Vault saves keys to Supabase; extension reads chrome.storage.local; never synced
**Status:** ❌ still broken — awaiting ApiVault.tsx upload
**Affects:** rotation.js (all providers score 0), CometBrowser.tsx callAI

## BUG #2 — llm_generate infinite loop (26 consecutive calls)
**File:** CometBrowser.tsx  **Line:** Step 4 same-action guard
**Error:** Agent calls llm_generate 26 times without navigating
**Root cause:** hasParam=false for llm_generate → unique step-index key each time → guard never fires
**Status:** ❌ patch written, not yet tested
**Affects:** Agent never completes multi-step tasks, burns all 30 steps

## BUG #3 — JSON truncation crash
**File:** CometBrowser.tsx  **Line:** Step 3 parse block
**Error:** JSON parse + repair both failed: Unterminated string at position 628
**Root cause:** LLM response truncated mid-token; neither sanitize nor repair closes open quotes
**Status:** ❌ patch written, not yet tested
**Affects:** consecutiveAIErrors++ on every truncated response → agent aborts after 3
