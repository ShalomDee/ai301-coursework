# Procedure: how this tool grades a PR package

These are steps for grading a pull request someone else wrote, not for
writing one. Do every step in order and write down what each step asks
you to note before moving on; later steps compare those notes side by
side. Each stage below has the same steps in both modes. Where a live
input differs from the bundle, the step says so (the evidence guide
has the full map).

## Read order

The plan comes first and the description comes last. That way the
plan sets the yardstick, and the diff is read against the plan before
any confident description sentence can frame it. Calib-03's "no
changes beyond the plan" description over a diff that rewrites an
unplanned file is the case this order exists for.

1. **Repo facts first.** Copy the "pull requests" line and the
   "contribution policy" line verbatim into your notes (live: read
   `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the
   house rules in `scope.md`). Under them, list every ask a PR's own
   text or diff can satisfy, one per line: required sections, issue
   reference form, checklist items, changelog/whatsnew entry, title
   convention, tests, named checks, AI disclosure. Mark process-only
   asks (CLA, vouch flow, assignment, post-open CI) as "not graded".
2. **Issue and thread.** Note in one line what the issue asks to be
   fixed. Note any maintainer direction (OWNER, MEMBER, COLLABORATOR,
   CONTRIBUTOR) that the plan says it follows.
3. **The plan.** Note, as separate lists:
   - (a) Deliverables: every change the plan commits to, one per
     line (each numbered step, each file in Files, each "and" item
     such as "a warning AND a docs update").
   - (b) Boundary: the not-in-scope line, verbatim.
   - (c) Notes: every deviation, deferral, or "known limit" note,
     verbatim.
   - (d) Test plan: every repro case or failure mode it will re-run,
     each expected-after it states, each control, and each suite or
     check it names.
   - (e) Repro cases: from the repro evidence, each failing command
     or set of steps and its observed failure. Only these are repro
     cases; a control, or a test-plan item that checks new behavior
     the fix adds (a warning, an error message), is not.
4. **The diff.** List every changed file. Under each, list each hunk
   as one line: what it does, in your words, with its first changed
   line quoted. Do not read the description yet.
5. **The commit list.** Copy the messages.
6. **The test evidence.** Note each command or action run, and its
   before and after results as shown.
7. **Last, the title and description.** List every sentence that
   claims what the change includes or excludes, every section heading,
   every checklist line (with its box state), the issue reference, and
   any AI-use disclosure, each quoted.

## Evidence gathering

Build these pairings from your notes. Each one is a table you fill
before grading anything.

1. **diff-within-plan:** for each hunk from Read order step 4, write
   the plan deliverable (3a) or note (3c) it serves, or "test",
   "explanatory comment", or "repo-asked entry (changelog/whatsnew/
   i18n)". If none fits, write "UNPLANNED: <what it does>". Check each
   UNPLANNED hunk against the boundary (3b): a hunk the plan explicitly
   scopes out is UNPLANNED even if it seems helpful.
2. **plan-delivered:** for each deliverable in 3a, write the hunk that
   delivers it, or "MISSING". For each MISSING item, look for a note
   in 3c AND a sentence in the description that discloses it; record
   both quotes or write "no note".
3. **description-matches-diff:** for each claim from Read order step
   7, write "true" with the hunk that shows it, or "CONTRADICTED"
   with the hunk or missing file that contradicts it. Skip claims about
   test results; those go to steps 4 and 5.
4. **repro-before-after:** for each repro case in 3e, write the
   evidence line that re-runs it, its before, and its after, or "NOT
   RE-RUN". Check that the command or input in the evidence is the
   repro case's (3e), not a control or a different input. For every
   other test-plan item in 3d (controls, new-behavior checks), write
   the stated result or the regression test that asserts it, or "NOT
   COVERED"; these need no before. Mark an
   after that is only a verdict ("works now") as "NOTHING OBSERVABLE".
   If a regression test is offered as proof, write what it asserts.
5. **repo-checks-run:** list each suite or check named in 3d or in
   step 1's asks. Beside each, quote the reported command and outcome,
   or write "NOT REPORTED". Note any failing result and whether the
   evidence gives a reason it is unrelated to the diff.
6. **diff-clean:** go through the added and removed lines of every
   hunk and quote each debris line: debug prints or logs (also when
   commented out), commented-out code, dead or unused functions,
   unrelated TODOs, formatting-only or import-only changes, whole runs
   of removed lines re-added unchanged. Quote the commit messages.
7. **template-asks-met:** for each gradable ask from Read order step
   1, quote the description section or diff file that meets it, or
   write "IGNORED". For a ticked checklist box, check it against the
   evidence.
8. **ai-disclosure:** quote the policy's disclosure requirement, or
   write "none stated". Then quote the description's disclosure
   sentence, or write "absent".
9. **Preferred checks:** quote the title and the commit messages.

Live mode: the same pairings, with these sources: the plan is
`plan.md`; the diff is `git diff main...HEAD`; the commits are
`git log main..HEAD`; the evidence is `test_evidence.md` plus the
description's Testing section; the description is `pr_draft.md`
after its first line; the asks come from the template,
`docs/CONTRIBUTING.md`, `scope.md`'s house rules, and the course's AI
disclosure ask. Also check the branch name (`git branch
--show-current`) against the house rule `<type>/<issue>-<slug>` and
record it under template-asks-met.

## Check execution

1. Run the required checks in rubric order: diff-within-plan,
   plan-delivered, description-matches-diff, repro-before-after,
   repo-checks-run, diff-clean, template-asks-met, ai-disclosure. Then
   the preferred checks: title-says-what, commits-tell-the-story.
2. Apply each check's pass condition from `rubric.md` to the table
   for that check, word for word. Do not add requirements the rubric
   does not state, and do not excuse a failure the rubric names.
3. Decision rules:
   - One "UNPLANNED" hunk fails diff-within-plan, however small or
     well-meant.
   - One "MISSING" deliverable with "no note" fails plan-delivered.
     A MISSING item with a note in the plan AND a disclosing sentence
     in the description passes.
   - One "CONTRADICTED" claim fails description-matches-diff.
   - One "NOT RE-RUN" or "NOTHING OBSERVABLE" repro case fails
     repro-before-after, unless the description says which case was
     not re-run and why. One "NOT COVERED" control or new-behavior
     item also fails it; a stated result or an asserting test covers
     it.
   - One "NOT REPORTED" suite or check fails repo-checks-run. A failing
     result reported honestly with a reason passes.
   - One quoted debris line fails diff-clean.
   - One "IGNORED" gradable ask fails template-asks-met.
   - A mandatory disclosure that is "absent" fails ai-disclosure; a
     policy of "none stated" passes it.
4. Grade every check, even after one has failed. The student needs
   every result.
5. If the evidence a check names is missing from the package (for
   example, no test-evidence section at all), grade `fail` when the
   pass condition requires that thing to be present. Grade `unclear`
   only when the package is silent on something you cannot decide
   either way.
6. You may grade a check from your tables without rereading the whole
   package. If a table entry is ambiguous, reread only the section the
   check's Evidence column names.
7. For each check, record one line of evidence: the quote or fact
   that decided it (for a fail, the first offending hunk, claim, or
   missing item).

## Verdict assembly

1. List the required checks and their grades.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is
   `reject`.
4. Preferred checks never change the verdict; report them anyway.
5. Name the deciding check: the first required check, in rubric
   order, graded `fail` or `unclear` (or "all required checks
   passed"). In the summary, quote the two things it compared side by
   side, for example the plan's boundary next to the unplanned hunk,
   the description's claim next to the diff, or the test plan's repro
   case next to the evidence actually run.
6. Emit the JSON block exactly as `SKILL.md` specifies: every check
   in rubric order, one evidence line each, the verdict, and nothing
   after it.
