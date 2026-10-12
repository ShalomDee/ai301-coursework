---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, its description, its commit list, its unified
diff, and its test evidence) read against two things: the accepted
plan it claims to implement (including that plan's deviation or
deferral notes and the repro evidence the plan built on) and the issue
that plan belongs to, together with the repo's stated asks for pull
requests (PR template, contributing guide, AI policy).

Answer only that question, for exactly one package per run. Do not
review the code's style, do not judge whether the issue was worth
fixing, do not re-grade the plan's diagnosis (that was week 3's
question), and do not suggest a better fix. Never answer from gut
feel: you answer by executing `procedure.md`, which applies the checks
in `rubric.md` to evidence located with `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If the prompt hands you an eval package bundle
(a markdown file headed `# Eval package: <id>`), you are in eval mode.
Otherwise you are in live mode.

**Live mode** checks the student's own pull request before it is
opened. Run from the top folder of the student's clone of their fork,
on their fix branch. Read these inputs, and only these:

1. `plan.md` (in that folder): the posted plan, including its
   `## Deviations` section. This is the plan the diff is read against.
2. The branch's diff: the output of `git diff main...HEAD` (three
   dots), run from the working copy. This is every committed change
   the branch makes relative to `main`. Also run
   `git diff main...HEAD --stat` for the file list and
   `git log main..HEAD --format='%h %s%n%b'` for the commit list.
   Uncommitted edits and untracked files are NOT part of the PR; do not
   grade them. (`plan.md`, `comment.md`, `pr_draft.md`, and
   `test_evidence.md` are working notes and must never appear in the
   diff; if one does, that is debris.)
3. `pr_draft.md`: the first line is the PR title; everything after it
   is the PR description, written into the repo's PR template.
4. `test_evidence.md`: the captured before/after output and the
   repo's checks' output.
5. The issue: fetch its thread with `gh issue view <number> --repo
   <scope repo> --comments` (or the GitHub API/web if `gh` is
   unavailable). The issue URL or number comes from the student's
   request or from `plan.md`.
6. The repo's stated asks, read from the working copy:
   `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.
   The course itself always asks for an AI-use disclosure in the PR's
   Notes for Reviewers section; treat that as a stated ask.

The student's posted unit-2 repro comment on the issue (linked from
`plan.md`) is the repro the test evidence must re-run. A student on
the house chain reads the house plan and the house repro pack in
place of their own plan and repro comment; every check grades the
same things there.

**Eval mode** grades a package bundle. The bundle is the whole world:
take every fact from the bundle text, fetch nothing, read no other
file, and ignore `scope.md` and `voice-guide.md` entirely. The
bundle's sections map onto the live inputs: "Plan context" is
`plan.md`, "Repo facts" is the template and contributing guide,
"Issue" and "Thread highlights" are the issue, and "Candidate PR"
holds the title, description, commits, diff, and test evidence. Eval
mode always grades a complete package: every check in the rubric,
then the full verdict rule. Use the bundle id as the `item`.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before any git command or fetch.

- If its `Repo:` line still carries the placeholder
  (`<ORG>/<PATH-REVIEW-REPO>` or anything else in angle brackets),
  stop without grading. Tell the student to fill the `Repo:` line in
  `scope.md` with their section's Path Review repo, and emit no
  verdict JSON.
- Confirm the PR will target the scoped repo: the issue must belong
  to the repo on the `Repo:` line, and the working copy must be a
  fork or clone of it (`git remote -v`). If the issue or the repo is
  anywhere else, refuse to grade and say which repo the scope allows.
  Never guess or widen a scope.
- Apply the house rules listed there as stated asks: the branch name
  must be `<type>/<issue-number>-<slug>` with the PR's own issue
  number, the PR delivers one posted plan for one issue, and the
  template is filled in full. A house rule changes what the rubric's
  checks read as the repo's asks; it never adds a check of its own.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the
outgoing text (the PR title and the PR description from
`pr_draft.md`) against each of its rules and its "Things I never
post" list. For every rule the draft breaks, report it in the summary
under a "Voice" heading: quote the rule's name, quote the draft
sentence that breaks it, and suggest the rewrite in one line. The
voice guide never changes the verdict on its own, because voice is
personal and the rubric has no check that reads it; a voice note is
advice, not a grade.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read all three components before grading, in this order:

1. `rubric.md` defines the checks (name, evidence, pass condition,
   weight) and the verdict rule. It decides WHAT passes.
2. `references/evidence-guide.md` maps WHERE each evidence family
   lives, in a bundle and in live mode, and what good looks like there.
3. `procedure.md` is the operating procedure: the read order, the
   evidence-gathering moves, how each check executes, and how the
   grades become the verdict. Execute it as written, step by step,
   exactly the way an executor follows a rubric.

Where the procedure is silent on a step you need, do not improvise
around it: take the most literal reading of the rubric's pass
condition, and name the gap in your summary under a "Procedure gaps"
line so the student can fix the procedure.

If `rubric.md` has no checks in its table or no verdict rule, or if
`procedure.md` or `references/evidence-guide.md` has no steps or
content of its own (only headings and template comments), refuse to
grade: say which file is empty and that this tool cannot grade
without it, and emit no verdict. An empty tool that invents checks at
runtime produces verdicts that look like judgment and are noise.

## Verdict and output

The verdict space is binary: `accept` means ready to submit (open the
PR); `reject` means hold (fix first). There is no third verdict, no
"accept with reservations", and no score. Reservations go in a
check's evidence line, never in the verdict.

Before the JSON block, print a short readable summary: one line per
check (name, grade, and the deciding fact), then the verdict with its
deciding check per the procedure. In live mode, add the "Voice" notes
and, on a reject, a short "Fix before opening" list, one line per
failed check. Then end your reply with this fenced JSON block, valid,
with every rubric check in rubric order, and nothing at all after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, before the PR exists, set `item` to the issue URL plus
the branch name (for example `<issue URL> @ fix/60-none-context`).

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quoting the line that decided it: a diff line, a file path, a
  description sentence, an output line. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse PR that delivers its
  plan, with evidence, can be ready; a long, confident, beautifully
  formatted one can be hiding drift. Read the diff against the plan,
  the evidence against the test plan, and the description against the
  diff. Never let the description's confidence stand in for the diff.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of silently inventing steps.
- **Unclear defaults to fail.** Apply the rubric's verdict rule to
  `unclear`. Where the rule is silent, an unverifiable claim is a
  failing one: a PR whose readiness you cannot verify from the package
  is not ready to submit. Use `unclear` only when the package genuinely
  lacks the evidence a check names, never as a softer `fail`.
- **Honest shortfalls are not failures.** A PR that does less than
  everything, and says so in the plan's note and the description, can
  be ready. Do not equate "less than the whole issue" with "hold".
