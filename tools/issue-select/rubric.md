# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Last 5 default-branch commits and authors | Most recent human commit or human PR merge is ≤180 days before capture date | required |
| repo-active | Archived flag; last push to any branch | Not archived, and last push ≤365 days before capture date | required |
| scope-bounded | Issue body, comments, linked/closed PRs, opener role, labels | Issue states one problem or one coherent goal with an actionable path to a fix, even if it names multiple causes, files, or candidate approaches. Fail only when: (a) it bundles multiple independent, separately-assignable tasks (e.g. a tracking list of unrelated issue numbers); (b) it's a feature request still awaiting a product/design decision, shown by concrete unresolved specifics like a missing asset, unset boundaries, or explicit deferral ("TBD", "out of scope for v1"); or (c) it invites contributors to self-select an arbitrary, unspecified slice of ongoing work with no stated deliverable. A brief issue naming specific examples, even with a trailing "etc.", is still bounded. Repeated abandoned attempts fail it unless a maintainer has since fixed the scope | required |
| unclaimed | Assignees, linked PRs (structured and comment-mentioned), comment thread | Fail if assigned, if any linked or comment-mentioned PR is open, or if someone states active work within 30 days. An unresolved referenced PR still blocks past 30 days unless closed/abandoned by its author or reopened by a maintainer. Old claims with no PR that were auto-unassigned or released don't block | required |
| ai-policy | Contribution policy / CONTRIBUTING.md | Pass unless the repo bans AI-assisted contributions outright, including reviewed and human-understood work. Conditional allowances (review, test, own the change) pass, as does no stated policy | required |
| responsive | Maintainer first-response sample; comment thread | ≥1 sampled issue got an Owner/Member/Collaborator reply within 30 days | preferred |

## Verdict rule

Accept only if every required check passes. Preferred checks affect ranking, not verdict. Unclear evidence on a required check fails it.