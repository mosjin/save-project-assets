---
name: save-project-assets-en
description: Use proactively when user confirms something works ("it works", "verified", "test passed"), before session ends / context compaction, or when user explicitly asks ("save assets", "save project", "update docs", "update memory"). Do NOT trigger on every git commit — only when there is new knowledge worth persisting.
---

# Save Project Assets

<!-- ─────────────────────────────────────────────
  CONFIG — fill in once for your project
  ───────────────────────────────────────────── -->
<!--
  GITHUB_REPO  = owner/repo  (e.g. myorg/myrepo)
  DOCS_DIR     = relative path to your docs folder (e.g. docs/)
  BRANCH       = default branch  (e.g. main)
-->

## Overview

Capture and persist all knowledge generated in this session: engineering lessons, feature status, fixed issues, ideas, and memory updates.

**Announce at start:** "Saving project assets..."

### Trigger Conditions

**✅ Run proactively (without being asked):**
| Signal | Example |
|--------|---------|
| User confirms something works | "it works", "verified", "test passed", "confirmed" |
| Session ending / context nearing full | Before `/compact`, before long pause |
| Feature branch merged to main | After `git merge` or PR merged |

**✅ Run on explicit request:**
| Phrase | Scope |
|--------|-------|
| "save assets" / "save project" | Full 7 steps |
| "update docs" | Steps 2-3-7 only |
| "update memory" | Step 6 only |
| `/save-project-assets-en` | Direct invocation |

**❌ Do NOT trigger on:**
- Every `git commit` / `git push` — too disruptive; Step 7 itself commits docs
- Generic "save" / "record" without clear intent — too ambiguous
- Test failures or work-in-progress — nothing confirmed yet
- Small refactor / formatting commits — no new knowledge

---

**Session start:** Silently load MEMORY.md to restore context. Do not announce unless asked.

---

## Step 0: Content Check (fast gate)

Before doing anything, answer:

- Did this session produce **new** lessons, fixes, or architectural decisions?
- Did the user **explicitly confirm** something works?
- Are there **new** ideas or status changes not yet in docs/memory?

**If all NO → stop immediately.** Say: "Nothing new to save this session."

Continue only if at least one answer is YES.

---

## Step 1: Harvest Session Knowledge

Before writing anything, collect from this session:

| Category | Source | Target |
|----------|--------|--------|
| Engineering lessons | Bugs fixed, root causes, architectural decisions | `docs/TECH_LOG.md` |
| Feature status | Built / tested / merged | `docs/CHANGELOG.md` |
| Fixed issues | Confirmed working by user | GitHub issue comments |
| Ideas / future work | Noticed but not implemented | `docs/IDEAS.md` |
| Persistent facts | Architecture, preferences, constraints | Memory files |

**Only write new entries** — never duplicate existing content.

---

## Step 2: Update TECH_LOG.md (Lessons & Experiences)

File: `docs/TECH_LOG.md`

Add at the **top** (newest first):

```markdown
## {YYYY-MM-DD}: {Brief title} (Issue #{N})

### Problem
{What was wrong or the challenge}

### Root Cause
{Why it happened — name the specific code/API/behavior}

### Fix / Approach
{What was done; include key code snippets if instructive}

### Lessons
- **Rule**: {actionable rule for the future}
- **Why**: {reason this matters}
```

**Skip** if no new engineering lessons this session.

---

## Step 3: Update CHANGELOG.md (Feature Status)

File: `docs/CHANGELOG.md`

- New feature/fix completed → add or update `[version-dev]` section at top
- Format: `### 🐛 Fix` · `### 🚀 Feature` · `### 📐 Refactor`
- Include issue numbers, key behavior changes, test counts
- **Never** repeat existing entries

---

## Step 4: Comment on Fixed GitHub Issues

For each issue the user confirmed working this session:

```bash
gh issue comment {N} --repo {YOUR_GITHUB_REPO} --body "## ✅ Fixed

{2-3 sentences: root cause + fix approach + how it was verified}

Status: **fixed**"
```

**Only comment if the user explicitly confirmed it works.** No speculation.

---

## Step 5: Update IDEAS.md (Future Work)

File: `docs/IDEAS.md`

New ideas, improvements, or "noticed but not done" items:

```markdown
## {Feature area}

- [ ] {Specific idea} — {why it's worth doing}
```

**Skip** if nothing new.

---

## Step 6: Update Persistent Memory

Memory location: auto-detected by Claude Code at `~/.claude/projects/{sanitized-cwd}/memory/`

### 6a. Update MEMORY.md index
- Add pointer for any new memory file
- Update changed facts (new test count, new branch, new version, new lesson)

### 6b. Write/update memory files as needed

**When to create a new memory file:**
- New architectural pattern discovered
- Engineering lesson with future applicability
- Project status change (new branch, feature shipped, new issue pattern)
- User preference or workflow correction

**Memory file format:**
```markdown
---
name: {topic}
description: {one-line description for relevance matching}
type: project | feedback | user | reference
---

{Content}
**Why:** {motivation}
**How to apply:** {when this matters}
```

### 6c. Key facts to keep current
- Active branch and its purpose
- Latest test count
- Latest release version / artifact name
- Fixed issues list (append newly fixed)
- New engineering lessons

---

## Step 7: Commit Docs

If `docs/` files were modified:

```bash
git add docs/TECH_LOG.md docs/CHANGELOG.md docs/IDEAS.md
git commit -m "docs: {brief description of what was recorded}"
git push origin {current-branch}
```

---

## Session Start Protocol

On the first message of a new session:

1. Claude Code auto-loads MEMORY.md — silently absorb the facts
2. Note: active branch, open issues, recent lessons, last test count
3. Be ready to answer project-state questions without re-reading source files
4. If user references recent work, confirm context: "I have context from the previous session: {key facts}."

---

## Asset Locations (customize for your project)

| Asset | Default Path |
|-------|-------------|
| Tech lessons | `docs/TECH_LOG.md` |
| Changelog | `docs/CHANGELOG.md` |
| Ideas | `docs/IDEAS.md` |
| Architecture | `docs/ARCHITECTURE.md` |
| Memory (auto) | `~/.claude/projects/{sanitized-cwd}/memory/MEMORY.md` |
| GitHub repo | _(set YOUR_GITHUB_REPO in config above)_ |

---

## Checklist

- [ ] TECH_LOG updated (or confirmed nothing new)
- [ ] CHANGELOG updated (or confirmed nothing new)
- [ ] Fixed GitHub issues commented
- [ ] IDEAS.md updated (or confirmed nothing new)
- [ ] Memory files updated with current facts
- [ ] Docs committed and pushed
