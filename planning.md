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
6. (Later) Add text-to-speech for voice-note summaries; evaluate short-video generation tools.

### Module B — Task & Calendar Sync
1. Choose calendar backend (Google Calendar API is the likely default — confirm which calendar Taimoor actually uses).
2. Choose/confirm a task store (lightweight DB, or an existing tool like Notion/Todoist via API — see Open Questions).
3. Build a unified task schema: `{ source (academic/professional), title, due_date, status, origin_module }`.
4. Build calendar sync: tasks with due dates create/update calendar events; completed tasks are removed/marked.
5. Build a daily/weekly digest view (what's due, what's overdue, what's upcoming).

### Module C — Meeting Notes & Action Items
1. Confirm meeting platform(s) used at Nashville Peer / Taleemabad (Zoom, Teams, Meet, in-person).
2. Evaluate transcription approach: platform-native transcripts vs. a bot/recorder vs. manual audio upload.
3. **Flag before building:** get explicit consent from meeting hosts/organizations before recording or transcribing any professional meeting — this is a workplace confidentiality issue, not just a technical one.
4. Build a note-summarization pipeline: key decisions, open questions, action items.
5. Push extracted action items into Module B's task store automatically.

### Module D — Document Synthesis (Professional)
1. Reuse Module A's ingestion/summarization pipeline as the base.
2. Add a professional-document mode: internal reports, briefs, MEL documents — tuned for extracting decisions/implications rather than academic critique.
3. Tag outputs by project/organization so synthesis stays organized across Nashville Peer vs. Taleemabad work.

### Module E — Data Analysis Assistant
1. Define standard intake: raw data format (CSV/Excel/SQL/survey exports).
2. Build a cleaning step: missing values, type coercion, deduplication, validation checks.
3. Build a manipulation layer: joins, aggregations, reshaping (pandas or equivalent).
4. Build an analysis layer: descriptive stats first; add whatever inferential/MEL-specific methods (e.g., pre/post comparisons, indicator tracking) are actually used in coursework or work.
5. Standardize output: a reproducible notebook/script per analysis + a plain-language summary of findings.
6. Version control every analysis (this doubles as portfolio evidence per CLAUDE.md priority #2).

### Module F — LinkedIn Presence Manager
1. Note: LinkedIn does not offer a general public API for automated posting — confirm what's actually feasible (manual-assist drafting vs. an approved LinkedIn API app) before assuming automation.
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

- **Academic integrity:** the agent assists with understanding readings and
  organizing assignments — it does not complete graded work on Taimoor's
  behalf. Confirm Vanderbilt's/Peabody's academic integrity and AI-use
  policy before deciding what "do the assignments" can mean in practice.
- **Confidentiality:** professional meeting notes and internal documents from
  Nashville Peer and Taleemabad must not be stored, processed, or shared in
  ways that violate those organizations' confidentiality expectations —
  get explicit consent before recording meetings.
- **No fabrication:** LinkedIn updates and blog drafts must only describe
  real work — consistent with CLAUDE.md's "never fabricate" rule.

## 8. Technology Stack (proposed — confirm before committing)

- **Agent/orchestration:** Claude (via API or Claude Agent SDK) as the
  reasoning layer.
- **Calendar:** Google Calendar API (pending confirmation of which calendar
  is actually used).
- **Task store:** lightweight DB (SQLite/Postgres) or an existing tool via
  API (Notion/Todoist) — trade-off: a custom DB gives full control, an
  existing tool gives a UI for free. Needs a decision.
- **Data analysis:** Python (pandas, matplotlib/plotly, Jupyter notebooks).
- **Document ingestion:** PDF/text extraction library + OCR fallback.
- **Meeting transcription:** platform-native transcripts where available,
  to avoid separate recording-consent complexity.

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

## 11. Open Questions (need Taimoor's input before building)

1. Which calendar does Taimoor actually use day-to-day (Google/Outlook/other)?
2. Preference for task store: custom build vs. existing tool (Notion, Todoist, etc.)?
3. What does "do the assignments" mean in practice, given academic integrity
   rules — outlining/drafting support, or something narrower?
4. What meeting platform(s) are used at Nashville Peer and Taleemabad, and
   has consent for recording/transcription been discussed with those teams?
5. Is there existing LinkedIn API access, or should Module F start as a
   manual-assist drafting tool only?
6. Which specific data-analysis methods come up most in coursework/work
   right now, to prioritize Module E's build order?
