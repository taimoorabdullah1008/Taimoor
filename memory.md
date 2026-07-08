# Memory Journal

A running log of what we worked on and what I learned along the way.

---

## 2026-07-03 — Set up the project repository

**Task:** Initialized the `Taimoor` repository with a README, an initial "Pilot Project" file, and uploaded early planning materials.

**Lesson learned:** Starting with a minimal, working structure (even just a README and one file) makes it much easier to layer in real documents later — I didn't need everything figured out before creating the repo.

---

## 2026-07-07 — Wrote agent_loop.md

**Task:** Created `agent_loop.md`, explaining the Observe → Decide → Act → Get Feedback → Improve loop using a worked example (a student, Ayesha, who wasn't responding to a reminder email) to show how an agent adjusts its recommendation after feedback fails.

**Lesson learned:** A single concrete example carried through multiple rounds explains a loop far better than describing each step abstractly — the "did it work? if not, try something different" pattern only became clear once tied to a real scenario.

---

## 2026-07-07 — Hit a wall pushing to GitHub

**Task:** Tried to push `agent_loop.md` to the repo and got repeated `403 Forbidden` errors, both through direct git push and the GitHub integration.

**Lesson learned:** Being listed under "Authorized GitHub Apps" is not the same as the app being "Installed" on a repository — authorization just links identity, while installation is the separate step that actually grants read/write permission. I had assumed authorizing was enough.

---

## 2026-07-08 — Fixed the GitHub App installation

**Task:** Installed the Claude GitHub App properly (via github.com/apps/claude) with repository access to `Taimoor`, then successfully pushed `CLAUDE.md` for the first time without manual copy-pasting.

**Lesson learned:** It's worth diagnosing the *exact* cause of a permissions error (installation vs. authorization) rather than working around it repeatedly — the one-time fix saved having to manually paste every future file into GitHub's web UI.

---

## 2026-07-08 — Consolidated CLAUDE.md onto main

**Task:** Moved `CLAUDE.md` from its feature branch onto `main`, so it now sits alongside `planning.md`, `agent_loop.md`, and `README.md` as the shared source of truth for this project.

**Lesson learned:** Keeping the core guiding documents (purpose, rules, priorities) together on the main branch — rather than scattered across feature branches — makes them easier to reference consistently in every future session.
