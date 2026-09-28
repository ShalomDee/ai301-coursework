# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->
I am a student contributor investigating a specific issue in the repository. I write from what I have actually checked, not from what I assume is true. Maintainers can expect concise comments that name the behavior, show my evidence, and make clear what I will do next.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->
### Rule: Name the exact issue

I name the concrete behavior, version, command, or input when it matters instead of referring vaguely to "this bug" or "the issue."

- Wrong: "I'm picking up this bug and will look into it."
- Right: "I'm picking up the report that `--style` is ignored on v1.20.0, and I'll investigate it and write up what I find."

### Rule: Promise only the next step

I can promise to investigate and report back. I do not promise a fix, a merge, or a date before I know what the problem requires.

- Wrong: "I'll have this fixed by tomorrow."
- Right: "I'll reproduce this locally and post a report with my environment, steps, and observed output."

### Rule: Separate evidence from inference

I state what the run showed before offering any interpretation. I do not call something confirmed, easy, or caused by a particular component unless the evidence supports that claim.

- Wrong: "Confirmed. This is definitely a parser bug and should be an easy fix."
- Right: "My run reaches the same panic shown in the issue. I'll keep the cause separate from the reproduction until I trace it."

### Rule: Say uncertainty plainly

If I cannot reproduce the behavior, or if my environment differs from the report, I say that directly instead of forcing a confident conclusion.

- Wrong: "It basically reproduces, so I think the report is correct."
- Right: "I could not reproduce the reported panic on this setup; my run exits normally. I've included the exact version, command, and output below."

### Rule: Keep the tone plain

I skip praise, filler, exaggerated enthusiasm, and language that sounds like I am trying to impress the maintainer. The comment should read like a useful engineering update.

- Wrong: "Hi!! I'm super excited to contribute to this amazing project and tackle this issue!"
- Right: "I'm investigating this issue and will post a reproduction report with the environment, steps, and output."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- A promised fix or delivery date before I have investigated the issue.
- "Confirmed" or "reproduced" without evidence that matches the reported behavior.
- "Easy fix," "obvious cause," or another diagnosis I have not verified.
- "Same as above, can confirm" instead of my own reproduction evidence.
- Generic praise, assignment requests, or filler that does not help triage.
- A claim that hides uncertainty or leaves out a repository-required disclosure.
