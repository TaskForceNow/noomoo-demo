# AGENTS.md

Instructions for coding agents (Claude Code and others) working on this project. The project uses **noomoo** for deterministic, evidence-based documentation checks and task-quality gates.

## Restore context (in this order)
1. `npx noomoo context-status --write` — rebuilds `.agent/context/status.md` and `.agent/context/current-state.md` from the repository
2. This file (`AGENTS.md`)
3. Full workflow rules: `node_modules/noomoo/docs/WORKFLOW_RULES.md`
4. `.agent/context/handoff.md` — check its header: a stale handoff describes an earlier state
5. `.agent/context/current-state.md`
6. `git status --short --branch`

## Non-negotiable rules
- Communicate and write project documentation in English.
- Do not create, modify, or delete files without explicit owner approval.
- Get approval for a task plan before starting (what, how, where, and how to verify). Ask when uncertain; do not invent details.
- Write each Markdown paragraph, list item, and blockquote on **one physical line**; do not hard-wrap text. Separate paragraphs with blank lines only. The `prose-wrap` pre-commit check rejects wrapped paragraphs.
- Reports must contain verifiable facts, numbers, and source references. Before writing, read `node_modules/noomoo/docs/reports/TEMPLATES.md` and `docs/GLOSSARY.md`: the `reports` stage checks headers, sections, and language. Report defects in the noomoo core to `docs/CONTOUR-BACKLOG.md`, not the project backlog.

## Required validation pipeline
- Before starting a task: `npx noomoo task-ready TASK-NNN` — proceed only with a passing gate.
- Pre-commit automatically runs the documentation gate (`.githooks/`, installed by `npx noomoo init --yes`); a failing gate blocks the commit.
- Full audit: `npx noomoo validate`. Status: `docs/validation/STATUS.md`.

## Documentation naming
`requirements/req-NNN-*` · `use-cases/uc-NNN-*` · `features/feat-NNN-*` · `tasks/task-NNN-*` · `reports/report-NNN-*` · `decisions/NNN-*` · `proposals/NNN-*`. IDs: `TASK-NNN`/`FEAT-NNN`/`REQ-NNN-I`. Use **one entity per file**, with the chain requirement → use case → feature → task. Layer templates are in `docs/*/TEMPLATES.md` inside the package.

Store raw artifacts (calls, transcripts, images) in `docs/raw/`, outside traceability and gate checks. Filenames are unrestricted; turn source materials into structured project documents.

## Agent Context Memory
`.agent/context/status.md` and `.agent/context/current-state.md` are built from the repository by `npx noomoo context-status --write` and are not tracked. `.agent/context/handoff.md` is the agent's snapshot with fixed sections (done in this session, current work, open owner decisions, blocking questions, one next action); rewrite it after commit/push/PR/merge, major decisions, and before ending a session, then stamp it with `npx noomoo context-status --handoff <active|completed|blocked>`. All three are a convenience view, not acceptance evidence. In case of conflict, precedence is: Git → project documentation → workflow rules.
