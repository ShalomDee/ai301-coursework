# Evidence guide: where evidence lives in a PR package

An eval bundle is one markdown file with these sections, in order:
Repo facts, Issue, Thread highlights (each comment tagged with the
author's role), Plan context (the repro evidence the plan built on,
then the plan itself), and Candidate PR with five subsections: Title,
Description, Commits, Diff, Test evidence. Plan wording varies by
author ("Scope, one bounded change", "Not in scope", "Files", "Test
plan", "Known limit", "Deliberately deferred"); read the whole Plan
context block, not only the heading a check's name suggests.

In live mode the same evidence lives in the student's working copy
of their fork, on the fix branch:

| Bundle section | Live source |
|---|---|
| Plan context (plan + repro) | `plan.md` with `## Deviations`; the student's posted unit-2 repro comment on the issue |
| Diff | `git diff main...HEAD` (three dots); file list: add `--stat` |
| Commits | `git log main..HEAD` |
| Title, Description | `pr_draft.md`: line 1 is the title, the rest is the description |
| Test evidence | `test_evidence.md`, and the description's Testing section |
| Repo facts | `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, `scope.md` house rules |
| Issue, Thread | `gh issue view <n> --repo <scope repo> --comments` |

## Plan fidelity (harness category: silent-drift)

- **Where it lives (eval):** the plan's deliverables are in Plan
  context: the "Scope" sentence, the "Files" list, the numbered or
  "and" items, and the "Not in scope" line. Deviation, deferral, and
  limit notes sit inside the plan text ("Known limit, stated in the
  plan", "Deliberately deferred"). What actually changed is in the
  Diff: the `--- a/` and `+++ b/` file headers and each `@@` hunk. The
  Commits list often names the drift outright ("refactor: simplify
  truncation helpers", "Add experimental.X setting", "bump tcell").
  The description's fidelity claims ("implements the plan exactly",
  "no functional changes outside", "docs now document") are in
  Description.
- **Where it lives (live):** `plan.md` (Files, Approach, Scope, and
  `## Deviations`), against `git diff main...HEAD --stat` and the full
  diff, and against the description in `pr_draft.md`.
- **What good looks like:** every changed file and hunk traces to a
  plan deliverable, a test of the fix, a repo-asked entry (changelog,
  whatsnew, i18n string), or a deviation note that names it with a
  reason. Every plan deliverable is in the diff, or its absence is
  noted in the plan AND restated in the description (a known limit or
  a deferred code path, written down in both places, is honest; a
  promised docs update that is missing under a "docs now document it"
  claim is not). The description claims exactly what the diff does.
- **What drift looks like:** more than the plan (a refactor, rename
  pass, new option or setting, a rewrite of a file the plan never
  names, unrelated text edits), less than the plan (a promised docs
  update, test, or second fix missing), or a different change than
  planned (a library bump instead of the planned code fix), in every
  case with no note, and often under a description claiming fidelity.

## Test evidence (harness category: not-tested)

- **Where it lives (eval):** the Candidate PR's "Test evidence"
  subsection, plus any before/after the Description carries. Read it
  against the plan's "Test plan" sentence (each repro case and failure
  mode, each expected-after, each control, each named suite) and the
  repro evidence at the top of Plan context (the failing command and
  its output). The Diff shows any regression test; read what it
  asserts.
- **Where it lives (live):** `test_evidence.md` (the repro re-run on
  `main` for the before and on the branch for the after, plus the
  output of `make test-unit`, `make test-integration`, `make lint`,
  `make typecheck`), the description's Testing section, and the plan's
  `## Test plan`. The repro to match is in the student's posted repro
  comment and quoted in `plan.md`.
- **What good looks like:** the repro case itself re-run, with the
  same command or steps as the repro evidence: the before (observed
  or carried from the repro) and a specific after (the output, the
  exit code, the count, the toast text, a byte-identical diff), meeting
  the plan's expected-after. Every case the test plan names is covered
  or its omission explained. The named suites and checks are reported
  by command with their outcome. A check that fails for a reason
  outside the diff (a local environment problem, "no tests ran") is
  reported as is, with the reason.
- **What not-tested looks like:** "colors work now", "verified
  working for a day", "all tests pass" with nothing named; evidence
  from the control or a different input (a one-file case when the bug
  needs two files, a GET when the bug is a POST); one of two named
  failure modes silently skipped; a regression test that asserts
  neighboring behavior instead of the repro case; no evidence at all.

## Diff quality (harness category: unreviewable)

- **Where it lives (eval):** the Diff's added (`+`) and removed (`-`)
  lines, hunk by hunk, and the Commits list.
- **Where it lives (live):** `git diff main...HEAD` and
  `git log main..HEAD`. The `--stat` list should name only the files
  the fix changes; `plan.md`, `comment.md`, `pr_draft.md`, and
  `test_evidence.md` must not appear.
- **What good looks like:** every added or removed line carries the
  change, a test of it, or a comment explaining it, so a maintainer
  can read the fix in one breath. Unchanged lines that a diff repeats
  inside the statement being edited (a multi-line call where one
  argument changes) are part of the change.
- **The debris tells:** a debug print or log, live or commented out
  (`// eprintln!("DBG ...")`, `eprintln!("DEBUG ...")`, `print(`);
  commented-out code kept "for reference"; dead code (an unused
  function, `allow(dead_code)`, an `_unused_` probe); an unrelated
  `TODO`; import reordering or restructuring; reformatting a struct
  or statement that does not otherwise change; whole runs of lines
  removed and re-added identically; commit messages like "wip", "fix",
  "fmt + cleanup", "misc cleanups while debugging".

## Standards and comms (harness category: standards-wall)

- **Where it lives (eval):** the Repo facts block. The "pull
  requests" line holds the template's required sections, the
  checklist, the issue-reference form, and the changelog or whatsnew
  ask. The "contribution policy" line holds the contributing rules
  and any AI policy. Read them against the Description (sections,
  checklist answers, `closes #N`, disclosure sentence), the Title, and
  the Diff (for a required changelog or whatsnew file).
- **Where it lives (live):** `.github/PULL_REQUEST_TEMPLATE.md`
  (Summary, Issue with `Closes #`, Changes, Testing checklist,
  Screenshots / Demo, Notes for Reviewers), `docs/CONTRIBUTING.md`
  (branch naming `<type>/<issue>-<slug>`, Conventional Commit
  messages, all checks run, seeded-bug xfail markers removed), and the
  scope's house rules, against `pr_draft.md` and the branch name. The
  course always asks for an AI-use disclosure in Notes for Reviewers,
  in the student's own words.
- **What compliant looks like:** every required section present with
  real content (an "N/A" with a reason counts; a blank placeholder
  does not); the issue referenced in the asked form; each checklist
  item answered, and no box ticked that the evidence shows is false;
  a required changelog or whatsnew file present in the diff; the
  disclosure present whenever the policy requires all AI usage to be
  disclosed (ghostty-style "in any form" policies), or the words
  stated as the contributor's own where the policy asks for that
  (ripgrep-style). Process asks the PR text cannot show (CLA, vouch,
  CI that runs after opening) are not graded.
- **What the wall looks like:** a good fix whose description skips
  the repo's required checklist, has no `closes #N` where the template
  asks for one, has no whatsnew entry where the repo requires one for
  bug fixes, or has no AI disclosure under a policy that requires
  one. Whether the description's claims match the
  diff is plan fidelity, above, not this family.
