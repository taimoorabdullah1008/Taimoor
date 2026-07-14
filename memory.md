# Memory Journal

A running log of what we worked on and what I learned along the way.

---

## 2026-07-14 — Pivoted from the old pilot project to the new agent plan

**Task:** Retired the old "Pilot Project" scope and its files, and defined the
Personal Productivity & Career Agent project in `planning.md` — the modules
for reading/coursework, task & calendar sync, meeting notes, document
synthesis, data analysis, LinkedIn, and publication writing.

**Lesson learned:** Cleanly removing an old project's files before starting
the new one (instead of layering the new plan on top) kept the repo from
turning into a confusing mix of two unrelated projects for future sessions.

---

## 2026-07-14 — Closed the remaining open questions in planning.md

**Task:** Resolved the plan's open questions — which calendars to sync,
task-store choice, what "do the assignments" scope means, meeting-platform
consent, LinkedIn API access, and the data-analysis method ceiling.

**Lesson learned:** Most of these weren't research problems, they were
decisions I already had the constraints for (e.g., Taleemabad runs Google
Workspace, so that settled the calendar API choice) — the plan was stalling
on questions I could have just answered directly instead of leaving open.

---

## 2026-07-14 — Wrote a new CLAUDE.md for the project

**Task:** Drafted a CLAUDE.md for this project — under 50 lines, covering
purpose, non-negotiable rules, pointers to `planning.md`/`agent_loop.md`, and
working style — as a class assignment on memory engineering.

**Lesson learned:** The 50-line limit forced real prioritization. Cutting the
guardrails down to the essentials (integrity, confidentiality, no
fabrication, the consent gate) made the non-negotiables sharper, not weaker
— length was hiding vagueness, not adding precision.

---

## 2026-07-14 — Caught a silent conflict with an existing CLAUDE.md

**Task:** Before treating the new CLAUDE.md as done, checked git history and
found `main` already had an older CLAUDE.md (general career-strategy rules)
that a prior commit on this branch had deliberately removed as part of the
pivot.

**Lesson learned:** Always check git history before adding a file the README
already points to. Skipping that check would have meant merging a silent
overwrite into main without understanding why the old file was gone in the
first place.

---

## 2026-07-14 — Opened PR #1 instead of merging straight to main

**Task:** Pushed the branch and opened a pull request bundling the whole
pivot (removed files, resolved questions, new CLAUDE.md) rather than
merging directly, then paused for review instead of merging right away.

**Lesson learned:** Bundling a multi-step pivot into one PR made it possible
to see the whole change at once — and pausing before merge turned it into an
actual review checkpoint instead of a rubber stamp.
