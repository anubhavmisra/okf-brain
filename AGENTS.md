# Continuity Agent Skill — Navigation Guide

Guide for AI agents extracting records from chats (with optional grounding sources), and answering questions from existing bundles.
Read [SKILL.md](SKILL.md) first for the 6-step orchestration workflow and query mode.

## Structure

```
continuity/
  SKILL.md          # Main skill manifest — read this first
  AGENTS.md         # This navigation guide
  references/       # Detailed reference files — load on demand
```

## Quick Start

```
New to continuity? Read in this order:
  1. SKILL.md                         ← Step 0 update check, 6-step workflow, query mode
  2. references/stages.md             ← extract → review → write/lessons
  3. references/objects.md            ← default object types and field schemas
  4. references/okf-output.md         ← OKF bundle layout and write rules
  5. references/query.md              ← answering questions from an existing bundle
```

---

## Reference Index

| File | What it covers |
|------|----------------|
| [`references/stages.md`](references/stages.md) | Three pipeline stages: extract draft records, self-review, write bundle + conditional lessons |
| [`references/objects.md`](references/objects.md) | Default object catalog (Task, Thread, Attendance, Appointment, Lead, Expense, Decision, Blocker, Question), field schemas, custom type guidance |
| [`references/okf-output.md`](references/okf-output.md) | Bundle layout, frontmatter rules, summary-first prior-record loading, run retention, slug collisions |
| [`references/query.md`](references/query.md) | How to answer questions from an existing `okf/` bundle without re-extracting |

---

## Pipeline Overview

```
chat (+ connectors / grounding) + lessons + prior summaries + anchor_date
        ↓
   Stage 1: Extract     → draft records (with citations, confidence, updates_record_id)
        ↓
   Stage 2: Review      → revise drafts in place (+ user corrections)
        ↓
   Stage 3: Write       → OKF bundle on disk; lessons if corrections warrant
        ↓
   Chat summary
```

Query path (no new transcript): `index.md` + `records/index.md` → load matches → answer with citations. See [query.md](references/query.md).

---

## Most Common Agent Mistakes

| Mistake | Correct approach |
|---------|------------------|
| Skipping Step 0 on a GitHub-cloned skills install | Fetch/pull when `origin` is `inwrkai/continuity` (or legacy `ops-brain`) and workspace ≠ `SKILL_DIR` |
| Running `git pull` while developing this repo | Skip update check when workspace root is `SKILL_DIR` |
| Hallucinating fields not in transcript | Only extract fields clearly present or directly inferable |
| Merging all items of the same object type into one record | One draft = one real-world item |
| Inventing `record_id` for updates | Copy `record_id` only from loaded prior records |
| Storing relative dates ("Thursday") without anchoring | Resolve against `anchor_date` to ISO 8601; keep original phrasing in citations |
| Writing numeric confidence scores | Use `high` / `medium` / `low` only |
| Loading every `records/*.md` on every run | Read `records/index.md` first; load full files only for likely matches |
| Updating lessons from every self-review | Prefer user corrections; add self-review lessons only for recurring patterns |
| Re-running extraction to answer a read question | Use [query.md](references/query.md) |
| Referencing external pipeline code | Skill is standalone — agent performs all stages directly |

---

## Output Location

Default: `okf/` in the current workspace. User may override. See [okf-output.md](references/okf-output.md) for the full bundle structure.
