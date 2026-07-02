# AGENTS.md - Planning Repository Contract

The org-wide `.github/AGENTS.md` and `.github/ENGINEERING.md` bind fully here.
This file adds the planning repository contract.

## Mission

Planning docs are decision records for humans.
They explain order, gates, dependencies, and rationale.

## Editing rules

Agents may propose edits, but MUST NOT rewrite history.
`ROADMAP.md` and `BUILD-ORDER.md` changes cite what changed and why.
Task IDs are stable forever.
Issues reference task IDs.
Do not renumber or reuse task IDs.
If a task is superseded, mark it superseded and point to the replacement.

## Phase gates

Phase exit criteria may be edited only by explicit maintainer decision; gate changes state the decision, reason, and affected phase.
Implementation code respects the gates in `BUILD-ORDER.md`.
Markdown design notes may proceed before gates only when planning docs allow it.

## Never do this

- Never rewrite old rationale to make a new decision look preexisting.
- Never remove a dependency without explaining the replacement.
- Never edit phase exit criteria as drive-by cleanup.
