# SR&ED Documentation Skill

A [Claude](https://claude.ai) skill that helps Canadian software teams produce **accurate,
defensible documentation** for an **SR&ED tax credit claim** — the technical write-ups, the
time tracking, and the evidence trail — straight from your codebase.

It runs inside your repository, reads your git history, and **interviews you one question
at a time** to capture what a claim actually needs. Everything it produces lands in a
`.sred/` folder at the root of your repo, ready to hand up to management and your
accountant. It also generates a **browsable HTML dashboard** so managers can see the
claim's status at a glance without reading Markdown files.

> **You do not need to know anything about SR&ED to use this skill.** This README teaches
> you the concepts as you go, and the skill itself is built to coach you, push back when
> something doesn't look eligible, and never let you over-claim.
>
> **What's a "skill"?** A skill is a set of instructions that tells Claude (the AI) how to
> behave for a specific task. You don't install a program — you place this folder in your
> project, and when you talk to Claude about SR&ED, it follows these instructions
> automatically. You interact with it by typing natural-language requests ("log my SR&ED
> work this week") and it interviews you, reads your code, and writes the documentation.

---

## Table of contents

1. [What is SR&ED? (a plain-language primer)](#1-what-is-sred-a-plain-language-primer)
2. [What this skill does — and what it deliberately does *not* do](#2-what-this-skill-does--and-what-it-deliberately-does-not-do)
3. [Key concepts and glossary](#3-key-concepts-and-glossary)
4. [Installing and setting up](#4-installing-and-setting-up)
5. [The `.sred/` folder, explained](#5-the-sred-folder-explained)
6. [The three modes](#6-the-three-modes)
7. [Walkthrough 1: first run (onboarding)](#7-walkthrough-1-first-run-onboarding)
8. [Walkthrough 2: a weekly capture session](#8-walkthrough-2-a-weekly-capture-session)
9. [Walkthrough 3: review/audit before filing](#9-walkthrough-3-reviewaudit-before-filing)
10. [A worked software example (eligible vs routine)](#10-a-worked-software-example-eligible-vs-routine)
11. [How time tracking works](#11-how-time-tracking-works)
12. [How evidence and provenance work](#12-how-evidence-and-provenance-work)
13. [Handing off to management](#13-handing-off-to-management)
14. [The manager dashboard](#14-the-manager-dashboard)
15. [How it stays honest](#15-how-it-stays-honest)
16. [FAQ](#16-faq)
17. [Disclaimer and sources](#17-disclaimer-and-sources)

---

## 1. What is SR&ED? (a plain-language primer)

**SR&ED** (Scientific Research and Experimental Development, pronounced "shred") is a
Canadian federal tax incentive run by the Canada Revenue Agency (CRA). If your business
does qualifying R&D in Canada, you can reduce your taxes — and often receive a cash
refund — based on what you spent on that work (mostly salaries).

The catch is that **not all engineering is R&D in the CRA's eyes**, and to claim you must
*describe the work* in a specific way and *back it with records*. This is where most
software teams lose money: the work qualified, but nobody wrote down the right things at
the right time.

### The two tests your work must pass

1. **The "why" — was there genuine technological uncertainty?**
   At the start, was it actually *unknown* whether you could achieve the result, or *how*,
   using standard practice and publicly available knowledge? "Hard" or "time-consuming" is
   not enough. There has to be a real gap in what the technology field already knows.

2. **The "how" — did you investigate systematically?**
   Did you (1) define the problem, (2) form a hypothesis, (3) test it by experiment or
   analysis, and (4) draw conclusions from the results? A methodical *process* alone
   doesn't count — the CRA wants to see hypotheses tested by evidence.

### Three things beginners are always surprised by

- **You don't have to succeed.** If you tried an approach and proved it *doesn't* work,
  that's still new knowledge and can still qualify.
- **It's about the technology, not your business.** "It made us money" or "customers loved
  it" is irrelevant to eligibility. "It was unknown whether X was technically achievable,
  and we found out" is what matters.
- **Most of your codebase doesn't qualify.** UI, CRUD, dashboards, integrations with
  documented APIs, dependency bumps, refactors, routine bug-fixes — almost always *not*
  SR&ED. The eligible work is usually a small, genuinely hard slice.

### What the claim itself looks like

When you file, you fill out **Form T661**, and for **each project** you answer three
questions (each with a strict word limit):

- **Line 242 — What technological uncertainties did you try to overcome?** (≤ 350 words)
- **Line 244 — What work did you do to overcome them?** (≤ 700 words)
- **Line 246 — What advancements did you achieve or attempt?** (≤ 350 words)

You also need **time records** showing how many hours each person spent on the eligible
work, and **supporting evidence** that the work happened when you say it did. The CRA
strongly prefers *contemporaneous* documentation — records created *as the work happened*,
not reconstructed a year later from memory.

**This skill exists to capture all of that, every week, while it's fresh — and to keep it
honest enough to survive a review.**

---

## 2. What this skill does — and what it deliberately does *not* do

### It does

- **Eligibility triage** — pressure-tests whether work is real SR&ED or routine
  development, and tells you when *not* to claim something.
- **Technical narratives** — drafts the line-242/244/246 write-ups, structured exactly the
  way the T661 wants them, within the word limits.
- **Time tracking** — records hours by developer, by project, split from non-eligible
  work, with daily resolution.
- **Evidence trail** — links every claim to the commits, pull requests, tests, and
  benchmarks in your repo that prove it.
- **Manager dashboard** — generates a browsable HTML page where managers and consultants
  can see the claim's status, open issues, and time breakdowns at a glance.

### It does *not*

- Calculate any **dollar amounts**, expenditures, or the credit/ITC.
- Fill out or file **Form T661** (or T2SCH31 / T2038(IND)).
- Give **tax advice**.

Those belong to your management and your accountant or SR&ED consultant. This skill feeds
them clean, accurate inputs — hours and narratives — and stops there. Think of it as the
*engineering half* of the claim, done properly.

---

## 3. Key concepts and glossary

- **Project** — a body of related work pursuing **one technological advancement** by
  resolving **one shared uncertainty**. *Not* your repo, *not* a feature, *not* a sprint.
  Usually a small slice of your code. You declare projects; the skill suggests candidates.
  **One codebase normally contains several SR&ED projects** — see the callout in §5. Each
  is independent and gets its own T661 answers.
- **Technological uncertainty** — a genuine gap in what the technology field knows: it was
  unknown whether/how a result could be achieved with standard practice.
- **Advancement** — the new technological knowledge you gained (success *or* a proven dead
  end).
- **Systematic investigation** — the define → hypothesize → test → conclude cycle.
- **Support work** — programming, testing, design, data collection, etc., eligible only
  when proportional to the experimental work it supports.
- **Journal** — the *raw*, contemporaneous, week-by-week log per project. Your evidence.
- **Narrative** — the polished, T661-shaped write-up, synthesized from the journal when
  you're preparing to claim. Your deliverable.
- **Provenance tag** — how trustworthy a statement is: `evidence-backed` (proven in the
  repo), `attested` (you said so, plausibly), or `unverified` (asserted but unsupported —
  flagged for follow-up).
- **Cursor** — a bookmark (a git commit + date) recording how far the skill has already
  documented, so it never re-asks about old work. Stored in `state.json`.
- **Dashboard** — a generated HTML page at `.sred/html/index.html` that gives managers a
  browsable, visual overview of the claim: project status, word-count meters, open items,
  and time breakdowns. Regenerated automatically; never edited by hand.
- **Contemporaneous** — created at the time the work happened. The gold standard for
  evidence, and why weekly capture matters.

---

## 4. Installing and setting up

### What you need

- **Claude Code** (recommended) or another Claude environment that supports skills and
  runs with your codebase as context (e.g., the Claude desktop app with your project folder
  open). Claude Code is a command-line tool — install it from
  [https://docs.claude.com](https://docs.claude.com) if you don't have it yet.
- A **git** repository. The skill relies on git history for evidence and continuity. If your
  code isn't in git, you'll need to set that up first.
- Optional: the **`gh` CLI** (GitHub's command-line tool), if you want pull-request details
  included in the evidence trail. Without it, the skill uses git-only and still works fine.

### How to install

Copy the `sred-documentation/` folder into your project's skills directory. For Claude
Code, that's typically:

```
your-repo/
└── .claude/
    └── skills/
        └── sred-documentation/   ← this folder
            ├── SKILL.md
            ├── README.md
            └── references/
```

That's it. There's nothing to compile, no dependencies to install, and no configuration
files to write. The skill activates automatically when you talk to Claude about SR&ED —
just mention SR&ED, R&D tax credits, time tracking for a claim, the `.sred` folder, or
anything similar.

### First use

Open your repo in Claude Code (or your Claude environment) and say something like:

> "Set up SR&ED documentation for this project."

The skill detects there's no `.sred/` folder yet and walks you through onboarding — one
question at a time, each with a recommended answer you can accept or change (see
[Walkthrough 1](#7-walkthrough-1-first-run-onboarding) for exactly what that looks like).

After setup, your entire weekly routine is:

> "Log my SR&ED work for this week."

That's it. The skill handles the rest: reading your commits, interviewing you, writing the
documentation, and regenerating the dashboard.

---

## 5. The `.sred/` folder, explained

Everything lives in one `.sred/` folder at the **root of your (mono)repo**, covering all
subsystems — web, scraper, infra, admin, utils, and so on.

```
.sred/
├── config.yml                     # your settings: fiscal year-end, work week, excludes, language
├── state.json                     # the cursor: how far documentation has progressed
├── README.md                      # a short in-repo explainer (generated)
├── projects/                      # one folder per SR&ED PROJECT — usually SEVERAL
│   ├── anti-bot-evasion/          # SR&ED project (a slug you choose)
│   │   ├── project.md             # its definition (uncertainty theme, paths it spans)
│   │   ├── journal.md             # RAW weekly log — your contemporaneous evidence
│   │   ├── narrative.md           # its OWN T661 answers (242/244/246, word-capped)
│   │   └── evidence.md            # links: claim → commit / PR / test
│   ├── agentic-problem-solving/   # another SR&ED project, fully independent
│   │   └── …
│   └── iframe-site-previews/      # another SR&ED project
│       └── …
├── timesheets/
│   └── FY2025-2026/
│       └── 2026-W05/
│           ├── alice.md           # one file PER DEVELOPER per week (no merge conflicts)
│           └── bob.md
├── html/
│   └── index.html                 # generated manager dashboard (open in any browser)
└── exports/                       # generated CSV + narrative bundle for management
```

**Commit `.sred/` to your repo.** Two reasons: your teammates share it, and its git
history becomes dated proof of *when* you documented the work — exactly the
contemporaneous evidence the CRA values.

Three ideas make this layout work:

- **One repo, many SR&ED projects.** Your company codebase (say *MyCoolProject*) is *not*
  an SR&ED project — it's just where `.sred/` lives. Inside it you'll usually have several
  distinct SR&ED projects, each defined by its own technological uncertainty: e.g.
  *bypassing anti-bot detection*, *novel uses of AI agents to solve a problem where no
  known method works*, and *running full-site previews in iframes without breaking
  functionality*. Each gets its own folder, its own journal, and its own set of three T661
  answers — they're filed separately. An SR&ED project is defined by its **uncertainty, not
  by a folder of code**: two projects can share files, and one feature can split across two
  projects (or be partly routine). The `paths` you give each project just tell the skill
  where to look; they don't have to be exclusive.
- **Journal vs narrative (two layers).** Each week you add a few honest lines to the
  *journal*. You don't write polished prose every week. Later — usually once, before
  filing — the skill synthesizes those journal entries into the *narrative* that goes on
  the form. The journal is the evidence; the narrative is the product.
- **Per-developer timesheet files.** Each developer logs into their own file for the week,
  so two people working the same week never edit the same file and git never conflicts.

---

## 6. The three modes

You don't pick modes by name; the skill infers from what you ask. But it's useful to know
they exist.

| Mode | You say something like… | What happens |
|------|--------------------------|--------------|
| **Capture** (default) | "Log my SR&ED work this week." | Reads new commits since last time, interviews you about the eligible work, drafts your time, writes the journal + evidence, and regenerates the manager dashboard. |
| **Status / gaps** | "How far behind am I on SR&ED docs?" | Read-only. Tells you how stale things are and how many commits are unreviewed. Changes nothing — does *not* regenerate the dashboard. |
| **Review / audit** | "Stress-test our SR&ED claim before we file." | Turns into a skeptical CRA reviewer, synthesizes the journals into T661 narratives, enforces word limits, flags weak or unsupported claims, and regenerates the dashboard. |

A good rhythm: **capture weekly**, glance at **status** if you've been away, and run a
**review** before management compiles the claim each year.

---

## 7. Walkthrough 1: first run (onboarding)

A condensed example of the conversation the first time you run it (one question at a time,
each with a recommendation):

> **You:** Set up SR&ED documentation for this repo.
>
> **Skill:** There's no `.sred/` folder yet, so I'll set one up at the repo root. First I
> need your **fiscal year-end** — I can't infer it from the code. Most small/medium
> Canadian companies use a calendar year-end (Dec 31). What's yours?
>
> **You:** March 31.
>
> **Skill:** Got it — FY ends 03-31. Next, your **standard work week** in hours, used only
> to sanity-check time entries (so we never log 60 SR&ED hours in a 40-hour week). I'd
> recommend 40. Keep it?
>
> **You:** Yes.
>
> **Skill:** Capture period — I recommend **weekly** (fresh enough to be accurate, light
> enough to keep up). Or align to 2-week sprints. Weekly?
>
> **You:** Weekly.
>
> **Skill:** I'll exclude routine subsystems from project discovery so we're not wading
> through noise: `admin/**`, `utils/**`, lockfiles, generated and vendored code. Anything
> to add or remove?
>
> **You:** Add `web/components/**`, it's mostly UI.
>
> **Skill:** Done. Now let me look for **candidate projects**. Scanning branches, churn,
> and commit messages… I found two areas that look like they might involve real
> uncertainty:
>
> 1. **`scraper/`** — branch `spike/anti-bot`, commits like "captcha approach #3 failed",
>    "rotating fingerprint experiment". Looks like genuine experimentation.
> 2. **`infra/queue/`** — "rewrote dedup, exactly-once still racy", benchmark files.
>
> I'd nominate both as candidate projects, and *not* nominate the `web/` and `admin/` work
> (looks routine). Want me to create project folders for these two? You can rename them.

…and so on. Nothing becomes a project without your yes. Once you've confirmed, the skill
creates the project folders, writes the initial config and state files, generates the
manager dashboard at `html/index.html`, and asks you to commit `.sred/`.

---

## 8. Walkthrough 2: a weekly capture session

> **You:** Log my SR&ED work for this week.
>
> **Skill:** You're `alice@…`. Since the last capture (commit `9f3a…`, 2026-02-09) there
> are 41 new commits. Filtering out excluded paths, 12 touch your SR&ED projects — 9 in
> **resilient-scraping**, 3 in **exactly-once-queue**. Let's start with scraping.
>
> I can see commits `a1b2` "try TLS fingerprint randomization", `c3d4` "still detected,
> trying header order", and a failing test `test_evasion::rotates_ja3`. So I won't ask
> *what* you did — I can see it. What I can't see is the *why*: at the start of this work,
> was it genuinely unknown whether you could evade detection, or were you applying a known
> technique?
>
> **You:** Known techniques (proxies, user-agent rotation) stopped working against the new
> bot-detection vendor. There's no public method for defeating JA3 + behavioural
> fingerprinting together at our request volume.
>
> **Skill:** Good — that's a real uncertainty, and it reframes nicely for line 242: *"It
> was unknown whether request-level fingerprint evasion could be sustained against combined
> JA3 and behavioural detection at N req/min, as standard rotation techniques no longer
> succeeded."* Accurate?
>
> **You:** Yes.
>
> **Skill:** Your hypothesis for line 244 — what did you try first, and what did the data
> say? I see three approaches in the commits; walk me through the first that failed.
>
> *(…continues per the grilling script…)*
>
> **Skill:** For time: the repo shows you on scraping Mon/Tue/Thu and the queue Wed.
> Roughly how many hours each day went to this SR&ED work? Your standard week is 40h.
>
> **You:** Mon 6, Tue 5, Thu 7 on scraping. Wed 6 on the queue.
>
> **Skill:** That's 24 SR&ED hours of ~40 — the rest routine? I'll log it. The Thu block is
> mostly the evasion experiments (experimental development) and ~2h of writing tests
> (support: testing). Sound right? … Logged. I've updated both journals, added the failing
> JA3 test and three commits to the evidence index, written `timesheets/FY2025-2026/
> 2026-W07/alice.md`, advanced the cursor, and regenerated the manager dashboard. Please
> commit `.sred/`.

---

## 9. Walkthrough 3: review/audit before filing

> **You:** We're filing soon — stress-test the scraping project as if you were the CRA.
>
> **Skill:** Reviewing `resilient-scraping`. Synthesizing the journal into the T661
> narrative now. Line 242 is 280/350 words — good. Line 244 is **760/700** — over the cap;
> I'll tighten the third iteration's description without losing the hypothesis→result arc.
>
> Two concerns a reviewer would raise:
> - The journal claims a "40% reduction in detection rate" but I find no benchmark file or
>   commit for it. I've flagged it **`[unverified]`** — either point me to the evidence or
>   we soften the claim.
> - The week of 2026-W03 logs 8 SR&ED hours but no commits touch the project's paths that
>   week. Flagged for your explanation.
>
> Line 246 looks solid and is genuinely about new knowledge (which evasion combination held
> and which failed). Once you resolve the two flags, I'll set the narrative status to
> `dev-reviewed`, regenerate the dashboard, and you should commit. I will **not** mark it
> accepted — that's management's call.

---

## 10. A worked software example (eligible vs routine)

This is the single most useful thing to internalize: **the same work is described two
ways, and only one of them is a claim.**

### ❌ How developers naturally describe it (not claimable)

> "We built a new caching layer to make the dashboard faster. It was a lot of work and we
> had to refactor a bunch of stuff, but now load times are way better and customers are
> happy."

Nothing here is SR&ED: it's a deliverable and a business outcome. There's no stated
uncertainty and no experiment.

### ✅ The same work, if real uncertainty existed (possibly claimable)

> **Uncertainty (242):** "It was unknown whether we could maintain read-after-write
> consistency for per-tenant dashboards under bursty concurrent writes without a global
> lock. Standard cache-invalidation patterns (TTL, write-through) either served stale data
> or serialized writes past ~2,000 writes/sec in our tests; no published approach addressed
> our multi-tenant access pattern."
>
> **Work (244):** "We hypothesized that a per-tenant CRDT-based cache could converge
> without locking. We prototyped three variants — last-write-wins, a G-Counter approach,
> and a hybrid — and load-tested each against a 5,000 writes/sec synthetic workload,
> measuring stale-read rate and convergence time. LWW and the G-Counter both produced
> >1% stale reads; the hybrid held to 0.02% up to ~4,000 writes/sec, degrading past that."
>
> **Advancement (246):** "We established that a hybrid CRDT cache can preserve
> read-after-write consistency for this multi-tenant pattern up to ~4,000 writes/sec
> without a global lock, and that simpler CRDT structures cannot. We also identified the
> degradation point as a direction for further work."

The difference is entirely in **whether a genuine technological uncertainty existed and was
resolved by experiment** — and in writing it down that way. The skill's job is to find out
which case you're in (and tell you honestly when it's the first one).

---

## 11. How time tracking works

- **Discrete daily entries**, per developer, per project, recorded in **SR&ED hours only**
  (the eligible portion of your time).
- **The skill drafts entries from your git activity**; you just confirm or correct. The
  goal is review, not bookkeeping.
- **Reconciliation:** the skill knows your standard week and won't let totals exceed it
  without you flagging an exception (overtime, on-call). This stops impossible-looking logs
  before they're written.
- **Activity category:** each entry is tagged as core *experimental development* or
  *support* (programming, testing, data collection, …), because support work is only
  eligible when it's proportional to the experimental work.
- **Self-report is the source of truth.** Git is only a memory aid and a fact-checker — the
  skill never invents hours from commit timestamps (commit time isn't hours worked).
- **One file per developer per period**, so the team never collides in git.

At hand-off, the skill rolls all developers' files up into a single CSV your accountant can
drop into a spreadsheet.

---

## 12. How evidence and provenance work

The skill stores **references, not copies**: a commit SHA, a PR number, a `file:line`, a
test name. Your repo is the durable store, and SHAs are permanent and tamper-evident, so
there's no need to duplicate code into `.sred/`. The one exception: if a piece of evidence
*isn't* under version control (a benchmark result, a design doc pasted from a wiki), the
skill snapshots it, because a reference to something deletable isn't evidence.

Every claim and time entry carries a **provenance tag**:

- **`evidence-backed`** — tied to something in the repo (a commit, test, or measurement).
- **`attested`** — you stated it and it's plausible, but it can't be independently verified
  (hours, intent, your reasoning about *why* something was uncertain).
- **`unverified`** — asserted but unsupported, or you weren't sure. These are surfaced
  prominently so management can chase them down before filing.

This is how the skill can be helpful *and* honest: it never pretends an unproven claim is
proven.

---

## 13. Handing off to management

When it's time to give the claim to management or your accountant/SR&ED consultant, ask:

> "Prepare the SR&ED hand-off for FY2025-2026."

The skill generates an `exports/` bundle:

- **A consolidated time CSV** — developer, date, project, SR&ED hours, activity category,
  provenance — ready for a spreadsheet.
- **A narrative bundle** — the per-project line-242/244/246 write-ups.
- **An open-items list** — every `[unverified]` claim and any time entries that need
  explanation, so nothing weak slips into the filing unflagged.

It also regenerates the **manager dashboard** (`html/index.html`) so it reflects the final
state. You can share the dashboard file directly — it's a single self-contained HTML file
that opens in any browser, even offline.

Management and the accountant then do the dollars and the actual filing. That division is
intentional and keeps each side doing what it's good at.

---

## 14. The manager dashboard

The skill generates a **self-contained HTML dashboard** at `.sred/html/index.html` — a
visual, browsable overview of the entire claim designed for managers, accountants, and
SR&ED consultants who don't want to read Markdown files or navigate folder trees. Open it
in any browser; it works offline and can be emailed as a single file attachment.

### What's on it

- **Overview bar** — total SR&ED projects, hours, developers, and open items at a glance.
- **Needs-attention panel** — every flag that needs resolution before filing, color-coded
  by severity: unverified claims, eligibility doubts, thin narratives, word-limit
  overflows, time/evidence mismatches, backfill warnings, and stale-capture alerts. This
  is usually where a manager looks first.
- **Project cards** (expandable) — each SR&ED project with its status badge (`active` /
  `dormant` / `closed`), narrative badge (`draft` / `dev-reviewed` / `accepted`), T661
  word-count meters (lines 242/244/246 with color-coded fill bars), narrative excerpts
  with provenance tags, and recent journal entries.
- **Considered-and-excluded strip** — routine work the eligibility gate did *not* claim,
  with the reason. This is valuable for auditors: it shows the claim was selective, not
  blanket.
- **Time section** — hours-by-project and hours-by-developer bar charts, a detailed time
  table with provenance tags, and a three-tier provenance legend.

### When it regenerates

The dashboard is **regenerated automatically** at the end of every capture and review/audit
session, and on first run (onboarding). It is also regenerated when you prepare a hand-off.
It is **not** regenerated after status/gaps mode, since that's read-only and changes
nothing. You can also regenerate it on demand:

> "Refresh the SR&ED dashboard."

### What it is not

The dashboard is a **generated view** — like the CSV export, it's derived from the
canonical Markdown files and regenerated fresh each time. It is never edited by hand, and
it carries a header and footer saying so. If a manager spots something wrong, the fix
happens in the source (journal, narrative, timesheet) via another capture or review
session, and the dashboard reflects it on the next regeneration.

---

## 15. How it stays honest

Accuracy is the whole point, so the skill is built to resist the failure modes that wreck
SR&ED claims — over-claiming and fabrication:

- **It never fabricates.** Gaps are flagged `[unverified]`, never filled with
  plausible-sounding filler.
- **It will talk you out of claiming routine work.** A tool that only ever helps you claim
  *more* is an audit liability.
- **It won't pad.** If a project lacks enough real material for a defensible line 242, it
  says so instead of inflating it to 350 words. (The CRA requires claim content to be
  "accurate, true, original, and not copied"; padding and plagiarism can void a claim.)
- **Drafts stay drafts.** Narratives move from `draft` to `dev-reviewed` only when you
  confirm, and the skill never marks anything `accepted` — that's management's call.
- **Git is the audit trail.** Corrections are new dated commits; the skill never rewrites
  history to alter past entries.

---

## 16. FAQ

**Do I need to understand SR&ED to use this?**
No. The skill coaches you, and this README is your primer. It will flag eligibility issues
for you.

**We failed — the approach didn't work. Can we still claim it?**
Often yes. Proving an approach *doesn't* work is valid new knowledge. Capture what you
hypothesized, what you tested, and what you learned.

**Is using a new framework/library SR&ED?**
Almost never. Learning or applying existing tools and techniques is explicitly excluded.
Eligibility comes from genuine uncertainty you had to *experiment* to resolve.

**I forgot to log for a month. What now?**
Run capture; the skill diffs everything since the cursor and walks you through it. For work
that predates the skill entirely, ask for a **backfill** — but those reconstructed entries
are capped at `attested` provenance and flagged, since they aren't contemporaneous. The
lesson the CRA teaches is to log weekly.

**We have several developers. How does that work?**
Each developer runs capture for their own work and logs into their own timesheet file. The
shared `.sred/` folder lives in the repo; per-developer files avoid merge conflicts.
Narratives are shared per project and synthesized from everyone's journal entries.

**I'm the only developer. Does this still work?**
Yes. Everything works the same — you just won't have the multi-developer file sharding.
You'll have one timesheet file per week, and the journal entries will all be yours.

**It's a monorepo with tons of routine code. Won't it drown in noise?**
That's exactly what the `exclude` list in `config.yml` and per-project path globs are for.
Discovery scans broadly minus the noise; documenting a known project is scoped to its
paths.

**Can it write in French?**
Yes — set `language: fr` in `config.yml`. (Form T661 accepts either official language.)

**Does this file my taxes or tell me my refund?**
No. It produces hours and narratives only. Dollars, the credit calculation, and filing
Form T661 (and T2SCH31 or T2038(IND)) are handled by management and your accountant.

**How often should I run it?**
Weekly. Frequent enough to stay accurate, light enough to keep up. Contemporaneous records
are the strongest evidence.

**What if I'm not sure whether something qualifies?**
Capture it honestly and let the eligibility gate and a later review decide. The skill is
comfortable concluding "this is routine — don't claim it," which is a feature.

**How do managers see the claim status?**
Open `.sred/html/index.html` in any browser. It's a self-contained dashboard showing every
project, its narrative word counts, open flags, and time breakdowns. It regenerates
automatically after each capture and review session. You can also email the file directly —
no server or special software needed.

**Can I regenerate the dashboard manually?**
Yes — just ask: "Refresh the SR&ED dashboard."

**Do I need to be online for the dashboard to work?**
No. The file is fully self-contained. Fonts fall back to system defaults if offline. The
only thing that needs a network connection is the Geist font from Google Fonts, which is
purely cosmetic.

**What does the "considered and excluded" section mean?**
It lists work the eligibility gate evaluated and determined was *routine* engineering — not
SR&ED. Showing what you *didn't* claim is valuable: it demonstrates the claim was selective
and defensible, not a blanket grab for everything the team did.

**What if I have no idea where to start?**
Just open your repo with Claude and say: "Set up SR&ED documentation for this project." The
skill walks you through everything step by step, one question at a time, and explains its
recommendations as it goes. You don't need to prepare anything.

**What's the difference between a "journal" and a "narrative"?**
The *journal* is the raw, week-by-week log you build during capture sessions — it's your
contemporaneous evidence. The *narrative* is the polished, T661-shaped write-up synthesized
from the journal during a review/audit session — it's the deliverable that goes on the
form. Think: journal = field notes, narrative = the paper you write from them.

---

## 17. Disclaimer and sources

**This skill and its output are not tax advice.** It helps you organize technical and time
documentation. Whether your work qualifies for SR&ED, how much you can claim, and how to
file are determinations for your management and a qualified accountant or SR&ED
professional. Always verify current rules with the CRA.

Sources informing this skill (retrieved June 2026):

- CRA — SR&ED tax incentives program:
  https://www.canada.ca/en/revenue-agency/services/scientific-research-experimental-development-tax-incentive-program.html
- CRA — What work is eligible:
  https://www.canada.ca/en/revenue-agency/services/scientific-research-experimental-development-tax-incentive-program/sred-eligibility.html
- CRA — How to write your project description for Form T661 (lines 242/244/246, word
  limits, broken-down questions, accuracy/originality rule):
  https://www.canada.ca/en/revenue-agency/services/scientific-research-experimental-development-tax-incentive-program/sred-claim/group-projects/project-description-t661.html
- Interaction pattern inspired by the "grilling" skill by Matt Pocock:
  https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md
