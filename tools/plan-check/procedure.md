# Procedure: how this skill grades a plan package

These are steps for grading a plan someone else wrote, not for writing
one. Do every step in order. Write down what each step asks you to
note before moving on; later steps use those notes.

## Read order

1. Read the Repro evidence block first, before the plan, so the
   plan's confident wording cannot set your expectations. Note, as a
   numbered list, every step and every control run with its result
   (what was varied, and whether the bug appeared). Note the Expected
   and Actual lines. Note what the evidence isolates as the trigger
   (the variable that turns the bug on and off).
2. Read the Issue section. Note in one line what the reporter asks to
   be fixed. This is the yardstick for scope.
3. Read the Thread highlights. For each comment note the author's
   role. List separately every maintainer comment (OWNER, MEMBER,
   COLLABORATOR, CONTRIBUTOR) that gives explicit direction: names a
   culprit location, proposes or rejects an approach, posts a patch
   or test build, or asks for testing. Ignore NONE comments as
   direction, but note any cause they claim, because a plan may copy
   it.
4. Read the Repo facts block. Copy the contribution policy line
   verbatim into your notes and mark whether it requires anything of
   issue comments (AI disclosure, human-written words, templates).
5. Only now read the Candidate plan, then the Candidate plan comment.

Live mode: the repro evidence is the student's posted repro comment
on the issue (or the repro quoted in the drafts); the thread is the
live issue thread; repo facts come from the repo's CONTRIBUTING and
AI policy files; the plan and comment are the student's drafts. Keep
the same order.

## Evidence gathering

For each check, pull exactly this, and quote it in your notes:

1. diagnosis-grounded: quote the plan's cause sentence. Next to it,
   put your list from Read order step 1. For each repro step or
   control, write one word: "consistent" or "contradicts", with the
   reason. A control that shows the bug with the blamed component
   absent or bypassed, or the blamed component working under the
   bug's conditions, or the failure already present before the blamed
   component runs, is "contradicts".
2. bounded-scope: list every distinct change the plan proposes, from
   the scope lines AND every numbered item in Changes / Proposed
   changes / Approach / Files (scope evidence often lives under
   Changes, not under Scope). Mark each one "needed for the bug",
   "test/proof", "safety", or "extra". Explicit deferrals are not
   changes.
3. executable-by-a-stranger: quote the files or areas named and the
   one sentence that states the chosen change. Quote any phrase that
   leaves the core decision open ("somewhere", "or", "not sure",
   "investigate", "profile").
4. test-decisive: quote the test plan. Write the observable outcome
   it names and whether that outcome differs before vs. after the
   fix given the repro's Actual line.
5. thread-engaged: from Read order step 3, take the maintainer
   direction list. For each item, quote where the plan or comment
   follows it or names it, or write "not engaged".
6. repo-policy: from Read order step 4, take the requirement on issue
   comments (or "none"). Quote the comment sentence that satisfies
   it, or write "absent".
7. comment-faithful and unknowns-stated: quote any comment promise
   not in the plan; quote the plan's risk lines.

## Check execution

1. Run the required checks in this order: diagnosis-grounded,
   bounded-scope, executable-by-a-stranger, test-decisive,
   thread-engaged, repo-policy. Then the preferred checks.
2. Apply each check's pass condition from rubric.md to the notes from
   Evidence gathering, word for word. Do not add requirements the
   rubric does not state, and do not excuse a failure the rubric
   names.
3. Grade every check, even after one has failed; the student needs
   every result.
4. diagnosis-grounded: one "contradicts" makes it fail. bounded-scope:
   one "extra" makes it fail, even when the core fix is right.
   executable-by-a-stranger: an open core decision fails it; an open
   secondary detail flagged as a risk does not. test-decisive: an
   outcome that would read the same before and after the fix fails.
   thread-engaged: one "not engaged" maintainer direction fails it;
   no maintainer direction passes it. repo-policy: "absent" against a
   real requirement fails it; requirement "none" passes it.
5. If the evidence a check names is genuinely missing from the
   package (for example, no test plan at all), grade fail when the
   rubric's pass condition requires that thing to be present;
   grade unclear only when the package is silent on something you
   cannot decide either way.
6. A check may be graded from your notes without rereading the whole
   package. Reread only the section the check names if a note is
   ambiguous.
7. Record, for each check, one line of evidence: the quote or fact
   that decided it.

## Verdict assembly

1. List the required checks and their grades.
2. If every required check is pass, the verdict is `accept`.
3. If any required check is fail or unclear, the verdict is `reject`.
4. Preferred checks never change the verdict; report them anyway.
5. In the summary, name the deciding check (the first required check
   that failed, or "all required checks passed"), and quote the plan
   line and the evidence line that decided it, side by side when they
   conflict (for example, plan cause vs. repro control).
6. Emit the JSON block exactly as SKILL.md specifies, with every
   check, and nothing after it.
