# SR&ED eligibility and Form T661 — reference

Distilled from the Canada Revenue Agency (CRA) SR&ED program pages (program, "What work
is eligible", "How to prepare your claim", and "How to write your project description for
Form T661"), retrieved June 2026. Authoritative source:
https://www.canada.ca/en/revenue-agency/services/scientific-research-experimental-development-tax-incentive-program.html
Verify specifics against the CRA before filing — rules and limits can change.

## Contents
1. The two eligibility requirements
2. Technological uncertainty vs a business goal
3. Advancement (and why failure still counts)
4. The four steps of a systematic investigation
5. Eligible work types (including support work)
6. Work that is NOT eligible
7. Software-specific guidance and common pitfalls
8. Form T661 — the three narrative questions and word limits
9. The CRA's broken-down sub-questions
10. Accuracy and originality rule

---

## 1. The two eligibility requirements

For work to be eligible it must be conducted in Canada and meet **both**:

- **The "why":** the work is done to achieve a **technological advancement** (or to advance
  scientific knowledge) — i.e., to generate *new knowledge* because it was *uncertain*
  whether a result could be achieved given the existing knowledge base.
- **The "how":** the work is a **systematic investigation or search** carried out by means
  of **experiment or analysis**.

Both must be present. Routine development that uses existing knowledge — however hard,
valuable, or time-consuming — is not SR&ED if there was no genuine uncertainty resolved
through systematic investigation.

## 2. Technological uncertainty vs a business goal

The uncertainty must be a **gap in the technology knowledge base**, not a business want.

- Business goal (not sufficient on its own): "We needed the page to load faster."
- Technological uncertainty (the eligible framing): "It was unknown whether sub-10 ms
  median latency was achievable for this query shape at our data volume, because standard
  indexing and caching approaches degraded past ~2M rows and the literature offered no
  applicable method for our access pattern."

The test: at the start, could a competent professional in the field have known the result
or the route to it using **standard practice and publicly available knowledge**? If yes,
there is no uncertainty. If genuinely no, you may have SR&ED.

## 3. Advancement (and why failure still counts)

An advancement is **new knowledge that moves the understanding of technology forward** —
not new knowledge for your business, but for the field's technology base. Crucially, **you
do not have to succeed.** Learning that an approach *does not* work is valid new knowledge.
What matters is the knowledge gained, not whether the product shipped or the business
benefited.

## 4. The four steps of a systematic investigation

A systematic investigation **must include** all four:

1. **Define a problem** (the technological obstacle).
2. **Advance a hypothesis** aimed at resolving it.
3. **Plan and test** the hypothesis by experiment or analysis.
4. **Develop logical conclusions** based on the results.

Having "a methodical process" or following best practices is *not* enough. The hypothesis →
test → conclusion arc is what the CRA looks for, and it is exactly what the line-244
narrative must show.

## 5. Eligible work types

- **Basic research** — advancing scientific knowledge without a specific application.
- **Applied research** — advancing scientific knowledge with an application in view.
- **Experimental development** — generating/discovering technological knowledge to create
  or improve materials, devices, products, or processes, **including incremental
  improvements**. (Most software SR&ED is here.)
- **Support work** — eligible only if it **directly supports** and is **commensurate with**
  (proportional to) the needs of the above. Eligible support categories: engineering,
  design, operations research, mathematical analysis, **computer programming**, **data
  collection**, **testing**, and psychological research.

Tag programming/testing/data-collection time as support work, because its eligibility hinges
on being proportional to the experimental-development need — that proportionality is what an
auditor probes.

## 6. Work that is NOT eligible

Excluded by definition — never claim:
- Market research or sales promotion
- Quality control or **routine testing**
- Research in the social sciences or humanities
- Prospecting/exploring/drilling for or producing minerals, petroleum, natural gas
- **Commercial production** of a new/improved product, or commercial use of a new process
- **Style changes** (for software, read: UI/visual restyling)
- **Routine data collection**

Also not eligible (acquiring/applying existing know-how):
- Training and on-the-job learning
- Hiring experts to apply what they already know
- Purchasing proprietary knowledge

## 7. Software-specific guidance and common pitfalls

Software claims draw scrutiny. Use these to keep the eligibility gate honest.

**Usually NOT eligible (routine):**
- Building features with established frameworks, libraries, and patterns
- CRUD, forms, dashboards, admin panels, integrations using documented APIs
- UI/UX styling and redesigns (style changes)
- Configuration, dependency upgrades, refactoring for cleanliness
- Routine debugging and bug fixing
- Performance tuning that standard profiling/known techniques resolve
- Learning a new language/framework

**Possibly eligible (look closer):**
- A required capability where no known method works at your constraints (scale, latency,
  concurrency, accuracy, hardware limits) and the outcome was genuinely uncertain
- Novel algorithms where existing ones provably fail your requirements
- Integrating systems in ways with unpredictable behavior that needed experimentation
- Achieving a performance/accuracy target that standard practice could not, requiring
  hypotheses tested by measurement

**The reframing move:** capture *what was uncertain at the technology level* and *what you
tried and measured*, not *what feature you delivered*. "We built a caching layer" is a
deliverable. "It was unknown whether our consistency guarantee could be preserved under
X concurrency without a global lock; we hypothesized a CRDT-based approach, prototyped
three variants, measured conflict rates, and found two failed and one held to N writers"
is a (possible) SR&ED narrative.

When work is routine, **say so** and recommend not claiming it.

## 8. Form T661 — the three narrative questions and word limits

On Form T661, for **each project**, you describe the work by answering three questions
(referenced by their line numbers), each with a hard word cap:

- **Line 242 — What scientific or technological uncertainties did you attempt to
  overcome?** (maximum **350 words**)
- **Line 244 — What work did you perform in the tax year to overcome the uncertainties
  described in line 242?** (maximum **700 words**)
- **Line 246 — What scientific or technological advancements did you achieve or attempt to
  achieve as a result of the work described in line 244?** (maximum **350 words**)

`narrative.md` mirrors exactly these three sections and enforces these caps.

(Filing context, out of scope for this skill: the ITC is also claimed on Form T2SCH31 for
corporations or T2038(IND) for individuals. The SR&ED reporting deadline is generally
18 months after the fiscal year-end — confirm with the CRA/your advisor.)

## 9. The CRA's broken-down sub-questions

The CRA decomposes each line into simpler prompts. Use these as the spine of both the
interview and the narrative sections.

**Line 242 (uncertainties):**
- Describe your business and your regular commercial activities.
- What was the objective of the work you want to claim?
- Explain why the available solutions/knowledge did not meet your needs.

**Line 244 (work performed):**
- Describe the hypothesis, idea, or potential solution you started from.
- Describe the work you did to test it / overcome the limitations.
- What data or feedback did you collect?

**Line 246 (advancement):**
- Describe the technological advancement (the new knowledge) you achieved — including what
  you learned from approaches that did not work.

## 10. Accuracy and originality rule

The CRA states that information on Form T661 must be **accurate, true, original, and not
copied from other sources**, and that false/inaccurate claims, including plagiarism, may
result in disallowance, penalties, or other enforcement. Therefore:
- Synthesize narratives only from *this project's* journal and evidence.
- Never copy the CRA's published examples or generic boilerplate.
- Never pad a thin project to hit a word count.
