# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
- **Where it lives:** In an eval bundle, look at the issue context for the reported target, the repo-facts block for repository or release context, and the repro report's environment record for what was actually tested. In live mode, compare the issue thread with the student's environment block and the repository's README, CONTRIBUTING guide, release information, or other setup documentation.
- **What good looks like:** The report names the tested software/tool version and OS, and includes a commit, tag, runtime, compiler, architecture, installation method, or other toolchain detail when it is relevant to reproducing the issue. The tested environment matches the issue's target, or the report explicitly identifies the difference rather than treating two environments as equivalent.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
- **Where it lives:** In an eval bundle, use the repro report's setup, commands, input files or payloads, configuration, and trigger steps, checked against the issue context and repo-facts block. In live mode, read the reproduction steps together with the repo's documented setup instructions and the issue's original example.
- **What good looks like:** A stranger can start from the stated starting point and reach the attempted behavior without inventing a missing command, input, configuration, or setup decision. Inputs preserve the parts of the issue example that affect the behavior, and commands are concrete enough to run.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
- **Where it lives:** In an eval bundle, use the report's output excerpts, terminal logs, stack traces, screenshots, exit codes, generated files, or other artifacts, then compare them directly with the behavior described in the issue context. In live mode, use the artifacts attached or pasted in the draft report and the original issue's expected and reported behavior.
- **What good looks like:** The artifact itself shows the target behavior or failure mode. A panic should not be treated as reproduced by an unrelated parse error, and a different exit status, message, input, or code path must not be presented as equivalent without evidence. Expected versus actual behavior is explicit enough to identify the delta.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
- **Where it lives:** Compare the repro report's conclusion and expected-versus-actual statement with its environment, steps, and artifacts. Also read any claim in the comment that says the issue was reproduced, not reproduced, confirmed, intermittent, version-specific, or otherwise characterized.
- **What good looks like:** The wording goes no further than the evidence. A successful reproduction is supported by matching artifacts. A cannot-reproduce result is also valid when the report records the attempted environment and steps and shows the observed result. Uncertainty, environment differences, or partial results are stated instead of being hidden behind confident language.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
- **Where it lives:** In an eval bundle, read the claim comment and repro report against the issue context and repo-facts block, including any stated CONTRIBUTING, README, AGENTS.md, issue-template, code-of-conduct, or AI-use disclosure requirements. In live mode, check the draft comments against the actual issue thread and the repository's current contribution and communication rules before posting.
- **What good looks like:** The claim is specific to the issue and promises only investigation and a report, not a guaranteed fix or date. The repro comment is the contributor's own evidence rather than a piggyback confirmation. Both comments follow explicit repository requirements, including disclosure rules when required, and avoid claims that the evidence does not support.
