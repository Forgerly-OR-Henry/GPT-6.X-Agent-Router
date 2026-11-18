# Task Progress Workflow

Use this workflow when the progress-tracking conditions in [Global Working Agreements](../global-AGENTS.md#ai-workspace-and-task-progress) apply. This file supplies the detailed procedure; it does not independently authorize file changes or create a tracking requirement for ordinary tasks.

## Workspace Setup and Retention

When auxiliary files must be saved, use this layout at the project root. Create only the directories and files actually needed:

```text
.ai-workspace/
|-- README.md          # Purpose and retention policy
|-- progress/          # Current task progress and history
|   |-- CURRENT.md     # Progress for the current task
|   `-- history/
|       |-- ARCHIVE.md # Consolidated summaries of older tasks
|       `-- ...        # Up to 10 recent individual task files, plus explicit retention exceptions
`-- tmp/               # Infrequent, disposable temporary files
```

- On first creating `.ai-workspace/`, add a `README.md` explaining its purpose and cleanup policy. Do not create an empty workspace for a question or simple operation.
- Reserve `.codex/` for Codex configuration and instructions, and `.agents/` for project skills; neither is a location for ordinary working files.
- Never store passwords, tokens, private keys, database backups, or other sensitive information here. Required source, runtime configuration, migrations, formal documentation, and final deliverables must not exist only in this workspace.
- Use `tmp/` sparingly for reproducible, disposable intermediates that genuinely need a file and do not belong in the project. Do not create temporary files for simple commands, routine investigation, or small notes. Temporary files are not formal deliverables, durable evidence, or task progress.
- Remove task-created temporary files as soon as they are no longer needed and check again before delivery. Keep the workspace tidy without deleting user-created files or retained project evidence.
- Follow repository version-control policy. If none exists, do not commit auxiliary files or change `.gitignore` without authorization.

## Current Task Record

Use `progress/CURRENT.md` as the authoritative record of the current task and the primary source for handoff. Before substantive work on a tracked task, capture the goal and convert each user requirement into a checkable acceptance criterion:

```markdown
# Current Task

- Status: In progress
- Goal:

## Acceptance Criteria

- [ ] Criterion one
- [ ] Criterion two

## Completed

- [x] Completed item and result

## In Progress

- [ ] Current item

## Remaining

- [ ] Pending item

## Blockers

- None

## Verification

- [ ] Tests, build, or result inspection
```

- Update the record after substantive progress, when requirements or decisions change, and before handoff. Update acceptance criteria as soon as the user adds or changes a requirement. A tool call alone does not require a progress update.
- Record concrete results, confirmed decisions, and the context needed to continue. Avoid vague entries such as "handled" or "improved"; do not present an unverified inference as a confirmed fact.
- Before marking complete, check the original request, follow-up requirements, and acceptance criteria against the delivered work and available evidence. Record which checks passed, failed, were skipped, or were inconclusive.
- Difficulty, long runtime, context limits, or partial progress are not reasons to stop while in-scope work can continue. Mark blocked only when further progress requires a missing decision, permission, credential, external-state change, or new authorization. Record the affected requirement, the cause, and the minimum known action needed to unblock it. Follow the working agreements for reporting the blocker to the user.
- When resuming or taking over a task, read its current record, verify the actual project state, and continue from the remaining work. Do not routinely scan all history.
- Never compress `CURRENT.md` or overwrite an unfinished record.

## Completion, Pause, and Task Switching

- On genuine completion, archive the full current record as `progress/history/YYYY-MM-DD-task-name.md`. Use a non-conflicting filename; never overwrite another record.
- Paused and blocked tasks are not complete. Keep their status, remaining work, and blockers in `CURRENT.md` by default.
- If a task must actually be switched while unfinished, first preserve the complete record as `progress/history/YYYY-MM-DD-task-name-paused.md` or `progress/history/YYYY-MM-DD-task-name-blocked.md`, according to its real status. Verify that the archived contents are complete before replacing `CURRENT.md` with a new task. Record the archived path in the handoff so the task can be resumed.
- Do not archive an unfinished task as completed merely to clear the current record.

## History Retention

Apply retention when archiving a task; ordinary work does not require repeated history scans.

- Keep the 10 most recent individual task files, plus any older files the user explicitly asked to retain. Exclude `ARCHIVE.md` from the count.
- Summarize the oldest excess files into the rolling `progress/history/ARCHIVE.md`. Each summary must preserve the original filename, task date and name, outcome, key changes, verification results, and unresolved issues.
- For an unfinished archived task, also preserve its remaining requirements, decisions, blockers, and resumption context. Do not describe it as completed.
- Delete only the summarized excess files after checking that their essential information was preserved. Never remove a history file the user explicitly requested to retain.
- Before moving or deleting files, verify that the resolved targets are within the intended workspace and follow the applicable filesystem safeguards.

Use this format for `progress/history/ARCHIVE.md`:

```markdown
# Progress Archive

## YYYY-MM-DD - Task name

- Original file:
- Outcome:
- Key changes:
- Verification:
- Unresolved issues:
```
