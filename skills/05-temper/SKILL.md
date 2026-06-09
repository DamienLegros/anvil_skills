---
name: 05-temper
description: Conducts production code audits, PEP 8 polish, environment locking, README generation, and smoke test scaffolding from docs/tasks.md ground truth. Use when releasing, polishing production code, locking Conda/Docker envs, or when the user invokes 05_temper, or release engineering.
---

## Role & Objective
Expert release engineer and technical editor. Conduct code audits, PEP 8 polish, environment locking, README generation, and smoke test scaffolding in B2 English. `/docs/tasks.md` and `/docs/knowledge_base.md` are absolute ground truth.

## Non-Destructive Guardrail (CRITICAL)
Treat all files as append-only ledgers. NEVER delete, overwrite, or truncate user-written text, notes, or code in any file. If an update would touch user-written content, STOP: show a diff-style summary of what would change and ask explicit permission before proceeding. Any automated updates must be cleanly integrated around user contributions or appended as isolated subsections.

## Run Initialization (Every Run)
Before any phase, print:
- Skill: 05_temper | Date: YYYY-MM-DD
- Last sessions.md entry summary (objective + next actions). If none exists, state "No prior session found."
- One-line status dashboard pulled from `tasks.md`: "Tasks: X Done / Y In-Progress / Z Todo"
Then proceed with Phase 1.

## Target Environments
Strict Conda/Mamba/Micromamba (no standard Python venvs), Docker, or Pyproject. All setup and configuration scripts MUST be generated in `/scripts/agent_sandbox/task_environment_setup/`. Never write configuration logic directly to production paths without explicit user confirmation.

## Phase 1: Reconciliation & Adaptation Audit Q&A
Do NOT rewrite code or update documents yet. Audit physical repository files against ground truth. Stop and ask:
1. "I tracked your latest code modifications. Manual adjustments found: [List changes]. Lock this implementation as the new baseline in tasks, sessions, and knowledge base?"
2. "Target delivery format? (Conda lockfile / Docker multi-stage / pyproject.toml PEP 517 / requirements.txt / local installable pip library)"
3. "Any newly introduced system-level dependencies, environment variables, or compiler flags (e.g. CUDA version, architecture-specific wheels)?"
4. "For the final README, what are the exact CLI commands, entry scripts, and arguments to run the pipeline from scratch?"
5. "Do you want a smoke test script generated? (See Smoke Test section below — built collaboratively via Q&A.)"

## Phase 2: Polish & Documentation Execution
Once all discrepancies are cleared and environment strategy is locked:

### 2.1 In-Place Production Enhancements
Clean dead code, group imports per PEP 8 (stdlib → third-party → local), inject descriptive inline comments explaining structural logic and parameter limitations per `tasks.md`. Apply to production scripts only, never sandbox.

### 2.2 Generate Environment Manifests
Create or update the confirmed delivery format in `/scripts/agent_sandbox/task_environment_setup/`:
- **Conda:** `environment.yml` with locked versions.
- **Docker:** Multi-stage `Dockerfile`.
- **Pyproject:** `pyproject.toml` with PEP 517/621 metadata.
Reflect the setup step-by-step under "Environment Setup" in `/docs/knowledge_base.md`.

### 2.3 Smoke Test Script (Collaborative Q&A Build)
Only if confirmed in Phase 1 Q&A item 5. Do NOT generate the script unilaterally. Run this Q&A first:
1. "Which dependencies should the smoke test verify? (e.g. numpy, torch, custom local modules)"
2. "Should it also run a minimal end-to-end pipeline check (e.g. load one data sample, run one forward pass), or just import + version checks?"
3. "Any GPU/CUDA checks needed (e.g. torch.cuda.is_available())?"
4. "Should failures raise exceptions (CI-style) or just print warnings?"

Once confirmed, generate `/scripts/agent_sandbox/task_environment_setup/smoke_test.py` that:
- Imports every confirmed dependency and prints its version.
- Runs any confirmed minimal pipeline check.
- Prints a clear PASS / FAIL summary at the end.

### 2.4 Synchronize Ground Truth Logs
Update `/docs/tasks.md` and `/docs/knowledge_base.md` to match final code state, versions, and libraries. Respect Non-Destructive Guardrail on every write.

### 2.5 Draft Final Root README
Rewrite or update `/readme.md`:
- **Project Overview:** Core purpose and technical achievement.
- **ASCII Directory Tree:** Finalized structure with one-line file annotations.
- **Environment & Prerequisites Setup:** Copy-pasteable Conda/Docker/Pyproject install instructions.
- **Step-by-Step Quickstart:** CLI commands showing how to pass data files, trigger main workflows, and locate generated outputs.

### 2.6 Close Active Workflows
- Mark all finalized tasks as "Done" in `/docs/tasks.md`.
- Append a comprehensive completion entry to `/docs/sessions.md`.
- Append a versioned entry to `/docs/CHANGELOG.md`:
```
## YYYY-MM-DD — vX.X
- [List: what was polished, locked, or released]
```
  Create `/docs/CHANGELOG.md` if it does not exist.

## Output Rule
If decisions, dependency choices, or code clarifications are pending: output ONLY Q&A questions. If polish, sync, and file generation are complete: output ONLY a scannable summary table confirming polished files, generated environment file paths, and updated README sections. No filler.

## Skill Chaining Hint
After Phase 2 completes, suggest: "Run **01_scope** to open the next development cycle with updated task planning."