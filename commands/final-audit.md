---
description: Final audit of changes that have been made.
---
# final-audit
Final gauntlet of reviews by expert agents (typically done just before committing)

## Usage
```
/final-audit <optional: feature-slug>

    <feature-slug>: Specify the folder of the sprint-plan.md you're working from
```

## Description
Run a comprehensive release audit across Security, Edge Cases, Performance, then Simplification. 
Use subagents PROACTIVELY.
If a <feature-slug> was provided, look up a sprint plan, typcially in `docs/<feature-slug>/sprint-plan.md` to see what work items were just completed to understand the scope and intent of the work.  Share the sprint-plan.md file with any sub-agents called upon to do work.

## Prompt
You are the audit conductor. Based on `{{args}}` and repo context pass the following to each agent:
- Scope: {{args.scope | default:"changed"}} 
- Strict mode: {{args.strict | default:"false"}}
- Skips: {{args.skips}} or use best judgement based on the file list and the role of each agent
- Context: Context from the {{args}}, the sprint-plan.md path if available.

Steps:
1) Delegate to **product-champion**  on the scope of changes to ensure the implementation is complete and correct and aligned with the product vision.  
2) Fix all issues found before continuing.
3) Delegate to **security-auditor** on the scope of changes.
4) Fix high/critical priority issues before continuing.
5) Delegate to **edge-case-bug-hunter** on the scope of changes. 
6) Fix high/critical priority issues before continuing.
7) Delegate to **performance-optimizer** on hot paths or scope.  Stop and fix high/critical priority issues before continuing.
8) Fix high/critical priority issues before continuing.
9) Run ALL tests.
10) Fix all issues before continuing.
11) Build all touched binaries.
12) Fix all build errors and warnings before continuing.
13) Produce a single markdown report with:
   - Executive summary (pass/fail + release risk from all agents)
   - Findings by agent (severity, location, with a green check if already fixed, a red X if critical and unfixed, and a yellow bang if low/medium risk and unfixed)
   - Make a recommendation whether to commit or not. If recommending 'commit' suggest the `/commit` command.
