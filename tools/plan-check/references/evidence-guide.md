# Evidence guide: where evidence lives in a plan package

An eval package is one markdown file with these sections, in order:
Repo facts, Issue, Thread highlights (each comment tagged with the
author's role in parentheses), Repro evidence, Candidate plan,
Candidate plan comment. Plan headings vary by author; read the whole
Candidate plan section, not only the heading a check's name suggests.

## Diagnosis and grounding

- Where it lives (eval): the cause is in the Candidate plan under
  Diagnosis, Cause, or Summary. The behavior it must explain is in the
  Repro evidence block: numbered steps, timings, artifacts (stack
  traces, outputs), "Control" or "Control runs" lines, and the
  Expected/Actual lines at the end.
- Where it lives (live): the cause in the student's plan.md; the
  behavior in the student's posted unit-2 repro comment on the issue
  (or the house repro pack quoted in the drafts).
- What good looks like: the stated cause explains every repro result,
  and especially the control runs. A control is the strongest
  evidence in the package: if a control shows the bug without the
  blamed component (or the blamed component working fine), the cause
  is wrong no matter how confident the plan or a thread commenter is.
  Example of wrong: the plan blames the pager's key bindings, but the
  repro shows the same 26 s with no pager at all and an instant seek
  with highlighting off.

## Scope

- Where it lives (eval): the plan's Scope / In scope / Not in scope
  lines, and every numbered item under Changes, Proposed changes,
  Approach, and Files and areas. Scope creep usually hides in the
  numbered list ("while touching...", "also migrate...").
- Where it lives (live): the same sections of plan.md.
- What good looks like: every listed change serves the reported bug
  (the fix, its tests, a doc line about the fixed behavior, a safety
  gate). A not-in-scope line names what is deliberately left alone.
  Deferring part of the issue with a stated reason is bounded. A
  migration, upgrade, new setting or option, rename, restructure, CI
  matrix, or retry framework the issue never asked for is creep, even
  bundled with a correct core fix.

## Executability

- Where it lives (eval): the plan's Files, Change(s), and Approach
  sections.
- Where it lives (live): the same sections of plan.md.
- What good looks like: named files or code areas plus one chosen
  change, so a stranger could open the file and start. Exact function
  names may be pinned later if the area is named. Bad: "investigate",
  "profile and optimize", "somewhere", "gocui? tcell? not sure",
  "upstream or vendored, whichever is easier".

## Test plan

- Where it lives (eval): the plan's Test or Test plan section; compare
  with the Repro evidence steps and its Expected line.
- Where it lives (live): plan.md's test plan vs. the posted repro
  steps.
- What good looks like: the repro steps re-run with a specific
  expected after-fix observable (exit 0, a printed match, the color
  flips at step 3, 3 pages not 4, under 0.3 s), or a named regression
  test asserting the repro case. Bad: "run the full test suite",
  "nothing regresses", "should feel fast", "nothing else broken".

## Honesty

- Where it lives (eval): the plan's Risk / Risks / unknowns lines,
  hedged sentences ("I have not yet measured..."), stated deferrals.
- Where it lives (live): plan.md's risks section and its
  `## Deviations` section after the build.
- What good looks like: real unknowns named as unknowns, with what
  the author will do about them. False confidence is certainty the
  evidence does not support (a cause "confirmed" that the repro
  contradicts). A mid-build deviation is recorded under Deviations in
  plan.md and, if the posted plan is no longer true, in a follow-up
  issue comment.

## Comms

- Where it lives (eval): the Candidate plan comment, read against
  (a) the Thread highlights, maintainer comments only (OWNER, MEMBER,
  COLLABORATOR, CONTRIBUTOR), and (b) the Repo facts block's
  "contribution policy" line, which includes any AI policy.
- Where it lives (live): the draft comment.md against the live issue
  thread and the repo's CONTRIBUTING / AI policy files.
- What good looks like: if a maintainer gave direction (isolated the
  culprit, proposed or rejected an approach, posted a patch, asked
  for testing), the plan follows it or the comment says why not. If
  the policy requires disclosing all AI use, or disclosure on issues
  or comments, the comment contains an explicit AI-use disclosure
  (treat every package as AI-assisted). If the policy requires
  comments in the contributor's own words, the comment does not read
  as boilerplate. Policies asking for disclosure only in pull
  requests place no disclosure requirement on the plan comment.
  Engaging open PRs or prior art, rather than racing them, is good
  practice.
