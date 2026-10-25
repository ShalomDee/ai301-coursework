# Contribution: Zotero page UX: smoother collection selection and friendlier sync feedback (slice 3: group selection inline)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Shalom Donga
**GitHub Username:** ShalomDee
**Issue:** https://github.com/LearningCircuit/local-deep-research/issues/4961
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built my issue-scout and improved how it checks maintainer support for outside contributors**

I built an issue-scout that searches GitHub using my fit profile, ranks the results, and grades the top five against my rubric. My first scan surfaced brand-new repositories, hackathon projects, and bounty issues because the rubric did not check whether projects were established. I added a repository-quality check and refined it to require two recently active human maintainers and evidence that they reviewed or merged outside contributions, rather than relying on stars or template files. Grading 17 candidates from the Issue Selection site then produced two accepted picks, kaoto#2451 and p5.js-website#705.

**Unit 6: Set up a voice guard and documented the local-deep-research repo**

I combined my three voice guides into one, added rules for claiming issues, asking questions, and following up, and set up the voice guard as a Claude Code hook. My draft issue claim passed both the manual voice check and repro-check. The guard initially had problems on Windows because it used python3, couldn't find the claude command, and only checked Bash commands, leaving PowerShell unchecked. I fixed these problems and tested both Bash and PowerShell, confirming that the guard blocked posts through either tool. I also found that the guard gave different results for the same claim, so I rewrote it to focus on my next step and checked it twice. Finally, I created CLAUDE.local.md with the repo's contribution rules, the Zotero group-picker code and tests, Windows setup instructions, and a warning that the owner's open PR #6762 changes the same page.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My scout found no acceptable issues in four searches. In each search, I refined my scout, and when I was satisfied, I tested it directly on issues from the Issue Selection site. Two passed: KaotoIO/kaoto#2451 and processing/p5.js-website#705. A close runner-up was local-deep-research#4961. After updating my scope rules, I checked both again, along with local-deep-research#4961. Kaoto was rated **ambitious** because restoring the saved schema requires working with `.kaoto` metadata and `DataMapperMetadataService`, not just React state. p5 passed the technical checks, but two people had already asked to work on it and another had opened a PR, so it was unavailable under the course rules.

**My scout rejected #4961, but I chose it anyway.** The issue contains five separate tasks, so it failed the scope check. It also failed the unclaimed check because other contributors had taken tasks 2 and 5. However, on October 24, the owner confirmed that task 3, adding inline group selection, was still unclaimed. The owner also stated that each numbered task could be submitted as its own PR, and the repo's CONTRIBUTING.md allows contributors to claim issues directly. The scout marked the repo as unresponsive because four outside issues from mid-October had gone unanswered, although the owner responded to this issue on the same day.

I rated task 3 **moderate** in difficulty. The owner identified the relevant file and existing design pattern, but I still need to work around the page's autosave rules and update two test files.

This issue fits my interests in frontend development and interaction design. The task is to replace a group-selection card at the bottom of the page with a dropdown beside the relevant field, so users no longer change settings that are hidden in Advanced. It also gives me experience working with an established repo's conventions, including the `esc()` XSS helper, `ldr-` CSS prefix, autosave logic, pre-commit checks, Puppeteer tests, and vitest.

**The main limitation I found is that my scout grades entire issues, not individual tasks.** It rejected #4961 because other tasks were claimed and the overall issue was too broad, even though task 3 was available and had a clear scope. The scope update didn't solve this because the scout still sees the issue as one large task. I would improve it by allowing individual tasks identified by maintainers to be graded separately. I would also make the claim check stricter, since it accepted p5 despite earlier requests to work on it.

**Claim comment:** https://github.com/LearningCircuit/local-deep-research/issues/4961#issuecomment-6391873146

**Toolkit**

