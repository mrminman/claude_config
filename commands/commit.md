---
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*), Bash(git log:*), Bash(git branch:*), Bash(ls:*), Bash(cat:*), Bash(date:*), Bash(~/.claude/go-to-git-root.sh:*), Bash(~/.claude/find-todo-files.sh:*)
description: Validate task docs, summarize changes strictly from git; optionally accept a feature-slug to incorporate docs/<feature-slug>/sprint-plan.md into context for the commit message; delegate to sub-agents; stage ONLY task/feature docs; commit (no push).
---

# Commit (Docs-First, Evidence-Backed, Sub‑agent Assisted)

> Optional `feature-slug` argument supported. Use only facts from `git` for decisions; sprint plan and chat/PR context are advisory. **Do not push.**  
> Sub‑agents: use **doc-writer** PROACTIVELY for doc edits (task + feature docs); use **commit-message-builder** MUST BE USED to craft the commit message.

## Usage
```
/commit <feature-slug>

    <feature-slug>: Folder name under `docs/<feature-slug>/` that contains `sprint-plan.md`.
```

## 1) Validate the Docs System (STOP on failure)
First, ensure we're working from the git repository root:
```bash
~/.claude/go-to-git-root.sh
```

If the folder `docs/<feature-slug>` doesn't exist or there is no `sprint-plan.md` file within it, STOP.

Run (from project root):
```bash
ls -la docs || true
ls -la docs/tasks || true
~/.claude/find-todo-files.sh
```

**If any required task file is missing OR conflicting variants exist:**
- Output **Validation: missing/conflicts** with a bullet list of issues.
- **STOP** (no edits, no staging, no commit).

> Note: Feature docs (under `docs/features/`) are optional and may be created/updated if justified by evidence.

---

## 2) Summarize Actual Work Done (facts only)
The main source of truth is the `sprint-plan.md` file and the documents referenced from there (such as the TDD and PRD).

Collect additional signals:
```bash
git branch --show-current
git status --porcelain
git diff --name-only
git diff --staged
git log --oneline -5
```

### 2a) Context Hints (optional, non-authoritative)
List helpful hints from chat/PR context if present, clearly labeled. If a sprint plan was found at `docs/<feature-slug>/sprint-plan.md`, summarize its key goals/scope here (clearly labeled as advisory), and note the exact path used.

### 2b) Evidence Matrix (authoritative)
Produce a **Change Summary (evidence-backed)** where each bullet cites files/hunks from git outputs.  
If you must infer intent beyond raw diff, place under **Assumptions**.

---

## 3) Update Documentation in Place (via sub-agent)
Delegate to **doc-writer** (PROACTIVELY) with the evidence and desired targets:
- Allowed in this workflow:  `  
  - Feature docs: any `docs/features/*.md` (create if justified by evidence)
- Minimal diffs, preserve headings/format/style.
- In `completed.md`, append as `YYYY-MM-DD — <short description> [<leading commit hash(es) if relevant>]` using `date +%F`.

Apply the patches returned by **doc-writer**.

If **no doc changes are produced**, output **No doc changes to commit** and **STOP**.

---

## 4) Stage All Changes (Docs + Code)
Stage changes under the allowed doc paths:
```bash
git add -A
```
(Stage all changes — docs and any code/files ready to commit.)

---

## 5) Compose a Single Commit Message (via sub-agent)
Delegate to **commit-message-builder** (MUST BE USED) with the staged diff to craft the message. If a sprint plan was provided and found, include its contents alongside the diff as advisory input and instruct the builder to reflect sprint goals/work items when they align with the staged evidence:
- If staged files are all docs, enforce type `docs` and an appropriate scope (e.g., `tasks`, `features`, or `docs`).
- Must follow Conventional Commits; include emoji; no footers/signatures.

Use the returned `SUBJECT` and (optional) `BODY` verbatim.

Additional guidance for the builder when a sprint plan is present:
- Prioritize staged diff evidence over narrative.
- Use sprint goals to shape the subject/body and to group changes under the sprint’s intent.
- Consider using the `feature-slug` as the `scope` if it improves clarity.

---

## 6) Commit (no push)
```bash
git commit -m "<subject>" -m "<optional body>"
git log --oneline -1
```

---

## Output Checklist
- **Validation**: OK | missing/conflicts (if conflicts, stopped)
- **Context Hints**: (optional list)
- **Change Summary (evidence-backed)**: bullets tied to files/hunks
- **Assumptions**: (if any)
- **Doc Edits**: bullets per file touched
- **Commit**: exact subject (and body if any)

## Guardrails
- Evidence comes from `git` outputs; context (including sprint plan) is advisory only.
- If validation fails → **STOP**.
- If no doc edits → **STOP**.
- Only modify/stage task docs and feature docs in this workflow.
- Do not run tests here.
- Do not push.
