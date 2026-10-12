# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/ShalomDee/pathreview-ai301-fa26-s1/pull/1

**Branch**

fix/60-none-context-chunk-text

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 19/20 (`categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, `agreement: 19/20 scored items  (bar: 18/20: PASS)`). The one disagreement was `pkg-05` (clear-accept, gold `accept`), which my tool rejected on `repro-before-after`.
2. Partial run, `--only pkg-05,pkg-07,pkg-04,calib-04 --include-calibration`, after I revised `repro-before-after`: 3/3 scored items agreed (`pkg-05` now `accept`; the not-tested canaries `pkg-04` and `pkg-07` still `reject`), and `calib-04` still `reject`. Partial runs don't set the bar.
3. Full run, saved with `--save-run` as the `eval-run.txt` in this folder: 20/20 (`categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, `agreement: 20/20 scored items  (bar: 18/20: PASS)`).

I ran the harness with the same Windows fix as units 2 and 3 (`claude.cmd` instead of `claude` in `run_eval.py`, and `PYTHONUTF8=1`). No run crashed this time.

**Package analysis**

`pkg-05` (nushell/nushell#18848, clear-accept, gold `accept`). In my first full run my rubric said `reject`; in the saved run it says `accept`, matching the gold label.

The plan's test plan names three things: "re-run the issue's script, expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row plus a warning; cargo test on the config crate." The PR's evidence shows the first one decisively: the before table on 0.115.0 has only the `up` row, and the after table has both `char_r` and `up`. For the second it only says "Same-key redefine prints the one-time warning", but the diff adds `same_name_same_key_replaces_with_warning`, which asserts `merged ... count(), 1` and `warnings.len(), 1`.

My first version of `repro-before-after` said "Every repro case or failure mode the test plan names must be covered", so the grader treated the same-key item as a repro case and failed it: "Test plan's second case (same-name same-key, expect one row plus a warning) has only the assertion 'Same-key redefine prints the one-time warning' with no warning text, row output or before". But that item isn't a failing scenario from the repro evidence. The repro evidence only shows the two-key script losing the ctrl-r row. The same-key item checks new behavior the fix adds (the warning), and there is no "before" for a warning that didn't exist yet. After the revision, the grader's evidence in the saved run is: "Issue script re-run: before shows only the up row on 0.115.0, after shows char_r and up rows; the same-key control is covered by the stated warning result and the asserting test". Everything else about pkg-05 passed in both runs: the merge-key hunk, the `warn_once` hunk, and the tests are all plan deliverables, and the template's Description and User-facing changes sections are filled.

**Check rationale**

`| repro-before-after | The PR's test evidence section (live: `test_evidence.md` and the description's Testing section), read against the plan's test plan and the repro case. | Pass if the evidence re-runs the repro case (the same command, input, or user steps, on the fixed branch) and states (a) the before result, observed or carried from the repro evidence, and (b) the specific after result: the output text, exit code, count, rendered value, toast text, or visible state, matching or explaining the plan's expected-after. Every failing scenario the plan's repro evidence shows (each repro case or failure mode) must be re-run this way, unless the description says which one was not re-run and why. Other test-plan items, such as controls and checks of new behavior the fix adds (a warning, an error message), need no before: they pass when the evidence states their result or a regression test in the diff asserts them. Fail if the evidence is only a verdict with nothing observable ("works now", "colors work", "verified working", "fixed", "tested locally"); if it exercises a different path than the repro case (only a control, a different request or input, a single-file case when the bug needs two files); if it omits a failing scenario from the repro evidence without saying so; or if there is no evidence. A regression test offered as proof must assert the repro case, not neighboring behavior. | required |`

It reads this way because of the lecture's line "Evidence names the observable change. 'Tests pass' names nothing." The fail list names each way the not-tested packages dodge that. pkg-04 and pkg-10 give a verdict instead of output ("colors work now", "verified working on my machine for a full day"). pkg-07 runs only the single-file control, the path that never broke. pkg-14 runs a GET with header casing when the bug is a single-header POST. The last sentence exists because pkg-14's added test, `test_header_casing_preserved`, tests casing, not the fix.

The middle two sentences are the revision. I first wrote "Every repro case or failure mode the test plan names must be covered", aiming at calib-04, whose plan names two failure modes while the evidence re-runs only repro 1. That wording also caught pkg-05's same-key warning item, which isn't a failure mode at all. So I tied "must be re-run with a before and after" to the failing scenarios in the repro evidence, which is still what fails calib-04 (its repro evidence shows both `-j 9223372036854775807` and `-j 200000 -x echo` failing), and let controls and new-behavior items pass on a stated result or an asserting test. I rejected the opposite fix, dropping the "every failing scenario" sentence entirely, because then calib-04's silently skipped `--exec` abort would pass.

**Trade-offs**

The revision loosened a check, so before spending a full run I re-graded `pkg-05` with canaries: `pkg-04` and `pkg-07` (both not-tested, both agreeing in run 1) and the trap `calib-04` through `--include-calibration`. All three still came back `reject`. calib-04 still failed on this check: "Plan's repro 2 (`-j 200000 -x echo .`) is never re-run in the test evidence and the description does not say it was skipped; only repro 1 and the -j 64 control are shown." The confirming full run then showed nothing else moved: not-tested stayed 4/4, and the only change from run 1 was `pkg-05` flipping to agree.

The cost I accept: for controls and new-behavior checks, the check now trusts a stated result. A PR could write "control unchanged" or "prints the warning" without having run anything, and as long as the repro case itself is shown decisively, that sentence passes. I accept that miss because demanding a before/after for every control would hold good PRs, like pkg-05, over items that cannot have a "before".