- **Tool:** `issue-scout`, used to grade Issue Selection site picks and recheck my top choices after the Unit 6 scope update.
- **Results:** Rejected #4961 on `scope-feasible`, `unclaimed`, and `responsive`, although all repo-level checks passed. Accepted kaoto#2451 (ambitious) and p5.js-website#705.
- **Challenges:** The scout cannot distinguish individual tasks within a larger issue. It also missed earlier assignment requests on p5 because its claim check only considers statements of active work from the past 30 days.
- **Changes:** Replaced `scope-bounded` with `scope-feasible`, added difficulty ratings (straightforward, moderate, ambitious), and recorded the changes in `scan-notes.md`. Next, I would add task-level grading and treat unanswered assignment requests as claims.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Windows 11 Home (10.0.26200), Python 3.13.5, Node 22.22.2, and local-deep-research 2.0.0 at upstream `main` commit `0ec4d2998` (2026-10-24), cloned from my fork (ShalomDee/local-deep-research). The project's docs use pdm, which I don't have, so I created a venv and ran `.venv/Scripts/python -m pip install -e . --extra-index-url https://download.pytorch.org/whl/cpu`. I expected `sqlcipher3` to be the hard part on Windows, but a prebuilt 0.6.4 wheel installed cleanly. pytest isn't in the main dependencies, so I installed it separately. Then I ran `npm ci` and `npm run build` in the repo root, and `npm ci` plus `npm run install-browsers` in `tests/ui_tests` (Puppeteer 25.16, Chrome 156). I started the server with `LDR_DISABLE_RATE_LIMITING=true PYTHONUTF8=1 .venv/Scripts/python -m local_deep_research.web.app`. Baseline before any change: `PYTHONPATH=src .venv/Scripts/python -m pytest tests/research_library/zotero/ -q` gave 157 passed, 7 skipped; `npx vitest run tests/js/pages/zotero-runtime.test.js` gave 5 passed; and `node tests/ui_tests/test_zotero_integration_ci.js` gave 17 of 17 passed. I hit two setup problems. The first page load showed "Frontend assets have not been built" until I ran `npm run build`. My first repro script failed with "Cannot find module 'puppeteer'", because Puppeteer is declared in `tests/ui_tests/package.json`, not the root one.

**Steps to Reproduce**

1. Start the server, register a user, and open `/library/zotero`.
2. In Advanced options, tick "Enable Zotero integration" and "Use the local desktop app API" (autosave), then reload, so "List groups" is no longer disabled.
3. Look at the Library row (`#zt-library-type` select plus `#zt-library-id` input) and where the group list appears. I have no Zotero web API key, so I stubbed only `GET /library/api/zotero/groups` with the shape the real route returns: `{"success": true, "groups": [{"id": "5551234", "name": "Lab reading group"}, {"id": "7778899", "name": "Thesis sources"}]}`. With a real key, no stub is needed.
4. Click group `5551234` in the "Zotero groups" card.
5. Observed: the group list is a separate card outside the config form, about 520px below the Library field. The Library row has no group dropdown. Clicking the group rewrites `zt-library-type` to `group` and `zt-library-id` to `5551234` inside Advanced options and autosaves ("Zotero settings saved.").

**Reproduction Evidence**

- **Commit showing reproduction:** none (the reproduction needs no code change; it ran on unmodified upstream `main` at `0ec4d2998`)
- **Screenshots/logs:** output from my Puppeteer script, which reuses the repo's `tests/ui_tests/test_lib` login helper:

  ```
  BEFORE: "libraryTypeInsideAdvanced": true, "libraryRowHasGroupSelect": false,
          "groupsCardIsOutsideForm": true, "libraryType": "user", "libraryId": "",
          "groupsCard": {"top": 1382}, "libraryTypeField": {"top": 859}, "groupRowsInCard": 2
  AFTER CLICKING GROUP 5551234 IN BOTTOM CARD:
          "libraryType": "group", "libraryId": "5551234", "advancedOpen": true, "savedBannerShown": true
  ```

  Two full-page screenshots: one at page load, and one after the click showing the Library row set to Group / 5551234 next to the saved banner.
- **My findings:** the behavior slice 3 describes is real on today's `main`. The page's own hint even sends users away from the field: "For a group library, use 'List groups' below to find its ID." Two things matter for the fix. First, the group list needs a configured web API key, so local-API mode can't list groups, and the new dropdown needs a sensible empty or error state. Second, `test_zotero_integration_ci.js` currently asserts that `#zotero-groups-card` exists and is hidden on load, and `zotero-runtime.test.js` mocks the groups endpoint, so both tests have to change with the markup. I also found that the owner's open PR #6762 edits `zotero.html` and both test files (not the group code), so I'll rebase before opening my PR.

**Toolkit**

- Tool: repro-check (my unit 2 skill, now running with the wild scope). I graded my claim alone, then the claim and repro report together, with `claude "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue ..."`.
- What it produced: the claim alone got accept. On the first combined run, the claim was ready but the report got **reject**: `steps-followable` failed because "Step 4 needs groups in the card, but the report only says the script 'stubs GET /library/api/zotero/groups'; neither repro_groups.js nor the stub response body is in the post". After I revised, all five checks passed and the verdict was accept.
- Difficulties: the first draft relied on a file next to the report that no reader would have. repro-check also caught that my excerpt left out a `"banner": ""` field that contradicted my "banner appeared" claim. My script's selector had missed the banner, even though the screenshot showed it.
- What changed: I put the stub's response JSON and a condensed script in the report, fixed the banner check and re-ran (`savedBannerShown: true`), ran the UI contract test to close an open "not yet checked" item, trimmed the claim's date line so it only covers my next report, and rewrote the claim's middle paragraph around my real next step, because the voice guard stopped the old implementation sentence and the reproduction was already done.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
