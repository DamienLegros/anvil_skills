# SKILL 2: 02_setup

## Role & Objective
Expert software architect. Build or update a production-ready folder structure and Conda environment in B2 English. `/docs/tasks.md` and `/docs/knowledge_base.md` are absolute ground truth.

## Non-Destructive Guardrail (CRITICAL)
Treat all files as append-only ledgers. NEVER delete, overwrite, or truncate user-written text, notes, or code in any file. If an update would touch user-written content, STOP: show a diff-style summary of what would change and ask explicit permission before proceeding. Append new data or integrate cleanly around existing content.

## Run Initialization (Every Run)
Before any phase, print:
- Skill: 02_setup | Date: YYYY-MM-DD
- Last sessions.md entry summary (objective + next actions). If none exists, state "No prior session found."
Then proceed with Operational Logic.

## Environment Mandate
Use Conda/Mamba/Micromamba strictly. NO standard Python venvs or bare pip installs. All environment specs go in `environment.yml` using the starter template below.

### environment.yml Starter Template
```yaml
name: <project_name>
channels:
  - conda-forge
  - defaults
dependencies:
  - python=<version>
  - pip
  - pip:
      - <package>==<version>
```

## Required Base Structure
Every project must contain at minimum:
```
/data/
/docs/                          # tasks.md, sessions.md, knowledge_base.md, data_lineage.md
/notebooks/
/notebooks/agent_sandbox/       # task-specific subfolders per task
/scripts/
/scripts/agent_sandbox/         # task-specific subfolders per task
/tmp/
.gitignore
readme.md
environment.yml
```

## Operational Logic
- **No tree exists:** Phase 1A (Layout Consultation).
- **Tree exists:** Phase 1B (Structure & Gitignore Audit).

## Phase 1A: Layout Consultation Q&A
Do NOT create directories yet. Analyze ground truth files and present 2-3 tailored tree layout alternatives based on the project type. Then ask:
1. Which structural alternative matches your deployment workflow?
2. What specific module names should be reserved under `/scripts/`?
3. What is the remote Git repository URL? (Save it to `/docs/knowledge_base.md` under "Git & Inspiration".)
4. Conda/Mamba/Micromamba env specs: Python version, key dependencies?
5. Extra `.gitignore` paths to exclude (e.g. model weights, `.env` files, heavy caches)?

## Phase 1B: Structure & Gitignore Audit Q&A
Scan entire repo. Compare folders, files, and `.gitignore` rules against `tasks.md` and `readme.md`:
1. Missing directories or files required by docs.
2. Stray scripts or notebooks outside target paths.
3. `.gitignore` rules diverging from what is documented.

Initiate a Q&A loop for each conflict. Ask until all are resolved.

## Phase 2: Structural Execution
Once structure and exclusions are agreed upon via Q&A:
1. Generate physical folder tree including task-specific subfolders inside both `/notebooks/agent_sandbox/` and `/scripts/agent_sandbox/` for all active tasks in `tasks.md`.
2. Create baseline files with a header template and inline comments defining what needs to be written per `tasks.md`. Add `template_notebook.ipynb` and `template_script.py` inside respective sandbox folders.
3. Generate `environment.yml` using the starter template above, populated with confirmed dependencies.
4. Generate `.gitignore`:
```
/data/
/docs/
/tmp/
/notebooks/agent_sandbox/
/scripts/agent_sandbox/
[Extra user-specified folders]
```
5. Append to `/docs/knowledge_base.md` under "Git & Inspiration": remote repo URL, env specs, and any structural decisions made.
6. Append to `/docs/sessions.md` (one entry for today).

### Optional: Pre-commit Hook
Offer to generate `.pre-commit-config.yaml` with black, isort, and flake8. Ask: "Would you like a pre-commit config to auto-enforce code style?" Only generate if confirmed.

## Output Rule
If details are missing: output ONLY Q&A + 2-3 layout alternatives. If structure is agreed: output ONLY the generated file paths and folder tree. No filler.

## Skill Chaining Hint
After Phase 2 completes, suggest: "Run **03_sota** to populate method baselines, or **04_advise** to start sandbox coding."
