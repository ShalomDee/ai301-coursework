# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ShalomDee

**Plan comment**

PASTE-PLAN-COMMENT-LINK-HERE

Following up on my reproduction above (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878037027), here is my plan.

**Cause:** in `FaithfulnessChecker.check()`, line 38 of `rag/evaluator/faithfulness_checker.py` builds the context with `chunk.get("text", "")`. The `""` default only applies when the `text` key is missing. With `{"text": None}`, `.get()` returns `None`, and `" ".join(...)` raises `TypeError: sequence item 0: expected str instance, NoneType found` before any scoring runs. My control on the same commit backs this up: a chunk with no `text` key (`[{"content": "Python skills"}]`) does not crash and returns `0.0`.

**Change:** on line 38, use `chunk.get("text") or ""` so a `None` text is treated like a missing key. As `docs/CONTRIBUTING.md` asks for seeded bugs, I'll also remove the issue #60 `xfail(strict=True)` marker from `test_none_context_chunk_text`. Not in scope: claim extraction, the overlap scoring from #59 (its xfail markers stay), and other non-string `text` values.

**How I'll show it works:** I'll re-run the test from my reproduction. Today it fails with the `TypeError` (under `--runxfail`). After the change it should pass, with `check("Has Python skills", [{"text": None}])` returning `0.0`. The missing-key control should still return `0.0`, and the test file should go from `18 passed, 4 xfailed` to `19 passed, 3 xfailed`.

I've seen PR #74 on this issue. I'll read it before opening mine so I don't repeat its tests without saying so.

I used AI assistance (Claude) to draft this plan and to run the control. I reviewed it before posting, and I'll read every diff during the build.

---

## Your branch

**Branch**

fix/60-none-context-chunk-text

**Evidence**

Before (on `main` at `f89c06f`, the same command as my unit 2 reproduction):

```
$ .venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv --runxfail
...
        # Concatenate context text
>       context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E       TypeError: sequence item 0: expected str instance, NoneType found

rag\evaluator\faithfulness_checker.py:38: TypeError
=========================== short test summary info ===========================
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text - TypeError: sequence item 0: expected str instance, NoneType found
============================== 1 failed in 0.69s ==============================

$ .venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py -q
..x...x......x....x...                                                   [100%]
18 passed, 4 xfailed in 1.59s

$ .venv/Scripts/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker as F; print(F().check('Has Python skills', [{'content': 'Python skills'}]))"
2026-10-05 00:59:03 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
```

After (on `fix/60-none-context-chunk-text`, commit `66c446f`; the xfail marker is removed, so no `--runxfail`):

```
$ .venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv
collecting ... collected 1 item

tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text PASSED [100%]

============================== 1 passed in 0.38s ==============================

$ .venv/Scripts/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker as F; print(F().check('Knows Python.', [{'text': None}])); print(F().check('Has Python skills', [{'text': None}])); print(F().check('Has Python skills', [{'content': 'Python skills'}]))"
2026-10-05 01:00:11 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
2026-10-05 01:00:11 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
2026-10-05 01:00:11 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0

$ .venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py -q
..x...x......x........                                                   [100%]
19 passed, 3 xfailed in 0.34s

$ .venv/Scripts/python -m pytest tests/unit -m unit -q
376 passed, 52 xfailed, 3 warnings in 16.45s

$ git commit ...   (pre-commit hooks)
ruff.....................................................................Passed
black....................................................................Passed
mypy.....................................................................Passed
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20

That was one complete graded run, the one saved in `eval-run.txt` (`agreement: 20/20 scored items  (bar: 18/20: PASS)`, `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`). Two earlier launches crashed on Windows before grading any package, so they produced no score: first `FileNotFoundError` (the harness couldn't find `claude`, so I applied the same `claude.cmd` fix I used in unit 2), then a `UnicodeEncodeError` writing the prompt to the subprocess (fixed by running with `PYTHONUTF8=1`).

**Package analysis**

`pkg-15` (scope-creep, gold `reject`): my rubric also returned `reject`, so they agree. The note column named `bounded-scope` (plus the preferred `unknowns-stated`). `diagnosis-grounded`, `executable-by-a-stranger`, and `test-decisive` all passed, because the core fix is right: the plan's item 1, "Set the default `autoSelectFamilyAttemptTimeout` to 500 ms at app startup", matches the timeout the repro's control matrix isolates. The package is arguable for exactly that reason. A rubric that only asks "is the fix right?" would accept it. My `bounded-scope` check reads every numbered item under "Proposed changes", not only the scope line. The grader's evidence was: "Items 2-5 (node-fetch to undici migration, new Settings field, sync-status UI fix 'while in the area', retry-with-backoff wrapper) are not needed for the reported timeout bug." My pass condition says a plan fails on added work "even when the core fix inside it is correct", so the correct item 1 didn't save it.

**Check rationale**

`| diagnosis-grounded | The plan's stated cause (Diagnosis / Cause / Summary) read against every step, timing, artifact, and control run in the Repro evidence block, and against the Expected/Actual lines there. | Pass if the stated cause is consistent with every result in the repro evidence, including every control run. Fail if any repro step or control run shows the bug's behavior with the blamed component absent, disabled, or bypassed, or shows the blamed component working correctly under the bug's conditions, or shows the failure already present at a stage before the blamed component runs. Fail if the plan adopts a cause from the thread or issue that the repro evidence contradicts. Agreeing with a confident thread comment is never enough on its own; the repro evidence decides. | required |`

This check grew out of my group's Phase 1 failure on calib-03. The sample rubric's diagnosis check, "passes if the plan says what causes the bug", passed calib-03 because the plan did state a cause, the key-binding one copied from a thread commenter. But the repro's step 3 shows the 26 s cost with no pager in the loop at all. Our Phase 2 draft, "Is_reproducable … passes if repro evidence corresponds to issue", still graded the repro instead of the plan's cause against the repro. So I rewrote the check to compare the stated cause against each repro step and control run. I also spelled out the three ways a control can rule a cause out: the bug appears with the blamed part absent; the blamed part works under the bug's conditions; or the failure is already there before the blamed part runs. Each one matches a wrong-cause package: calib-03 (no pager), pkg-07 and pkg-11 (the blamed part working in a control), and pkg-16 (zeros already gone before the cast). The last sentence is there because calib-03's plan says "As identified in this thread": a confident NONE commenter is not evidence.

**Trade-offs**

The check trusts the repro evidence completely, so it misses wrong causes when the repro has no control run that tests the blamed component. A plan that names the wrong function in a package whose repro only shows "it crashes" passes, because nothing in the evidence contradicts it. I accept that miss: the alternative would be letting the grader judge a cause from its own code knowledge, and that is the "Claude's own opinion" grading the rubric is supposed to replace. It can also over-reject. A noisy or partial control that looks like it "shows the bug with the blamed component bypassed" fails a correct plan. pkg-14 is the nearby case: its cache control ("the next attach is clean, the one after leaks again") could be read as contradicting the reattach-handshake cause. It passed in my run because the plan explains that control ("The cache control fits…"), and nothing else changed elsewhere: all 7 clear-accepts still agree in the saved 20/20 run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
