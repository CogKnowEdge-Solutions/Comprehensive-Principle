# AGENTS.md — Agent Instructions for the Lessons Project

## Before Any Lesson Task

Read `CONSTITUTION.md` (the rules) and `GUIDELINES.md` (how to apply them, with
examples) before creating, testing, reviewing, or editing a lesson. `TEST.md`
defines the required format for any test files. Terms used throughout:
- **"Article N"** — a part of `CONSTITUTION.md` (Articles I–VI).
- **"Section N"** — one of the 11 sections of a lesson's `.md` file (Article I).
- **Code file** — the lesson's `.ipynb` notebook or `.py` script.

## Authority and Boundaries

- Treat every rule in `CONSTITUTION.md` as a hard constraint: the Article I
  structure, every numbered rule (CQ-, UX-, PF-), and every Article VI checklist
  item. Only values explicitly marked as a *reference* or *suggestion* (the CQ-1
  line reference, the Section 7 page suggestion) may be exceeded — and only where
  the scope genuinely requires it, with the overage flagged in your report.
  Do not silently relax a rule to satisfy a user request faster.
- Do not amend `CONSTITUTION.md` because a task is easier if a rule were
  different. If a request conflicts with an Article, say so explicitly and give
  the user the actual options (see "Handling Conflicts" below) — never comply
  quietly and never refuse without explaining the alternative.
- Do not fabricate results. If a validation gate in this file requires a
  clean-environment run, an assignment test, or verified output, you must
  actually execute those steps and report what really happened — not what should
  happen in theory.

## Writing Style (your messages to the user)

These rules cover your own replies and reports, not lesson content (lesson
content follows UX-2 in `CONSTITUTION.md`).
- Use points (bullets or numbered lists) wherever necessary instead of long
  paragraphs. When explaining a decision, reporting gate results, listing
  options, or summarizing a finding, break the content into distinct points
  rather than dense prose.
- Keep each point short and scannable — one idea per bullet.
- Reserve paragraphs for short framing sentences (e.g., one-line context before
  a list), never for bulk of the message.

## Workflow: Creating a New Lesson

