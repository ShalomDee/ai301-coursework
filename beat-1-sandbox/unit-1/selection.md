# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60**

[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
● All three candidates are in the scoped repo (codepath/pathreview-ai301-fa26-s1), so all three are gradable. Graded live against today, 2026-09-21. Shared repo-level evidence: not
  archived, last push f89c06f 2026-09-16 (5 days ago) by human collaborator Aburke225; no AI ban in docs/CONTRIBUTING.md, the PR template, or the README; the repo has zero pull requests
  total, so nothing can be claimed by a linked PR.

  Ranked read-out — all three accepted

  1. #60 — Faithfulness checker crashes on text: None (best fit)
  Tightest scope of the three: one function (check() in rag/evaluator/faithfulness_checker.py), a copy-pasteable repro that names the exact exception, and a named failing test
  (test_none_context_chunk_text). Pure debug-existing-behavior work in Python — squarely what your profile says you're comfortable with, with no design judgment needed.

  2. #56 — Structural chunker drops heading-less documents
  Equally crisp bug report with a repro and a named failing test (test_document_with_no_headings), single file. Ranked below #60 only because the body leaves the fix approach open
  ("chunked as a single block or falling back to another strategy") — you'd pick one. The rubric explicitly treats candidate approaches as still bounded, and your profile saysyou're fine
  with that, but it's one more decision than #60.

  3. #18 — Repo analyzer never receives a file list
  Still accepted and a genuinely good fit for someone comfortable reading unfamiliar code: the issue names the root cause, both files to touch (agent/tools/github_tool.py,
  ingestion/parsers/repo_analyzer.py), and the missing key (file_structure). It ranks last because it's an enhancement rather than a bug, spans two modules, and the issue itself estimates
  2–4 hours — the largest surface of the three, with no repro snippet or named failing test to anchor "done."

  Per-check grades (identical across the three except where noted): maintainer-alive pass, repo-active pass, scope-bounded pass, unclaimed pass, ai-policy pass, responsive pass (preferred
  — issue #52 drew a COLLABORATOR reply 6 days after opening).

  One note on the house rule: it didn't come into play. None of the three has a single comment, so there were no classmate claims to ignore. Worth claiming quickly — #68 and #69 both
  picked up student claims in the last two days.

  Also worth carrying into the PR: docs/CONTRIBUTING.md says seeded bugs carry @pytest.mark.xfail(strict=True) markers you must delete as part of the fix — it names issue #56's marker
  verbatim. That applies to #60 and #56; #18 is an enhancement and may not have one.

  [
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 main commits all authored by human collaborator Aburke225; newest f89c06f dated 2026-09-16, 5 days before
  2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence": "repo API: archived=False,pushed_at=2026-09-16T21:48:27Z, 5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass", "evidence": "One bug in one function: \"check() builds the context with chunk.get(\\\"text\\\", \\\"\\\")\" plus a runnable repro and
  named failing test test_none_context_chunk_text"},
        {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; comments: 0;timeline has only 4 'labeled' events, no cross-references; repo has 0 PRs in any state"},
        {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and README contain no restriction on AI-assisted contributions; no
  AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence": "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 main commits all authored by human collaborator Aburke225; newest f89c06f dated 2026-09-16, 5 days before
  2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence": "repo API: archived=False,pushed_at=2026-09-16T21:48:27Z, 5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass", "evidence": "One coherent goal in StructuralChunker.chunk(); naming two candidate fixes (\"single block or falling back to another
  strategy\") is allowed by the rubric, and a failing test test_document_with_no_headings is named"},
        {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; comments: 0;timeline has only 4 'labeled' events, no cross-references; repo has 0 PRs in any state"},
        {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and README contain no restriction on AI-assisted contributions; no
  AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence": "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 main commits all authored by human collaborator Aburke225; newest f89c06f dated 2026-09-16, 5 days before
  2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence": "repo API: archived=False,pushed_at=2026-09-16T21:48:27Z, 5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass", "evidence": "One coherent goal (populate repo_data[\"file_structure\"] so _detect_tests/_detect_ci work) with two named files and
  AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence": "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    }
  ]
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ claim issue 60 and start working on it
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
      "verdict": "accept"
    }
  ]

✻ Cogitated for 4m 52s

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ claim issue 60 and start working on it
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏸ manual mode on · ? for shortcuts · ← for agents
                                              ✘ Auto-update failed: claude.exe in use (close other Claude Code sessions, i
  noted): maintainer-alive pass, repo-active pass,
  scope-bounded pass, unclaimed pass, ai-policy pass,
  responsive pass (preferred — issue #52 drew a COLLABORATOR
  reply 6 days after opening).

  One note on the house rule: it didn't come into play. None
  of the three has a single comment, so there were no
  classmate claims to ignore. Worth claiming quickly — #68
  and #69 both picked up student claims in the last two days.

  Also worth carrying into the PR: docs/CONTRIBUTING.md says
  seeded bugs carry @pytest.mark.xfail(strict=True) markers
  you must delete as part of the fix — it names issue #56's
  marker verbatim. That applies to #60 and #56; #18 is an
  enhancement and may not have one.

  [
    {
      "item": "https://github.com/codepath/pathreview-ai301-f
  a26-s1/issues/60",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass",
  "evidence": "Last 5 main commits all authored by human
  collaborator Aburke225; newest f89c06f dated 2026-09-16, 5
  days before 2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence":
  "repo API: archived=False, pushed_at=2026-09-16T21:48:27Z,
  5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass",
  "evidence": "One bug in one function: \"check() builds the
  context with chunk.get(\\\"text\\\", \\\"\\\")\" plus a
  runnable repro and named failing test
  test_none_context_chunk_text"},
        {"name": "unclaimed", "grade": "pass", "evidence":
  "assignees: []; comments: 0; timeline has only 4 'labeled'
  events, no cross-references; repo has 0 PRs in any state"},
        {"name": "ai-policy", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and
  README contain no restriction on AI-assisted
  contributions; no AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence":
  "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR
  reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-f
  a26-s1/issues/56",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass",
  "evidence": "Last 5 main commits all authored by human
  collaborator Aburke225; newest f89c06f dated 2026-09-16, 5
  days before 2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence":
  "repo API: archived=False, pushed_at=2026-09-16T21:48:27Z,
  5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass",
  "evidence": "One coherent goal in
  StructuralChunker.chunk(); naming two candidate fixes
  (\"single block or falling back to another strategy\") is
  allowed by the rubric, and a failing test
  test_document_with_no_headings is named"},
        {"name": "unclaimed", "grade": "pass", "evidence":
  "assignees: []; comments: 0; timeline has only 4 'labeled'
  events, no cross-references; repo has 0 PRs in any state"},
        {"name": "ai-policy", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and
  README contain no restriction on AI-assisted
  contributions; no AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence":
  "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR
  reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-f
  a26-s1/issues/18",
      "checks": [
        {"name": "maintainer-alive", "grade": "pass",
  "evidence": "Last 5 main commits all authored by human
  collaborator Aburke225; newest f89c06f dated 2026-09-16, 5
  days before 2026-09-21 (<=180)"},
        {"name": "repo-active", "grade": "pass", "evidence":
  "repo API: archived=False, pushed_at=2026-09-16T21:48:27Z,
  5 days before capture (<=365)"},
        {"name": "scope-bounded", "grade": "pass",
  "evidence": "One coherent goal (populate
  repo_data[\"file_structure\"] so _detect_tests/_detect_ci
  work) with two named files and \"Estimated effort: 2-4
  hours\"; no TBD, deferral, or unset boundary"},
        {"name": "unclaimed", "grade": "pass", "evidence":
  "assignees: []; comments: 0; timeline has 5 'labeled'
  events plus one 'renamed' on 2026-09-16, no
  cross-references; repo has 0 PRs in any state"},
        {"name": "ai-policy", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and
  README contain no restriction on AI-assisted
  contributions; no AI_POLICY.md in repo root"},
        {"name": "responsive", "grade": "pass", "evidence":
  "Sampled issue #52 (opened 2026-09-10) got a COLLABORATOR
  reply from Aburke225 on 2026-09-16, 6 days later (<=30)"}
      ],
      "verdict": "accept"
    }
  ]

✻ Cogitated for 4m 52s

─────────────────────────────────────────────────────────────
❯ claim issue 60 and start working on it
  then #18. Next: claim #60 and open a PR. (disable recaps in /config)

────────────────────────────────────────────────────────────────────────────────────────
❯ claim issue 60 and start working on it                             
  2026-09-10) got a COLLABORATOR reply from Aburke225 on 2026-09-16, 6 days later
  (<=30)"}
      ],
      "verdict": "accept"
    }
  ]

✻ Cogitated for 4m 52s

※ recap: Goal was grading three candidate first issues in the CodePath PathReview repo
  against your rubric. All three passed every required check and were ranked #60, #56,
  then #18. Next: claim #60 and open a PR. (disable recaps in /config)

```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

15/20 → 17/20 → 16/20 → 20/20 → 16/20 → 17/20 → 20/20

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

issue-19: In an earlier run, my rubric returned `reject` while the gold label was `accept` because `scope-bounded` failed. The check was treating multiple possible causes and suggested fixes as evidence that the issue was unbounded, even though they were alternative approaches to one bug. I revised the check to distinguish multiple implementation approaches from multiple independent tasks. In my final run, the rubric returned `accept`, matching the gold label.

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

`| scope-bounded | Issue body, comments, linked/closed PRs, opener role, labels | Issue states one problem or one coherent goal with an actionable path to a fix, even if it names multiple causes, files, or candidate approaches. Fail only when: (a) it bundles multiple independent, separately-assignable tasks (e.g. a tracking list of unrelated issue numbers); (b) it's a feature request still awaiting a product/design decision, shown by concrete unresolved specifics like a missing asset, unset boundaries, or explicit deferral ("TBD", "out of scope for v1"); or (c) it invites contributors to self-select an arbitrary, unspecified slice of ongoing work with no stated deliverable. A brief issue naming specific examples, even with a trailing "etc.", is still bounded. Repeated abandoned attempts fail it unless a maintainer has since fixed the scope | required |`

I wrote the check this way because earlier versions treated signs of complexity, such as multiple files or possible approaches, as signs that an issue was unbounded. The evaluation cases showed that those are not the same thing. The current check instead looks for specific reasons the contribution is actually open-ended, while allowing one coherent task to remain bounded even when there are several ways to implement it.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

Making `scope-bounded` more permissive toward multiple files, causes, and candidate approaches risks accepting an issue that appears concrete but is still underspecified. I kept explicit failure conditions for independently assignable tasks, unresolved product decisions, and unspecified self-selected work. The final 20/20 run confirmed that this fixed the false rejections without changing the expected outcomes of the scope-reject cases.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. Issue #60 fits both my interests and the time available because it involves Python, RAG evaluation, and debugging existing behavior, which are areas I already have experience with. It is also focused on one function with a reproducible failure and a named failing test, so the scope feels realistic for Unit 2.
2. The verdict correctly identified the issue as bounded and a strong fit because the failure is specific, reproducible, and does not require a product or design decision. Beyond the rubric, I also considered which accepted issue I would actually want to work on. Compared with #56 and #18, #60 gives me relevant RAG work while still leaving enough debugging and implementation work to be meaningful.
3. I do not anticipate much difficulty claiming it because it currently has no assignee, comments, or linked pull request. The main concern is timing because other students are selecting from the same issue pool, so it could be claimed before I reach the claiming step in Unit 2.]


---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
