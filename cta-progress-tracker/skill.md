---
name: cta-progress-tracker
description: Measures progress across Salesforce CTA mock scenario solutions by running them through the cta-mock-judge skill and logging per-domain scores over time. Use when the user wants to log a judged mock attempt, see how they're trending across mocks, or find their recurring weak spots before the next mock board.
---

# CTA Mock Progress Tracker

You measure and report on the candidate's progress across their CTA mock scenario attempts.
You do this by driving the **cta-mock-judge** skill against the candidate's written solutions,
turning its qualitative verdict into a numeric trend line, and keeping a running log so the
candidate can see whether they're actually improving — and on which domains they keep getting
stuck.

You never invent scores. Every number you write must trace back to something the judge actually
said in that session's feedback.

## File layout this skill owns

```
mocks/                          # existing: raw mock scenario source files (pdf/docx)
scenarios/<slug>/*.md           # existing: markdown scenario write-ups
solutions/<slug>/
  solution.md                   # candidate's current/latest written solution for this mock
  history/attempt-<N>.md        # snapshot of the solution as it was judged at attempt N
  attempt-<N>-judgment.md       # full judge transcript/output captured for attempt N
progress/
  progress-log.json             # canonical machine-readable record of every judged attempt
  progress-report.md            # human-readable, regenerated report (trend + weak spots)
```

`<slug>` is the mock's name lowercased, with the extension stripped and anything that isn't
`[a-z0-9]` collapsed to a single `-` (e.g. `Universal Coffee Machines.pdf` → `universal-coffee-machines`,
`experience-cloud-mini-cta-scenario.md` → `experience-cloud-mini-cta-scenario`).

If `solutions/`, `progress/progress-log.json`, or `progress/progress-report.md` don't exist yet,
create them (empty log = `{"schemaVersion": 1, "attempts": []}`) rather than failing.

## Scoring model

Score the **6 Core Evaluation Domains** from cta-mock-judge, plus its **4 Phase 2 grading
dimensions**, each on this 1–5 scale:

| Score | Meaning |
|---|---|
| 1 | Critical gap — not addressed, or fundamentally wrong |
| 2 | Weak — addressed but generic/shallow, folded under light pushback |
| 3 | Adequate — defensible default answer, nothing that stands out |
| 4 | Strong — specific to the scenario, well-justified, held up under probing |
| 5 | Exceptional — board-ready; anticipated trade-offs before being asked |

Domains: `system_landscape`, `data_model`, `integration_identity`, `security_sharing`, `alm`,
`risk_tradeoffs`.
Meta dimensions (Phase 2): `breadth`, `depth`, `communication`, `adaptability`.

If the judge's feedback never actually engaged with a domain (didn't ask about it, didn't
critique it), record that domain as `null` ("Not Assessed") for this attempt — do not guess a
number just to fill the cell.

**Overall verdict** = average of the non-null core domain scores for that attempt:
- `< 2.5` → Not Ready
- `2.5–3.5` → Developing
- `3.5–4.3` → Board Ready (borderline)
- `> 4.3` → Board Ready

## Workflow

### 1. Discover what's judgeable
List `mocks/*` and `scenarios/*` to enumerate known scenarios (slug each). For each slug, check
whether `solutions/<slug>/solution.md` exists. Anything without a solution file yet has nothing
to judge — mention it's pending, don't block on it.

### 2. Decide what needs judging this run
For each slug with a `solution.md`:
- If there's no prior attempt in `progress-log.json` for that slug, it needs judging (attempt 1).
- If `solution.md` has changed since the last logged attempt's snapshot in `history/`, it needs
  judging (next attempt number).
- If the user named a specific mock, only process that one.
- Otherwise, judging only happens for mocks the user explicitly asks to log — don't silently
  re-run an unchanged solution.

### 3. Run the judge
For each solution that needs judging, invoke the **cta-mock-judge** skill in full: give it the
scenario content and the candidate's `solution.md` as the proposed solution, and let it run its
normal Phase 1 (Q&A challenge) and Phase 2 (structured feedback report) exactly as designed —
don't shortcut the Q&A. Capture the full session output.

### 4. Extract scores (separate step, after the judge is done)
Now switch hats from "judge" to "scorekeeper." Re-read the judge's Phase 2 report (and Phase 1
exchange) and, for each of the 6 domains + 4 meta dimensions, assign a score per the table above,
with a one-line justification quoting or paraphrasing the specific feedback that justifies it.
Also pull 2–3 concrete `strengths` and 2–3 concrete `gaps` verbatim/paraphrased from the report.

### 5. Persist
- Save the judge's full output to `solutions/<slug>/attempt-<N>-judgment.md`.
- Snapshot the judged solution to `solutions/<slug>/history/attempt-<N>.md`.
- Append an entry to `progress/progress-log.json`:
```json
{
  "id": "<slug>-<N>",
  "mock": "<slug>",
  "date": "<today, YYYY-MM-DD>",
  "attemptNumber": N,
  "solutionFile": "solutions/<slug>/solution.md",
  "judgmentFile": "solutions/<slug>/attempt-<N>-judgment.md",
  "domainScores": { "system_landscape": 3, "data_model": null, ... },
  "metaScores": { "breadth": 3, "depth": 2, "communication": 4, "adaptability": 3 },
  "overallVerdict": "Developing",
  "strengths": ["...", "..."],
  "gaps": ["...", "..."]
}
```

### 6. Regenerate `progress/progress-report.md`
Rebuild the whole report from `progress-log.json` (don't hand-edit incrementally — recompute):
1. **Attempt history table** — mock, attempt #, date, overall verdict, one-line trend arrow vs.
   this mock's previous attempt (↑/↓/→ on average domain score).
2. **Per-domain trend** — for each of the 6 domains, latest score, average score across all
   attempts across all mocks, and how many attempts scored ≤2 on it.
3. **Recurring weak spots** — domains ranked by frequency of scoring ≤2, across all mocks. This
   is the actual "what to fix before the next mock" signal.
4. **Meta-dimension trend** — same treatment for breadth/depth/communication/adaptability, since
   these track exam behavior (how they perform under pressure) separately from technical content.
5. **Suggested focus for next mock** — 2–3 sentences naming the worst recurring domain(s) and
   referencing the specific gap language pulled from judge feedback, not generic advice.

### 7. Report-only mode
If the user just asks "how am I doing" / "show my progress" and nothing new needs judging, skip
straight to reading `progress-log.json` and presenting the current report — don't re-run judging.

## What this skill does NOT do
- It doesn't grade without running cta-mock-judge first — it's a wrapper/logger around that
  skill's verdicts, not an independent judge.
- It doesn't score a domain the judge never actually challenged.
- It doesn't silently overwrite `progress-log.json` history — it appends attempts; corrections to
  a past entry should be called out explicitly to the user before editing.
