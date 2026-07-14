# Personal Productivity & Career Agent — Implementation Plan

## 1. Vision

Build a personal AI agent that reduces the overhead of Taimoor's dual life as a
graduate student (M.Ed, International Education Policy and Management,
Peabody College, Vanderbilt) and a working professional (Nashville Peer,
Taleemabad), so more time and energy goes toward the work itself rather than
the logistics around it — reading, note-taking, scheduling, tracking,
analysis, and public-facing writing.

The agent is not a single script — it's a set of connected modules that share
a common memory/task store and calendar, each doing one job well.

## 2. Context

- **Academic side:** M.Ed coursework at Peabody — assigned readings,
  discussion prep, assignments, deadlines.
- **Professional side:** Part-time roles at Nashville Peer and Taleemabad —
  meetings, assigned tasks, internal/external documents, data work.
- **Cross-cutting:** Building a public professional profile (LinkedIn,
  published writing) that compounds toward the long-term career goal in
  CLAUDE.md — international education development, policy, research, MEL, or
  consulting roles by graduation (May 2027).

## 3. Scope — Capabilities

Grouped into modules so each can be built and tested independently:

| Module | Capability |
|---|---|
| A. Reading & Coursework Assistant | Ingest assigned readings → summaries (text first, audio/video later) + discussion questions to bring to class |
| B. Task & Calendar Sync | Central task tracker synced to calendar for both academic and professional deadlines |
| C. Meeting Notes & Action Items | Attend/record professional meetings, take notes, extract assigned tasks into Module B |
| D. Document Synthesis | Read professional documents (reports, memos, briefs) and synthesize them, same approach as academic readings |
| E. Data Analysis Assistant | Clean, manipulate, and analyze data for both academic and professional work |
| F. LinkedIn Presence Manager | Draft and help keep LinkedIn profile/posts current and aligned with real work |
| G. Publication Writing Assistant | Turn real work/analysis into blog/op-ed drafts for outlets like Dawn, Tribune, and smaller publications |
| H. Multi-modal Output (later) | Convert text summaries into short voice notes / videos |

## 4. Architecture Overview

```
                         ┌─────────────────────┐
                         │   Shared Memory /    │
                         │   Task Store (DB)    │
                         └──────────┬───────────┘
                                    │
        ┌───────────────┬──────────┼──────────┬───────────────┐
        │               │          │          │               │
   ┌────▼────┐    ┌─────▼────┐ ┌───▼────┐ ┌───▼─────┐   ┌─────▼─────┐
   │ Module A│    │ Module C │ │Module D│ │Module E │   │ Module F/G│
   │ Reading │    │ Meetings │ │  Docs  │ │  Data   │   │ LinkedIn/ │
   │  + Qs   │    │  + Notes │ │Synth.  │ │Analysis │   │  Blogs    │
   └────┬────┘    └─────┬────┘ └───┬────┘ └───┬─────┘   └─────┬─────┘
        │               │          │          │               │
        └───────────────┴────┬─────┴──────────┴───────────────┘
                              │
                       ┌──────▼──────┐
                       │  Module B    │
                       │  Calendar +  │
                       │  Task Sync   │
                       └─────────────┘
```

Every module reads/writes to the same task store so a reading question, a
meeting action item, and a data-analysis deliverable all show up in one place
— synced outward to a real calendar (Google Calendar / Outlook, TBD).

## 5. Module-by-Module Build Steps

### Module A — Reading & Coursework Assistant
1. Define input format: PDF/EPUB/link ingestion for assigned readings.
2. Build a text-extraction + chunking pipeline (handle scanned PDFs via OCR if needed).
3. Build summarization prompt/pipeline tuned for graduate-level policy readings (structure: thesis, key evidence, methodology, critiques).
4. Build a "questions to ask in class" generator — prompted to surface genuine ambiguities/critiques, not generic questions.
5. Add per-course/per-week organization so summaries map to the syllabus.
6. Add assignment support, scoped to: outlining, drafting assistance,
   structuring, and checking Taimoor's own work — not producing
   submittable graded work wholesale. See Guardrails (Section 7).
7. (Later) Add text-to-speech for voice-note summaries; evaluate short-video generation tools.

### Module B — Task & Calendar Sync
1. Sync across all three calendars in use: personal Gmail (Google Calendar
   API), Vanderbilt Outlook (Microsoft Graph API), and the Taleemabad email
   calendar (Google Calendar API or Microsoft Graph, depending on whether
   Taleemabad runs Google Workspace or Microsoft 365 — confirm which).
2. Route events to the right calendar by source: academic → Vanderbilt
   Outlook, Taleemabad tasks → Taleemabad calendar, personal → Gmail. Build
   a merged read-view across all three so nothing gets missed, even though
   writes go to separate calendars.
