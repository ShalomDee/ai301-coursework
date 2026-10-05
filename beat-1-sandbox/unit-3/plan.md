# Plan: issue #60, FaithfulnessChecker crashes when a context chunk has `text: None`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60
Builds on my unit 2 reproduction:
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878037027

## Repro evidence this plan relies on

From my posted reproduction (Windows 10.0.26200, Python 3.12.3, pytest 9.1.1,
commit `f89c06f`):

```
.venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv --runxfail
```

with `feedback = "Has Python skills"` and `context_chunks = [{"text": None}]`:

```
E   TypeError: sequence item 0: expected str instance, NoneType found

rag\evaluator\faithfulness_checker.py:38: TypeError

FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
```

The exception is raised at:

```
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
```

Without `--runxfail`, the same test reports `XFAIL` (it is marked
`xfail(strict=True)` for issue #60).

Control (same commit, re-run while planning): a chunk with the `text` key
missing entirely does not crash.

```
FaithfulnessChecker().check('Has Python skills', [{'content': 'Python skills'}])
-> 0.0
```

So the failure needs the `text` key to be present with the value `None`. A
missing key is already handled by the `""` default.

## Diagnosis

`chunk.get("text", "")` only falls back to `""` when the `text` key is
missing. When the key is present and its value is `None`, `.get()` returns
`None`, and `" ".join(...)` on line 38 of
`rag/evaluator/faithfulness_checker.py` raises `TypeError` on that `None`
item. This happens while the context string is being built, before any claim
scoring runs. That fits both the traceback (the error points at line 38) and
the control (a missing key gives `""` and scores normally).

## Scope

In scope: how `check()` builds `context_text` from chunks whose `text` is
`None`, plus the regression test for it.

Not in scope: claim extraction (`_extract_claims`), the overlap and stop-word
logic in `_is_supported` (that is issue #59 and its xfail markers stay as
they are), non-string `text` values other than `None`, other evaluators,
callers of `FaithfulnessChecker`, and any lint/format cleanup elsewhere in the
file.

## Files

- `rag/evaluator/faithfulness_checker.py`: line 38 only.
- `tests/unit/test_faithfulness_checker.py`: remove the
  `@pytest.mark.xfail(strict=True, reason="issue #60: ...")` marker from
  `test_none_context_chunk_text`, as `docs/CONTRIBUTING.md` requires for seeded
  bugs (with the marker in place, a passing test becomes `XPASS(strict)` and
  fails CI).

## Approach

1. Change line 38 to treat a `None` text the same as a missing key:
   `" ".join([chunk.get("text") or "" for chunk in context_chunks])`.
2. Delete the issue #60 xfail marker from `test_none_context_chunk_text`.
   Leave the issue #59 markers alone.
3. Check `pyproject.toml` for a lint/type suppression that names issue #60
   and remove it if there is one. (I checked while planning and found none.)

## Test plan

1. Re-run my unit 2 command without `--runxfail` (the marker will be gone):
   `.venv/Scripts/python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -vv`
   - Today: `FAILED ... TypeError: sequence item 0: expected str instance, NoneType found` (with `--runxfail`).
   - Expected after: `PASSED`. `check("Has Python skills", [{"text": None}])`
     returns `0.0`, a float in `[0.0, 1.0]`.
2. Direct call from the issue:
   `FaithfulnessChecker().check('Knows Python.', [{'text': None}])` returns
   `0.0` instead of raising `TypeError`.
3. Control stays the same: `[{'content': 'Python skills'}]` still returns `0.0`.
4. Whole file: `pytest tests/unit/test_faithfulness_checker.py -q` goes from
   `18 passed, 4 xfailed` to `19 passed, 3 xfailed` (the three #59 xfails
   stay).
5. `make lint`, `make typecheck`, and `make test-unit` pass.

## Risks and unknowns

- `None` text now counts as empty context, so a chunk with no text supports
  no claims. This matches how a missing `text` key already behaves, which
  makes it the consistent choice. Raising an error would also stop the crash,
  but it would break the existing test, which expects a float back.
- Other non-string `text` values (for example an int) would still break the
  join. The issue doesn't report that, so I'm leaving it out, as noted under
  Not in scope.
- PR #74 on this issue takes a similar approach. A classmate's plan or PR
  doesn't block mine under the course's house rules, but I'll read #74 before
  I open my own so I'm not duplicating its tests without saying so.

## Deviations

The fix and the marker removal went in exactly as planned (line 38 now reads
`" ".join([chunk.get("text") or "" for chunk in context_chunks])`, and the
issue #60 xfail marker is gone from `test_none_context_chunk_text`). The
`pyproject.toml` check also held: there was no #60 suppression to remove.

One small deviation, in `tests/unit/test_faithfulness_checker.py`: my first
commit attempt was rejected by the repo's pre-commit `mypy` hook. The hook
type-checks every staged file in full, and two lines that already existed in
that file (`context_chunks = []` in `test_empty_context_chunks_returns_zero` and
`test_both_empty_returns_zero`) failed with `Need type annotation for
"context_chunks" [var-annotated]`. I hadn't planned to touch those lines, and I
had listed "lint/format cleanup elsewhere in the file" as out of scope. I
didn't want to skip the hook, so I annotated just those two lines as
`context_chunks: list[dict] = []`. That's two type annotations with no change
in behavior, and the file's test results stay at `19 passed, 3 xfailed`. No
other lines in the file changed, and no other files were touched.

The test plan held: the repro test went from `FAILED ... TypeError` to
`PASSED`, the direct call and the missing-key control both return `0.0`, and
the file went from `18 passed, 4 xfailed` to `19 passed, 3 xfailed`. Unit
suite: `376 passed, 52 xfailed`. Note: locally, `make typecheck` stops on a
numpy stub inside `.venv` (`Type statement is only supported in Python 3.12 and
greater`). That comes from the local environment, not from this diff, and the
pre-commit mypy hook passes on both changed files.
