# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record, read against the issue context and repo-facts block. | Pass if the report identifies the tested software/tool version and OS, plus the commit, tag, runtime, compiler, or other build/toolchain detail when that detail is needed to place the run. The environment must match the issue's target or clearly call out the difference. | required |
| steps-followable | The repro report's setup, commands, inputs, files, and trigger steps, read with the repo's documented setup instructions from the repo-facts block or evidence guide. | Pass if another contributor could start from the stated starting point and run the same reproduction without guessing a missing command, input, configuration, or setup step that could change the result. | required |
| behavior-matches | The report's observed output, logs, screenshots, exit status, or other artifacts, read directly against the behavior and failure mode described in the issue. | Pass if the evidence shows the same behavior the issue reports, not merely a nearby error or a different failure caused by changed input or setup. If the report cannot reproduce the issue, pass this check when the artifacts clearly show what happened instead and do not falsely claim the target behavior occurred. | required |
| outcome-faithful | The report's conclusion and expected-versus-actual statement, read against its environment, steps, and artifacts. | Pass if every conclusion is supported by the evidence shown. A reproduced result must be backed by matching artifacts; a cannot-reproduce result must say so plainly and show the attempted run. Fail if the wording is more certain or broader than the evidence supports. | required |
| repo-conventions | The claim comment and repro report, read against the repo-facts block and any applicable CONTRIBUTING, README, AGENTS.md, issue template, code-of-conduct, or stated disclosure policy identified in the evidence guide. | Pass if the comments follow the repository's stated contribution and communication requirements, including any required AI-assistance disclosure, and do not omit a convention that the repo explicitly requires for this kind of contribution. When the repo's policy requires AI-assistance disclosure, pass only if the claim comment or repro report contains an explicit statement naming the tool used and the extent of its assistance; the absence of any such statement is a fail, not an unclear. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept only if every required check passes. A required check graded `unclear` counts as a fail because the package does not yet contain enough verifiable evidence to post. Reject if any required check fails or is unclear.
