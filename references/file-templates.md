# File templates — `.sred/` scaffolds

Instantiate these exactly. Replace `<...>` placeholders. Keep provenance tags
(`[evidence-backed]`, `[attested]`, `[unverified]`) on claims and time entries. Use ISO
dates (`YYYY-MM-DD`) and ISO week numbers (`YYYY-Www`).

---

## `.sred/config.yml`

```yaml
# SR&ED documentation config — edit as needed, then commit.
fiscal_year_end: "<MM-DD>"        # e.g. "12-31" or "03-31"; set on first run
standard_week_hours: 40           # used to bound/reconcile time entries
capture_period: "weekly"          # "weekly" or "sprint" (2-week)
language: "en"                    # "en" or "fr" — narrative language
# Paths excluded from candidate-project nomination (routine subsystems, generated code):
exclude:
  - "admin/**"
  - "utils/**"
  - "**/*.lock"
  - "**/node_modules/**"
  - "**/generated/**"
  - "**/vendor/**"
# Optional: paths to prioritize as SR&ED-likely during nomination
include_priority: []
```

## `.sred/state.json`

```json
{
  "last_captured_commit": "<full-sha>",
  "last_captured_date": "<YYYY-MM-DD>",
  "developers": {
    "<git-email>": { "name": "<git name>", "last_logged_period": "<YYYY-Www>" }
  }
}
```

## `.sred/projects/<slug>/project.md`

```markdown
# <Project title>

- **Slug:** <slug>
- **Status:** active | dormant | closed
- **Fiscal year(s):** <FY2025-2026, ...>
- **Paths:** <glob>, <glob>          <!-- scoping guide for mining; MAY overlap other projects -->
- **Created:** <YYYY-MM-DD>

## Technological uncertainty (theme)
<One or two sentences: the knowledge gap this project exists to resolve.
NOT a business goal.>

## Advancement sought
<What new technological knowledge/capability the work aims to produce.>
```

## `.sred/projects/<slug>/journal.md`

Append-only, uncapped, contemporaneous. One dated, attributed block per capture. This is
the raw evidence the narrative is later synthesized from.

```markdown
# Journal — <Project title>

## <YYYY-MM-DD> — <developer name>
- **Uncertainty/problem worked on:** <...>  [evidence-backed | attested | unverified]
- **Hypothesis / approach tried:** <...>
- **What happened / what was measured:** <...>  (evidence: <sha>, PR #<n>, <file:line>)
- **Conclusion / next step:** <...>
- **Outcome:** worked | failed | inconclusive  (failure is still valid SR&ED knowledge)
```

## `.sred/projects/<slug>/narrative.md`

The T661-shaped deliverable. Word caps enforced: 242 ≤ 350, 244 ≤ 700, 246 ≤ 350.
Synthesized only from this project's journal + evidence. Never padded; never copied.

```markdown
# Narrative — <Project title>

- **Status:** draft | dev-reviewed      <!-- never "accepted"; that is management's call -->
- **Reviewed by:** <name> on <YYYY-MM-DD>
- **Word counts:** 242: <n>/350 · 244: <n>/700 · 246: <n>/350
- _Not tax advice. Final eligibility and filing determined by management/an SR&ED professional._

## Line 242 — Scientific or technological uncertainties (≤350 words)
**Business background and normal activities.** <...>
**Objective of the claimed work.** <...>
**Why available solutions/knowledge were insufficient.** <...>

## Line 244 — Work performed to overcome them (≤700 words)
**Hypothesis / starting point.** <...>
**Work done to test it.** <...>
**Data or feedback collected.** <...>

## Line 246 — Advancements achieved or attempted (≤350 words)
<New technological knowledge gained, including from approaches that did not work.>
```

## `.sred/projects/<slug>/evidence.md`

References, not copies. Snapshot only artifacts that are NOT under version control.

```markdown
# Evidence index — <Project title>

| Claim (journal/narrative reference) | Type   | Reference                    | Provenance       |
|-------------------------------------|--------|------------------------------|------------------|
| <short claim>                       | commit | <sha> — <subject>            | evidence-backed  |
| <short claim>                       | PR     | #<n> — <title>               | evidence-backed  |
| <short claim>                       | test   | <path>::<test name>          | evidence-backed  |
| <short claim>                       | bench  | <file:line> — <result>       | evidence-backed  |

## Snapshots (non-versioned artifacts)
- <filename> — captured <YYYY-MM-DD> from <origin>; stored at `snapshots/<file>`
```

## `.sred/timesheets/FY<start>-<end>/<period>/<developer>.md`

One file per developer per period (no merge conflicts). `<period>` is e.g. `2026-W05`.

```markdown
# Timesheet — <developer name> — <period>

- **Standard week:** <N>h   · **Total SR&ED hours this period:** <sum>
- **Total hours worked this period (for reconciliation):** <N or "standard">

| Date       | Project | SR&ED hours | Activity category        | Note (what was attempted)        | Evidence        | Provenance      |
|------------|---------|-------------|--------------------------|----------------------------------|-----------------|-----------------|
| YYYY-MM-DD | <slug>  | <h>         | experimental development | <one line>                       | <sha>, PR #<n>  | evidence-backed |
| YYYY-MM-DD | <slug>  | <h>         | support: programming     | <one line>                       | <sha>           | attested        |
```

Activity categories: `experimental development`, or support work —
`support: engineering | design | operations research | mathematical analysis |
computer programming | data collection | testing`.

## `.sred/README.md` (generated into the repo)

```markdown
# `.sred/` — SR&ED documentation

This folder holds contemporaneous documentation for a Canadian SR&ED tax credit claim:
per-project technical narratives, time logs, and an evidence trail. It is maintained by
the `sred-documentation` skill.

**Commit this folder.** Its git history is the dated, tamper-evident record of when the
work was documented — valuable evidence if the claim is reviewed.

- `projects/<slug>/` — one folder per SR&ED project (definition, raw journal, T661-shaped
  narrative, evidence index).
- `timesheets/FY.../<period>/<developer>.md` — hours by developer by project.
- `html/index.html` — auto-generated dashboard for managers (open in any browser; do not
  edit by hand — it is regenerated from the Markdown files on each capture and review).
- `exports/` — generated CSV + narrative bundle for management/your accountant.

**What this is not:** this folder does not contain dollar amounts, expenditure or credit
calculations, or filed forms. Those are produced downstream by management and your
accountant or SR&ED consultant. Nothing here is tax advice; final eligibility and filing
are their determination.
```
