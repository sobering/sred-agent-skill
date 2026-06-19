# Grilling script — the SR&ED interview question bank

Use this to drive the interview. Rules that apply throughout:
- **One question at a time.** Wait for the answer before the next.
- **Mine the codebase first.** If git, files, or tests answer it, don't ask — confirm.
- **Offer a recommended answer** with brief reasoning on every question.
- **Push past vagueness.** "We made it faster / better / more scalable" is a non-answer;
  drill to the specific knowledge gap, what was tried, and what was measured.
- **Accept "routine" and "I don't remember."** Routine work should be de-scoped; missing
  detail is flagged `[unverified]`, never invented.

The questions are grouped by phase. Phases 2–4 map directly to T661 lines 242 / 244 / 246.

---

## Phase 0 — Project framing (do once per project)

- I see activity in `<paths>`; is this part of one effort with a single technical goal, or
  several? (Goal: get to a project boundary defined by a shared uncertainty.)
- In one sentence, what was the hard technical problem here — the part you couldn't just
  look up or assemble from known tools?
- Which subsystems/paths does this project touch? (Confirm the path globs.)

## Phase 1 — Eligibility gate (the filter — be skeptical)

Ask these before investing in a narrative. Any "no" pushes toward *not eligible*.

- At the start, could a competent developer in your field have known whether this would
  work, or how to do it, using standard practice and public knowledge? (If yes → likely
  routine. Recommend not claiming.)
- What specifically was *uncertain* — not "hard," but genuinely unknown at the technology
  level?
- Did you have to **experiment or analyze** to resolve it (build variants, measure, compare),
  or did you implement a known solution?
- Is this mostly UI/styling, CRUD, configuration, integration via documented APIs,
  dependency upgrades, or routine bug-fixing? (If yes → excluded/routine.)

If the gate fails, say so directly: "This reads as routine engineering rather than SR&ED;
I'd recommend not claiming it. Want me to note it as considered-and-excluded?"

## Phase 2 — Uncertainty (T661 line 242, ≤ 350 words)

- Background: what does the business do, and what are your normal commercial activities?
  (Usually answerable from README/docs — confirm rather than ask.)
- What was the **objective** of the work you want to claim?
- What did you try or research **first**, and **why didn't existing solutions or knowledge
  meet your needs**? (Name the standard approaches that failed and why.)
- Restate the uncertainty as a knowledge gap: "It was unknown whether ___ could be achieved
  given ___." Does that capture it?

Probes if answers are thin:
- What constraint made the standard approach fail — scale, latency, concurrency, accuracy,
  memory, hardware, determinism?
- What did you read/evaluate that turned out not to apply?

## Phase 3 — Work performed (T661 line 244, ≤ 700 words)

- What **hypothesis or candidate solution** did you start from?
- Walk me through what you **did to test it**. (Mine commits/branches/tests first, then ask
  for the reasoning and sequence the repo can't show.)
- Did the first approach work? What did you change, and why? (Failed iterations are valuable
  — capture them.)
- What **data or feedback** did you collect — benchmarks, measurements, error rates, test
  results? (Link them as evidence.)
- How did you reach your conclusions from those results?

Probes:
- How many variants/approaches did you try? What distinguished them?
- Where in the repo is the evidence for each step? (Get SHAs / PRs / test files.)

## Phase 4 — Advancement (T661 line 246, ≤ 350 words)

- What **new technological knowledge** came out of this — what do you now know that wasn't
  known (to the field) before?
- If it failed: what did you establish *doesn't* work, and why is that still new knowledge?
- Keep it at the technology level, not the business level (avoid "we shipped the feature").

## Phase 5 — Time (per developer, per period)

Mine git for the developer's commits in the period first, then:

- Here's what the repo shows you touched this `<period>` on this project: `<summary>`.
  Roughly how many hours went to this **SR&ED** work, by day?
- Of those, how much was core experimental work vs **support** (programming, testing, data
  collection)? (Tag accordingly.)
- Your standard week is `<N>`h — does the total look right, or was it a light/heavy week
  (PTO, on-call, other projects)?
- Challenge if needed: "You logged `<X>`h on `<project>` but I see no commits touching its
  paths this period — what's the evidence?" (Flag `[unverified]` if unresolved.)

## Phase 6 — Provenance check (before writing)

For each claim and time entry, decide the tag:
- `[evidence-backed]` — tied to a commit/PR/test/measurement in the repo.
- `[attested]` — developer-stated and plausible but not independently verifiable (hours,
  intent, reasoning).
- `[unverified]` — asserted but unsupported, or the developer was unsure. Surface these
  prominently for management.

## Stopping condition

Stop grilling a project when: all four systematic-investigation steps are articulated with
specifics; the uncertainty is framed as a knowledge gap (not a business want); and every
claim is either evidence-backed/attested or explicitly flagged `[unverified]`. Then write.
