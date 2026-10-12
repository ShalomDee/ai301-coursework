# Rubric: is this pull request ready to submit?

Every check reads the package's own text. Terms used below:

- **Plan** means the whole accepted plan the PR claims to implement:
  its diagnosis, scope / in-scope / not-in-scope lines, Files list,
  Approach or Change steps, Test plan, risks, and any deviation,
  deferral, or "known limit" note (in a bundle: the "Plan context"
  block; live: `plan.md` including `## Deviations`).
- **Diff** means the candidate PR's unified diff (live: the output of
  `git diff main...HEAD`). The diff is the truth about what the PR
  changes. The description and commit messages are claims about it.
- **Repo asks** means what the repo states it wants from a pull
  request: the "pull requests" and "contribution policy" lines of the
  Repo facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, the scope's house rules, and the course's
  standing AI-disclosure ask).
- **Repro case** means the failing scenario the plan's repro evidence
  shows and the plan's test plan says will be re-run (not its
  controls).

Treat every package as AI-assisted work.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | Every changed file and every hunk in the diff, read against the plan's Files list, scope lines, Approach steps, and deviation or deferral notes. | Pass if every hunk is one of: the planned change itself; a test that exercises the fixed behavior; a comment explaining the planned change; an entry the repo asks for (CHANGELOG, whatsnew, release notes, i18n string for new user-facing text); or a change a deviation note in the plan names with a reason. Fail if any hunk adds work the plan does not name: a refactor, rewrite, or simplification of other code; a rename pass; a new option, flag, setting, or config field; a dependency bump; reworded text unrelated to the fix; edits to a file the plan never mentions. Fail even when the planned fix inside the diff is correct, and even when the extra work is adjacent to the fix. | required |
| plan-delivered | Each deliverable the plan names (each numbered change, each file, each "and also" item such as a docs update, warning, or test) looked up in the diff. | Pass if every deliverable the plan names is present in the diff, OR its absence is recorded both in the plan (a deviation, deferral, or "known limit" note) and in the PR description. A deliberately deferred part of the issue, disclosed in both places, passes: an honest shortfall is not a failure. Fail if a named deliverable is missing from the diff with no such note, or if the diff implements a different change than the plan's chosen approach (for example a library bump instead of the planned code fix). | required |
| description-matches-diff | Every sentence in the PR description that says what the change does or does not include ("implements the plan exactly", "no other changes", "no functional changes outside X", "docs updated", "CHANGELOG entry added", "tests added"), read against the diff. | Pass if every such claim is true of the diff. Read claims about size loosely when the diff agrees in substance (a new helper extracted for the one changed function is still "one function touched"). Fail if the diff contradicts a claim in substance: the description says nothing else changed, or that it follows the plan exactly, while the diff carries unplanned work; or the description says something is included (docs, a changelog entry, tests, a second fix) that the diff does not contain. Claims about test results are graded by the test checks, not here. | required |
| repro-before-after | The PR's test evidence section (live: `test_evidence.md` and the description's Testing section), read against the plan's test plan and the repro case. | Pass if the evidence re-runs the repro case (the same command, input, or user steps, on the fixed branch) and states (a) the before result, observed or carried from the repro evidence, and (b) the specific after result: the output text, exit code, count, rendered value, toast text, or visible state, matching or explaining the plan's expected-after. Every failing scenario the plan's repro evidence shows (each repro case or failure mode) must be re-run this way, unless the description says which one was not re-run and why. Other test-plan items, such as controls and checks of new behavior the fix adds (a warning, an error message), need no before: they pass when the evidence states their result or a regression test in the diff asserts them. Fail if the evidence is only a verdict with nothing observable ("works now", "colors work", "verified working", "fixed", "tested locally"); if it exercises a different path than the repro case (only a control, a different request or input, a single-file case when the bug needs two files); if it omits a failing scenario from the repro evidence without saying so; or if there is no evidence. A regression test offered as proof must assert the repro case, not neighboring behavior. | required |
| repo-checks-run | The test evidence and description, read against the test suite or checks the plan's test plan names and the checks the repo asks for (live: the four checks in the PR template's Testing list). | Pass if every suite or check the plan's test plan or the repo asks for is reported by name (the command or the suite) with its outcome, OR neither the plan nor the repo names any suite. A failing or empty check reported honestly, with its output and the reason it is not caused by this diff, passes. Fail if a named suite or check is never reported; if the only report is an unnamed "all tests pass" or "CI should pass"; or if a reported result is contradicted by the evidence shown. | required |
| diff-clean | The diff's added and removed lines, hunk by hunk, plus the commit list. | Pass if the diff contains only lines that carry the change. Fail on any debris: a debug print or log line left in (`print`, `eprintln!("DEBUG ...")`, `console.log`, `dbg!`), even as a comment; commented-out code (an old attempt or experiment kept "for reference"); dead code (an unused function, `_unused` or `allow(dead_code)` helpers, probes); a TODO or note unrelated to the fix; formatting or whitespace churn, import reordering, or a hunk whose removed and added lines are identical or differ only in layout, when it changes no behavior and no plan or deviation note requires it; working notes committed (plan.md, comment.md, pr_draft.md, test_evidence.md). Lines repeated unchanged inside a statement whose other line is the real change (a multi-line call where one argument changes) are part of that change, not churn. Explanatory comments about the fix are not debris. Throwaway commit messages ("wip", "fix", "misc cleanups while debugging") support a fail found in the diff but do not fail it alone. | required |
| template-asks-met | The repo asks, item by item, read against the PR description and the diff. | Pass if every repo ask that a bug-fix PR's own text or diff can satisfy is met with real content: each required template section present and filled (not left as a placeholder); the issue referenced in the form the template asks for ("closes #N", "fixes #N") where it asks; each required checklist item addressed; a changelog or whatsnew entry present in the diff when the repo requires one for bug fixes; a conventional-commit title where required; no checklist box ticked that the evidence shows is false. A repo with no template and no stated asks passes. Do not grade process steps the package cannot show (signing a CLA, a vouch flow, being assigned the issue, CI that only runs after opening). Fail if any applicable ask is visibly ignored, however good the fix is. | required |
| ai-disclosure | The repo's stated AI policy (the contribution policy line; live: the course's standing ask) read against the PR description. | Pass if the repo states no AI-disclosure requirement for pull requests, OR the description contains an explicit AI-use disclosure that meets the policy's terms (for example names the tool and the extent of the help, or confirms the words are the contributor's own, when the policy asks for that). Because every package is AI-assisted work, a policy that requires disclosing all AI usage "in any form", or requires it for pull requests, makes the disclosure mandatory: if the description has none, fail. A policy that only asks for human-written comments is met by a description in the contributor's own words that says so. | required |
| title-says-what | The PR title. | Pass if the title names what changed and where (a component, function, flag, or command), so a maintainer scanning the list can decide in thirty seconds. Fail on titles that name nothing ("fixed the bug!!", "fix", "Update file"). | preferred |
| commits-tell-the-story | The commit list. | Pass if each commit message says what its commit changes. Fail on throwaway messages ("wip", "fix", "hopefully", "misc"). | preferred |

## Verdict rule

Grade every check `pass`, `fail`, or `unclear`. The verdict is
`accept` (ready to submit) only if every `required` check is `pass`.
Any `required` check graded `fail` or `unclear` makes the verdict
`reject` (hold): a required fact that cannot be verified from the
package is not ready to submit. Use `unclear` only when the package
genuinely lacks the evidence a check names, never as a softer fail.
`preferred` checks are reported but never change the verdict.

Length, polish, confidence, and a filled-in checklist never earn a
pass on their own, and terseness never loses one: a short PR that
delivers its plan cleanly, with observable evidence and the repo's
asks met, is `accept`. A PR that discloses a shortfall in both the
plan and the description is graded on what it says it delivers.
