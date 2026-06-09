---
name: 01-cast
description: Initializes or synchronizes project documentation in B2 English across docs/tasks.md, docs/knowledge_base.md, readme.md, docs/sessions.md, and docs/data_lineage.md. Use when starting a project, scoping work, auditing repo vs documentation, or when the user invokes 01_cast, or project documentation setup.
---

## Role & Objective
Expert developer assistant. Initialize or continuously synchronize project documentation in B2 English. `/docs/tasks.md` and `/docs/knowledge_base.md` are absolute ground truth. Assume Python unless told otherwise.

## Non-Destructive Guardrail (CRITICAL)
Treat all files as append-only ledgers. NEVER delete, overwrite, or truncate user-written text, notes, or code in any file. If an update would touch user-written content, STOP: show a diff-style summary of what would change and ask explicit permission before proceeding. Append new data or integrate cleanly around existing content.

## Run Initialization (Every Run)
Before any phase, print:
- Skill: 01_cast | Date: YYYY-MM-DD
- Last sessions.md entry summary (objective + next actions). If none exists, state "No prior session found."
Then proceed with Operational Logic.

## Operational Logic
- **Files missing:** Phase 1A (Initial Scoping).
- **Files exist:** Phase 1B (Audit & Refinement).

## Phase 1A: Initial Scoping Q&A
Ask concisely:
1. Core Project Type? (AI/ML, ETL pipeline, DB build, Viz/Web app, General tool)
2. Data Specs: source, total size, file types, schema/metadata structure?
3. Tech Stack: Python libraries, DB engines, frameworks, design patterns?
4. Knowledge References: paper/repo/doc URLs?
5. Compute & Hardware: location (local/cluster), GPU/VRAM/RAM, runtime limits?
6. Storage & Logging: disk space needed, preferred tracking tool?
7. Timeline: expected hours per task?

## Phase 1B: Repository Audit & Refinement Q&A
Scan all repo directories, scripts, and notebooks. Identify discrepancies against ground truth:
1. Code/features implemented but missing from `tasks.md`.
2. Tasks marked "Todo" or "In-Progress" that appear completed in scripts.
3. Undocumented libraries or data formats used in code but missing from `knowledge_base.md`.
4. Entries in `knowledge_base.md` not referenced by any task in `tasks.md` — flag as orphaned.

Initiate a Q&A loop highlighting each discrepancy. Ask until all are resolved.

## Phase 2: Documentation Execution
Once details are clear, create or update all four files. Respect the Non-Destructive Guardrail on every write.

### /docs/knowledge_base.md
Organize all links and references under these exact four categories:
- **Literature & Theory:** Academic papers, architectural designs, engineering patterns.
- **Libraries & Tools:** Direct documentation links for Python packages, engines, frameworks.
- **Data & Formatting Specs:** Schemas, formatting rules, API guides.
- **Git & Inspiration:** Repositories for code patterns or workflows.

### /docs/tasks.md
For every task, include this exact structured block:
- **Task Name:** [Title] (Status: Todo/In-Progress/Done | Priority: H/M/L)
- **Data & Stack:** Targeted data files, schema specs, Python libraries, frameworks needed.
- **Method & Implementation Strategy:** Chosen methods, architectural logic, links to `knowledge_base.md`.
- **Validation & Testing Protocol:** Exact metrics, integrity checks, pipeline validation, unit/functional tests.
- **Hardware & Resources:** Location, GPU/RAM/VRAM requirements, storage size, time estimate, compute cost estimate (GPU hours + VRAM if a model/method is involved).
- **Risks:** Performance bottlenecks, training instability, data corruption, UI lag, engineering risks.
- **Known Issues & Blockers:** Unresolved technical debt or open questions for this task.

### /readme.md
- **Project Title & Goal:** Short B2 description.
- **Core Stack & References:** Core Python libraries, engines, key documentation links.
- **Infrastructure & Data Summary:** Compute locations, storage targets, data formats, total space needed.
- **Current Status:** One-line task status dashboard: "Tasks: X Done / Y In-Progress / Z Todo"

### /docs/sessions.md
Maintain exactly ONE entry per calendar day. If an entry for today exists, append to it. Never create a second entry for the same date.
```
### YYYY-MM-DD: Daily Summary
- **Daily Objective:** What was planned for today.
- **Decisions & Changes:** Documentation updates, stack choices, schema definitions, codebase reconciliation notes.
- **Next Day Actions:** Top priorities for the next working session.
```

### /docs/data_lineage.md
Create or append. Track: raw source file → processing script → output file, for every data transformation documented in this session. Format:
```
| Source | Script | Output | Notes |
```

## Output Rule
If details are missing or discrepancies exist: output ONLY Q&A questions. If documentation matches reality: output ONLY file update confirmations + one-line status dashboard. No filler.

## Skill Chaining Hint
After Phase 2 completes, suggest: "Run **02_setup** to scaffold the folder structure, or **03_sota** if tasks require a method baseline."