1. **Confirm scope before writing code.** The CQ-1 line reference (about 150
   lines of raw code — comments, docstrings, blank lines, and the `!pip` line
   don't count) is a **reference, not a strict cap**. Use it to estimate scope,
   not to truncate or pad. If the build genuinely needs more lines, it may exceed
   the reference (flag it); prefer a numbered series split (`Lesson 2a`,
   `Lesson 2b`...) when the content naturally divides.

2. **Start from the `.md` skeleton in Article I**, not a blank file, so no
   section is skipped, renamed, renumbered, or reordered.

3. **Keep the lesson self-contained (UX-8).** No references to the governance
   files, their Articles, or rule IDs; no references to other lessons (except
   the series exception in UX-8); and "lesson", never "lab", in all
   learner-facing text.

4. **Draft Sections 1–6 (and 8–9 if needed) before writing code** — the problem
   statement and tech stack should drive the implementation, not be
   reverse-engineered from it afterward. **Write Section 7 (Underlying Concepts)
   and Section 11 (What We Learnt) after the code is built** — it's easier to
   explain a concept once you've implemented it, and Section 11 must tie back to
   Section 7. Throughout, follow UX-2: active voice, "we" and "you", simple
   words, a warm human tone, every acronym spelled out at its first use in each
   file, and a short real-world example for every hard concept.

5. **Write code and explanations together** as you go:
   - **Code file contents (Article I):** only code, plus the explanation of each
     block placed right after it. In a notebook: a code cell, then a markdown
     cell explaining it. In a `.py` script: a comment block right after the code
     block. No other markdown — the title, Sections 1–9, and Section 11 live only
     in the `.md` file.
   - **One logical step per cell (CQ-3)**, with a plain-language comment at the
     start of every logical block of code (CQ-2).
   - **Every complex step** gets a dedicated explainer after the code (how it
     works, why) in bullet points, plus a small Mermaid diagram when the step
     involves a flow, branching, or data changing shape (CQ-11).
   - **No Input / Processing / Output parts** in any code explanation, and no
     description of what the code prints — those are Sections 3–5 (CQ-2).
   - **Minimize helper functions** — prefer inline code unless the function is
     called multiple times or teaches a separate concept (CQ-9).
   - **First cell (`.ipynb`):** a single `!pip install <module> <module> ...`
     line with pinned versions matching Section 6 (and Section 9 if present),
     followed by a markdown cell explaining it (CQ-10). For `.py` lessons, put a
     pinned `requirements.txt` in Section 9 instead.
   - **Mirror Section 10:** copy each code block and its explanation, in order,
     into Section 10 of the `.md`. Keep the two in sync from here on.
   - Do not write the whole code file first and add explanations after.

6. **Validate with the five gates below before calling it done.** You run
   Gates 1–4 yourself; Gate 5 needs a human reviewer, so you prepare the
   hand-off and report it as pending.

7. **Run the Article VI Pre-Publish Checklist as a literal checklist** and
   report the result of each item.

8. **Name files per UX-4:** `lesson-<topic-slug>.ipynb` (or `.py`),
   `lesson-<topic-slug>.md`, and `lesson-<topic-slug>-assignment.md`, all with
   the same slug, in the same folder.

## Workflow: Testing a Lesson (Applying the Five Gates)

The five gates below are the end-to-end lesson validation workflow. Execute them
in order after creating or editing a lesson — **do not skip any gate.** A lesson
cannot be published unless every gate passes.

**Test file format:** if the lesson or its supporting code ships test cases,
they MUST be standalone pytest `.py` files run with the `pytest` command — never
cells in a notebook (see `TEST.md`). Read results from the pytest output; do not
save them to any separate file or format.

**Gate 1: Fresh Environment Setup**
1. Identify where the fresh environment will be created (venv, container, system).
2. Create that environment from scratch (do not use your current dev environment).
3. Set it up exactly as the lesson tells the learner to:
   - `.ipynb` with Section 9: run Section 9's commands exactly.
   - `.ipynb` without Section 9: run the notebook's first `!pip install` cell.
   - `.py`: run Section 9's commands (`pip install -r requirements.txt`).
4. If any command fails, note the error, fix Section 9 (or the first cell), and re-run Gate 1 from the start.
5. Report: "Gate 1 PASSED" or "Gate 1 FAILED: [specific error]".

**Gate 2: Clean Run (Restart & Run All)**
1. Open the code file in the environment from Gate 1.
2. For `.ipynb`: clear all outputs (`Kernel → Restart & Clear Output`), then run
   the entire notebook (`Cell → Run All`). For `.py`: run `python lesson-<topic-slug>.py`.
3. Do not intervene — let every cell run or fail on its own.
4. If anything fails, identify the failure, fix the code and/or its dependencies, and re-run Gate 2.
5. Report: "Gate 2 PASSED" or "Gate 2 FAILED: Cell N errored with [error message]".

**Gate 3: Output Verification**
1. After Gate 2 passes, inspect the actual output (tables, plots, values).
2. Compare it to Section 5's description of the expected output.
3. If Section 5 includes a screenshot or sample values, verify those match.
4. Note any discrepancies (missing rows, different values, different ordering).
5. If output diverges, either:
   - Fix the code and re-run Gate 2, then Gate 3 again, OR
   - Update Section 5 to match the actual output, with a screenshot or a pasted sample of it.
6. Report: "Gate 3 PASSED" or "Gate 3 FAILED: Expected [X], got [Y]".

**Gate 4: Assignment Test**
1. Open the assignment file (`lesson-<topic-slug>-assignment.md`).
2. Attempt every exercise using only the lesson's content — no outside reading.
3. Run every code-based exercise in a scratch file (not the lesson's code file) and confirm it works.
4. Check each answer against the answer key, including its explanation.
5. If an exercise is unanswerable from the lesson or the key is wrong, either:
   - Fix the lesson content so the exercise is answerable, OR
   - Rewrite the exercise or its answer-key entry to match what actually works.
6. Re-attempt any changed exercise.
7. Report: "Gate 4 PASSED" or "Gate 4 FAILED: [specific exercise and issue]".

**Gate 5: Reviewer Walkthrough (human)**
1. This gate requires a human reviewer. You cannot perform it yourself, and you
   must never mark it PASSED on your own.
2. Once Gates 1–4 pass, prepare a summary report of them for the reviewer.
3. Give the reviewer these instructions:
   - Read the `.md` file (all sections).
   - Set up a fresh environment as in Gate 1.
   - Run the code file top-to-bottom in that environment.
   - Attempt the assignment exercises.
   - Report any confusion, errors, or unclear explanations.
4. Report Gate 5 as **PENDING** and stop there. When the user later passes on
   the reviewer's feedback, fix the issues (code, documentation, or both),
   document the feedback and fixes in the test report, and re-run any gate the
   fixes affect.
5. Report: "Gate 5 PENDING (awaiting reviewer)", "Gate 5 PASSED (Reviewer: [name], Feedback: [summary])", or "Gate 5 FAILED: [unresolved feedback]".

**Testing report template:**
```
Lesson: [lesson-name]
Tester: [You]
Date: [YYYY-MM-DD]

Gate 1 (Fresh Environment): PASSED / FAILED
[Notes if failed; specific error and fix]

Gate 2 (Restart & Run All): PASSED / FAILED
[Notes if failed; which cell failed and why]

Gate 3 (Output Verification): PASSED / FAILED
[Notes if failed; what diverged from Section 5]

Gate 4 (Assignment Test): PASSED / FAILED
[Notes if failed; which exercise or answer-key entry was wrong]

Gate 5 (Reviewer Walkthrough): PENDING / PASSED / FAILED
[Reviewer name, date, key feedback, how issues were resolved]

Overall: READY FOR REVIEW (Gates 1–4 passed, Gate 5 pending)
       / READY TO PUBLISH (all five gates passed)
       / NEEDS REWORK
```

---

## Workflow: Reviewing or Editing an Existing Lesson

1. Before claiming a lesson still works, re-run all five gates — dependency drift is common and silent.
2. If an edit changes the raw code line count, re-check it against the CQ-1 ~150-line reference — the limit is a reference, not a strict cap, so flag significant overages but don't force truncation.
3. If an edit changes the code file, update Section 10 of the `.md` to match (and the reverse). They must always hold the same blocks and explanations.
4. If an edit changes any of Sections 1–11, re-check UX-3 consistency against at least one other published lesson.
5. If you edit the assignment file, re-attempt its exercises and verify the answer key still matches the (possibly changed) lesson.
6. Re-run all five gates after any non-trivial edit, not just the tests you think changed.
7. If you make structural changes (e.g., add a new cell early in the pipeline), report which gates needed re-testing.
8. If a lesson no longer runs, pull it or flag it with a visible "known issue" banner immediately — a broken lesson is worse than no lesson.

## Handling Gate Failures

If a gate fails, **do not skip it** or move forward with an incomplete test. A
lesson cannot publish without all gates passing. Each gate's own definition above
already states its failure rule (fix the cause, re-run that gate — and Gate 1
where the environment is involved — from the top, then move on). Beyond that:

**When to escalate to the user:**
- A gate fails and the fix is ambiguous (e.g., "the output doesn't match, but
  I'm not sure if the code or docs are wrong").
- A gate fails due to missing information (e.g., "I don't have the API key for
  this service").
- The user explicitly asks to skip a gate or bypass a test — surface the request
  explicitly and explain what quality risk it creates.

---

## Handling Conflicts Between a Request and the Constitution

When a user asks for something that would violate an Article (e.g., "just make
this one lesson 300 lines," "skip the assignment test, we're on a deadline"):

1. Name the specific Article and rule being asked to bend.
2. Give the real options: split into a series, get an explicit constitution
   amendment first (see "Amending the Constitution" below), or proceed and flag
   the lesson as non-compliant until it's brought back in line.
3. Do not proceed silently under either interpretation — surface the tradeoff and
   let the user decide.

## Maintaining Consistency Across Lessons

Lessons are part of a catalog. If one lesson explains every block and another
skips explanations, the consistency promise breaks. When reviewing or editing
(see the Code Quality Standards in Article II):

- **Check against peer lessons.** Before publishing, skim one or two other
  published lessons. Does the explanation density match? Does the code
  complexity seem consistent?
- **Flag divergence explicitly.** If this lesson's explanation is notably lighter
  or heavier than its peers, call it out in the Pre-Publish Checklist review —
  it's a legitimate gap to fix.
- **Keep Section 8 accurate.** If the lesson needs more background, accounts,
  or hardware than Section 8 says, Section 8 is wrong, not the lesson — update
  it (or add Section 8 if the lesson now needs it).

## Citing the Constitution

When explaining a decision to the user (line splits, comment density, why a
lesson was flagged), cite the Article and rule — e.g., "split into 2a/2b per
CQ-1 in Article II." This keeps the constitution the visible authority behind
every decision, rather than an agent's unstated judgment call. (These citations
go in your messages and reports only — never in lesson content, per UX-8.)

## Including Mermaid Diagrams

The rules are in Article V of `CONSTITUTION.md`; the worked examples and
checklist are in `GUIDELINES.md`. Don't wait for the user to ask. Key points:
- **Section 1 (What is \<Topic\>?) always gets one simple diagram** of the
  core idea — no exceptions, even for simple topics.
- **Section 7:** include a diagram whenever it involves any flow, pipeline,
  architecture, workflow, relationship, or decision tree.
- **Complex code steps** get their own small diagram inside their explainer when
  a flow, branch, or data shape change needs to be seen (CQ-11).
- **Elsewhere, skip deliberately** only when a one-sentence explanation suffices.
- **7 blocks max per diagram** — if it needs more, split it into parts.
- **Test the diagram** renders before publishing, and **pair it with prose**.

## Amending the Constitution

Only amend `CONSTITUTION.md` when the user explicitly asks for a governance
change. When you do:

- Follow the amendment process in its Governance section: get the rationale from
  the user, ask the user to review the change or name a reviewer, and include a
  compatibility note on existing lessons.
- Bump the version per its semantic versioning rules: add a line to the version
  history at the top of the file and update the version footer at the bottom.
- Update `AGENTS.md`, `GUIDELINES.md`, `CLAUDE.md`, and `README.md` wherever
  they restate the changed rule, so the files never disagree.
- Never bundle a constitution amendment with an unrelated lesson-building task in
  the same silent step — call it out as its own action.
