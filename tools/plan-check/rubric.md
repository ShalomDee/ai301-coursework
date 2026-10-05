# Rubric: is this plan ready to post and build from?

Every check reads the package's own text. "Plan" means the whole
candidate plan section, whatever its headings are called (Diagnosis,
Cause, Summary, Scope, Change, Changes, Proposed changes, Approach,
Files, Test, Test plan, Risk). "Maintainer" means a thread participant
marked OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR; a NONE participant
is never a maintainer, however confident they sound.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (Diagnosis / Cause / Summary) read against every step, timing, artifact, and control run in the Repro evidence block, and against the Expected/Actual lines there. | Pass if the stated cause is consistent with every result in the repro evidence, including every control run. Fail if any repro step or control run shows the bug's behavior with the blamed component absent, disabled, or bypassed, or shows the blamed component working correctly under the bug's conditions, or shows the failure already present at a stage before the blamed component runs. Fail if the plan adopts a cause from the thread or issue that the repro evidence contradicts. Agreeing with a confident thread comment is never enough on its own; the repro evidence decides. | required |
| bounded-scope | Every item the plan says it will change: the scope / in-scope / not-in-scope lines AND every numbered item under Changes / Proposed changes / Approach / Files, read against what the issue asks for. | Pass if every proposed change is needed to fix the reported bug, prove the fix (tests, a doc line describing the fixed behavior), or make the fix safe. Fail if the plan adds any work the issue does not need: a refactor, rename, rewrite, redesign, dependency migration or upgrade, new option or settings field, new framework or abstraction, CI changes, or "while I'm here / while touching" work, even when the core fix inside it is correct. Deferring part of the issue and saying so is bounded, not a fail. | required |
| executable-by-a-stranger | The plan's Files / Changes / Approach sections. | Pass if the plan names the files or code areas it will change (a named module, crate, or function area is enough; exact line numbers are not required) AND commits to one concrete change a stranger could start making today. Fail if the core decision is left open: no files or areas named, the change is "investigate / profile / poke around", alternatives are left unresolved ("X or Y, whichever is easier", "not sure which layer"), or the location is "somewhere". An honestly flagged risk or open question about a secondary detail does not fail this check. | required |
| test-decisive | The plan's Test / Test plan section, read against the Repro evidence block's steps and its Expected line. | Pass if the test plan states a concrete observable outcome that would be different before and after the fix (a specific output, exit code, timing, color, count, or a named new regression test asserting the repro case), tied to the reproduced behavior. Fail if the only test is "run the existing/full test suite", "nothing regresses", "should feel fast/better", "see if it works", or any outcome a stranger could not check against the repro. | required |
| thread-engaged | The Thread highlights section (maintainer comments only, per the definition above) read against the plan and the Candidate plan comment. | Pass if there is no explicit maintainer direction in the thread, OR the plan follows that direction, OR the plan comment explicitly names the direction and explains why it diverges. Explicit direction means a maintainer pointing to a culprit location, proposing or rejecting a fix approach, posting a patch or test build, or asking for testing. Fail if the plan ignores or works around that direction without engaging it in the comment (for example, a docs-only workaround when the owner has isolated the culprit code). A maintainer only saying "known problem" or asking for info is not direction. | required |
| repo-policy | The Repo facts block's contribution policy line read against the Candidate plan comment. Treat every package as AI-assisted work. | Pass if the repo's stated policy places no requirement on issue comments, OR the comment meets every requirement that applies to issue comments. When the policy requires disclosing all AI usage "in any form" or for issues/comments, the comment must contain an explicit AI-use disclosure; if it does not, fail. A policy that asks for disclosure only in pull requests, or states no disclosure ask for issue comments, places no disclosure requirement on the plan comment. | required |
| comment-faithful | The Candidate plan comment read against the Candidate plan. | Pass if the comment promises only what the plan contains and does not overstate certainty beyond the plan. | preferred |
| unknowns-stated | The plan's Risk / unknowns lines and any hedged statements. | Pass if the plan names at least one real risk or open question, or the change is small enough that none is credible. | preferred |

## Verdict rule

Grade every check `pass`, `fail`, or `unclear`. The verdict is
`accept` (ready) only if every `required` check is `pass`. Any
`required` check graded `fail` or `unclear` makes the verdict
`reject` (hold): a required fact that cannot be verified from the
package is not ready to build from. Use `unclear` only when the
package genuinely lacks the evidence the check names, never as a
softer fail. `preferred` checks are reported but never change the
verdict. A plan that is short but satisfies every required check is
`accept`; length, polish, and confidence never earn or lose a pass.
