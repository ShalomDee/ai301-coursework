# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

ShalomDee

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878033936
I’d like to work on this issue. I’ll reproduce the crash locally and post a report with my environment, steps, and observed output.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878037027
I set up the repository using `make setup` following `docs/SETUP.md` and reproduced issue #60 on Windows at commit `f89c06f`.

Environment:
- Windows 10.0.26200.9457
- Python 3.12.3
- pytest 9.1.1

Command used:

```.venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv --runxfail```

The test uses:
```
feedback = "Has Python skills"
context_chunks = [{"text": None}]
score = checker.check(feedback, context_chunks)
```

Expected behavior:
`FaithfulnessChecker.check()` should handle a context chunk whose text value is None without crashing and return a float between `0.0` and `1.0`.

Observed behavior:
```
E   TypeError: sequence item 0: expected str instance, NoneType found

rag\evaluator\faithfulness_checker.py:38: TypeError

FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
```
The feedback string differs from the issue example, but the failure occurs while constructing `context_text` before the scoring logic, so this does not change the reproduced behavior.

The exception occurs at:
```context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])```


Running the test without `--runxfail` reports the existing regression test as `XFAIL`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

18/20 → 19/20 → 20/20

**Package analysis**

`pkg-20`: In my 19/20 run, my rubric returned `accept` while the gold label was `reject`. The package belonged to the disclosure category, and my original `repo-conventions` check mentioned required AI disclosure but did not make the absence of that disclosure an explicit failure condition. I revised the check so that when a repository requires AI-assistance disclosure, missing that disclosure fails the package. After the revision, my rubric returned `reject`, matching the gold label.

**Check rationale**

`| repo-conventions | The claim comment and repro report, read against the repo-facts block and any applicable CONTRIBUTING, README, AGENTS.md, issue template, code-of-conduct, or stated disclosure policy identified in the evidence guide. | Pass if the comments follow the repository's stated contribution and communication requirements, including any required AI-assistance disclosure, and do not omit a convention that the repo explicitly requires for this kind of contribution. When the repo explicitly requires disclosure of AI assistance, pass only if the claim comment or repro report contains an explicit statement that AI assistance was used and describes that assistance to the level required by the repo's policy. If the repo requires disclosure and no such statement appears, fail rather than mark unclear. | required |`

I revised this check because my earlier wording recognized disclosure requirements but was not specific enough about how to grade a package that omitted the required disclosure. I initially made the revision too strict by requiring the specific AI tool to be named, which caused `pkg-07` to incorrectly reject even though its disclosure satisfied the repository policy. I changed the check again so that it follows the level of disclosure the repository actually requires rather than inventing an additional requirement.

**Trade-offs**

Making the disclosure rule more explicit fixed `pkg-20`, but my first revision was too strict and caused `pkg-07`, a valid clear-accept package, to reject. I used `pkg-07` as a canary alongside `pkg-20` and revised the check so it requires disclosure only to the level stated by the repository's own policy. The final full run returned 20/20, showing that the change fixed the disclosure case without changing the expected result for `pkg-07`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