3. Task store: starting recommendation is a lightweight custom store
   (SQLite) rather than an existing tool — it's the simplest for the agent
   to read/write directly and avoids being locked into a third-party API's
   rate limits/schema this early. Trade-off: no ready-made UI, so the
   digest view (step 5) is the interface for now. This can be swapped for
   Notion/Todoist later if a visual UI becomes more valuable than direct
   control — revisit after the MVP is in daily use.
4. Build a unified task schema: `{ source (personal/academic/Taleemabad/Nashville Peer), title, due_date, status, origin_module }`.
5. Build calendar sync: tasks with due dates create/update events on the
   correct calendar; completed tasks are removed/marked.
6. Build a daily/weekly digest view pulling across all three calendars
   (what's due, what's overdue, what's upcoming).

### Module C — Meeting Notes & Action Items
1. Platforms confirmed: Microsoft Teams and Zoom, both used across Nashville
   Peer / Taleemabad. Build against both platforms' native transcription
   (Teams transcripts, Zoom cloud recordings/transcripts) rather than a
   separate recording bot, to keep this inside each platform's own
   consent/data-handling flow.
2. Build a note-summarization pipeline: key decisions, open questions, action items.
3. Push extracted action items into Module B's task store automatically.
4. **Status:** build and test this module using sample/mock transcripts
   first. Taimoor will confirm organizational consent for recording/
   transcription with Nashville Peer and Taleemabad separately — do not
   connect this module to real, live meetings until that's confirmed.
   Treat this as a hard gate before Phase 3 deployment, not before Phase 3
   development.

### Module D — Document Synthesis (Professional)
1. Reuse Module A's ingestion/summarization pipeline as the base.
2. Add a professional-document mode: internal reports, briefs, MEL documents — tuned for extracting decisions/implications rather than academic critique.
3. Tag outputs by project/organization so synthesis stays organized across Nashville Peer vs. Taleemabad work.

### Module E — Data Analysis Assistant
1. Define standard intake: raw data format (CSV/Excel/SQL/survey exports).
2. Build a cleaning step: missing values, type coercion, deduplication, validation checks.
3. Build a manipulation layer: joins, aggregations, reshaping (pandas or equivalent).
4. Build an analysis layer: descriptive stats as the primary, everyday use
   case (this is what gets used most); regression analysis (linear/logistic
   as appropriate) as the upper bound of complexity — no need to build
   beyond that for now.
5. Standardize output: a reproducible notebook/script per analysis + a plain-language summary of findings.
6. Version control every analysis (this doubles as portfolio evidence per CLAUDE.md priority #2).

### Module F — LinkedIn Presence Manager
1. **Starting mode: manual-assist only, no API automation.** LinkedIn's
   public API requires a formal partner application and approval process
   that isn't set up — rather than take that on now, this module drafts
   posts and Taimoor copies/pastes and publishes them himself. This can be
   revisited later if LinkedIn API access becomes worth pursuing.
2. Build a draft generator: turns real completed work (from Modules A/D/E) into short LinkedIn update drafts.
3. Human-in-the-loop review step — Taimoor approves/edits before anything posts, consistent with the "never fabricate" rule.
4. Track a cadence (e.g., one update every 1-2 weeks) tied to actual milestones, not filler content.

### Module G — Publication Writing Assistant
1. Build a pipeline that turns a finished analysis/reflection into a longer-form draft (op-ed/blog length).
2. Tune tone/structure per target outlet (Dawn, Tribune, smaller regional/education outlets) — these have different style and word-count norms; research each outlet's submission guidelines before drafting.
3. Human review + fact-check pass before submission — every claim must trace back to real evidence per CLAUDE.md.
4. Track submissions and outcomes to build a personal track record over time.

### Module H — Multi-modal Output (Later Phase)
1. Evaluate TTS tools for voice notes (only after Module A's text summaries are solid).
2. Evaluate short-form video generation only if there's a clear use case — don't build this before the text pipeline is proven useful day-to-day.

## 6. Shared Infrastructure

- **Orchestration:** a single agent loop (see the Observe → Decide → Act →
  Get Feedback → Improve pattern already used in this project) coordinating
  which module handles an incoming item (a reading, a meeting, a dataset).
- **Storage:** one task/memory store all modules read and write to.
- **Auth:** per-integration OAuth (Google Calendar, LinkedIn if applicable) —
  keep credentials out of the repo; use environment variables/secrets
  management.

## 7. Guardrails (non-negotiable)

- **Academic integrity:** confirmed scope is outlining, drafting assistance,
  structuring, and checking Taimoor's own work — the agent does not produce
  submittable graded work wholesale. Still confirm this scope is consistent
  with Vanderbilt's/Peabody's academic integrity and AI-use policy before
  first real use.
- **Confidentiality:** professional meeting notes and internal documents from
  Nashville Peer and Taleemabad must not be stored, processed, or shared in
  ways that violate those organizations' confidentiality expectations.
  Module C is being built now against Teams/Zoom native transcription, but
  will not touch real, live meetings until Taimoor confirms consent with
  both organizations — treat that confirmation as a hard gate before
  deployment, independent of when development finishes.
- **No fabrication:** LinkedIn updates and blog drafts must only describe
  real work — consistent with CLAUDE.md's "never fabricate" rule.

## 8. Technology Stack (proposed — confirm before committing)

- **Agent/orchestration:** Claude (via API or Claude Agent SDK) as the
  reasoning layer.
- **Calendar:** Google Calendar API for personal Gmail; Microsoft Graph API
  for Vanderbilt Outlook; Google Calendar API or Microsoft Graph for
  Taleemabad, depending on which platform Taleemabad runs (still open —
  see Section 11).
- **Task store:** starting with a lightweight custom DB (SQLite) — see
  Module B, Section 5, for the reasoning. Revisit if a UI-first tool
  (Notion/Todoist) turns out to matter more once the MVP is in daily use.
- **Data analysis:** Python (pandas, matplotlib/plotly, Jupyter notebooks),
  with `statsmodels`/`scikit-learn` for the regression-analysis ceiling.
- **Document ingestion:** PDF/text extraction library + OCR fallback.
- **Meeting transcription:** Teams and Zoom native transcripts (both
  platforms are in use), gated on organizational consent before live use.
- **LinkedIn:** no API integration for now — manual-assist drafting only
  (see Module F).

## 9. Phased Roadmap (aligned to July 2026 – May 2027 window)

- **Phase 0 (Jul 2026):** Finalize scope decisions (see Open Questions
  below), pick the tech stack, set up the shared task store.
- **Phase 1 (Aug–Sep 2026):** Ship Module A (reading summaries + questions)
  and Module B (task/calendar sync) as the MVP — these have the fastest
  payoff for coursework starting in the fall term.
- **Phase 2 (Oct–Nov 2026):** Add Module E (data analysis assistant) —
  directly strengthens the "technical and analytical credibility" priority
  in CLAUDE.md and produces portfolio-ready work.
- **Phase 3 (Dec 2026–Jan 2027):** Add Module D (professional document
  synthesis) and Module C (meeting notes), once consent/confidentiality
  questions are resolved.
- **Phase 4 (Feb–Mar 2027):** Add Module F (LinkedIn) and Module G
  (publication writing) — turn the accumulated real work from Phases 1-3
  into public-facing portfolio evidence.
- **Phase 5 (Apr–May 2027):** Add Module H (voice/video) only if the text
  pipeline has proven useful; otherwise use this window for polish and
  graduation-adjacent job search support instead.

## 10. Success Metrics

- Time saved per week on reading/note-taking/scheduling overhead.
- Number of real analyses/documents produced that are portfolio-ready.
- Cadence and quality of LinkedIn updates and published pieces.
- Zero academic-integrity or confidentiality incidents.

## 11. Open Questions

Resolved:

1. ~~Which calendar?~~ → All three: personal Gmail, Vanderbilt Outlook, and
   the Taleemabad email calendar.
2. ~~Task store?~~ → Starting with a lightweight custom store (SQLite);
   revisit later if a UI-first tool becomes more valuable (Section 5/8).
3. ~~What does "do the assignments" mean?~~ → Outlining, drafting
   assistance, structuring, and checking Taimoor's own work.
4. ~~Meeting platforms/consent?~~ → Teams and Zoom. Build now against
   sample data; Taimoor will confirm organizational consent before this
   touches real meetings.
5. ~~LinkedIn API access?~~ → Not set up / not something Taimoor is set up
   to configure right now — Module F starts as manual-assist drafting only,
   no API integration.
6. ~~Priority data-analysis methods?~~ → Descriptive statistics primarily,
   with regression analysis as the upper bound of complexity needed.

Still open, needed before Phase 0 is fully closed out:

1. Does Taleemabad run Google Workspace or Microsoft 365 for its email/
   calendar? This determines which API (Google Calendar vs. Microsoft
   Graph) Module B uses for that calendar.
2. Confirmation from Vanderbilt/Peabody's academic integrity policy that
   the outlining/drafting/structuring/checking scope for Module A is
   within bounds.
3. Organizational consent from Nashville Peer and Taleemabad for meeting
   recording/transcription (Module C) — required before live deployment,
   not before development.
