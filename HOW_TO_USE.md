# HOW TO USE — HUBTIQUE OS MULTI-ACCOUNT SYSTEM
## Simple. Read once. Follow every time.

---

## WHAT YOU UPLOAD EVERY SESSION

### Always upload (4 small files):
- PROGRESS.md
- ERRORS.md
- TRIED_FIXES.md
- VERSIONS.md

### Only upload broken files (check PROGRESS.md first):
If PROGRESS.md says "manifest.json broken" → upload manifest.json only
If PROGRESS.md says "scheduler.js broken" → upload scheduler.js + rotation.js (linked)
Do NOT upload all 10 files every time. Waste of tokens.

### Upload system log:
Copy the console errors from Chrome DevTools or terminal and paste at the bottom of the prompt.

---

## EVERY NEW ACCOUNT — DO THIS IN ORDER

1. Open new Claude account
2. Paste HUBTIQUE_AGENT_PROMPT.md (the big prompt)
3. Upload: PROGRESS.md + ERRORS.md + TRIED_FIXES.md + only the broken files
4. Paste your latest error log at the end like this:

```
=== LATEST ERROR LOG ===
[paste console errors here]
========================
```

5. Send. Wait for analysis. Confirm before it codes.

---

## WHEN TOKENS ARE RUNNING LOW

You will see Claude's responses getting shorter. Before it stops:

Type this: "tokens low — give me all 4 file updates now"

Claude will output exact text to paste into each file.
You paste it. Open next account. Continue.

---

## AFTER EACH FIX — UPDATE YOUR 4 FILES

Claude gives you exact text to paste. Go to GitHub:
- Click the file → click pencil ✏️ → paste → Commit changes
Takes 30 seconds per file.

---

## YOUR SESSION FLOW

```
New account opens
      ↓
Paste prompt + upload 4 tracking files + upload broken files + paste error log
      ↓
Claude reads PROGRESS.md + ERRORS.md + TRIED_FIXES.md (mandatory)
Claude reads ONLY broken files (smart, saves tokens)
      ↓
Claude outputs analysis (2 min, no code yet)
      ↓
You say "yes proceed" or "no, try this instead"
      ↓
Claude gives patch (changed lines only, not full file)
      ↓
You apply patch → test → paste result back to Claude
      ↓
Claude confirms or adjusts
      ↓
Tokens getting low → type "tokens low"
      ↓
Claude outputs 4 file updates
      ↓
You paste updates to GitHub (30 sec)
      ↓
Open next account → repeat
```

---

## FILES IN YOUR GITHUB REPO

```
bug-fixes-project/
├── HUBTIQUE_AGENT_PROMPT.md   ← The big prompt (copy-paste every session)
├── HOW_TO_USE.md              ← This file
├── PROGRESS.md                ← Update every session
├── ERRORS.md                  ← Add bugs, mark fixed
├── TRIED_FIXES.md             ← Add every fix attempt
├── VERSIONS.md                ← Save every working state
└── [broken files only]        ← Upload only what is being fixed
```

