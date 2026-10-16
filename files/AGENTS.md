# AGENTS.md — Agent Instructions for the Lessons Project

## Authority and Boundaries

- Treat every MUST/MUST NOT in `CONSTITUTION.md` as a hard constraint, not a
  suggestion. Do not silently relax one to satisfy a user request faster.
- Do not amend `CONSTITUTION.md` because a task is easier if a rule were
  different. If a request conflicts with an Article, say so explicitly and give
  the user the actual options (see "Handling Conflicts" below) — never comply
  quietly and never refuse without explaining the alternative.
- Do not fabricate results. If a validation gate in this file requires a
  clean-environment run, an assignment test, or verified output, you must
  actually execute those steps and report what really happened — not what should
  happen in theory.

## Writing Style

- Use points (bullets or numbered lists) wherever necessary instead of long
  paragraphs. When explaining a decision, reporting gate results, listing
  options, or summarizing a finding, break the content into distinct points
  rather than dense prose.
- Keep each point short and scannable — one idea per bullet.
- Reserve paragraphs for short framing sentences (e.g., one-line context before
  a list), never for bulk of the message.

## Workflow: Creating a New Lesson

1. **Confirm scope before writing code.** Line limits are a **reference, not a strict cap** (see CQ-1 / Article II in `CONSTITUTION.md`) — use them to estimate scope and calibrate explanation density, not to truncate or pad a notebook. If the build genuinely needs more lines, it may exceed the reference; prefer a numbered series split (`Lesson 2a`, `Lesson 2b`...) when the content naturally divides.

2. **Start from the 11-section structure in Article I**, not a blank file, so no section is skipped or reordered.

3. **Keep the content self-contained.** The governance files shape how you build
   it; they are not part of what the learner reads. In the notebook/script, the
   narrative `.md`, and the assignment file:
   - Do not reference `CONSTITUTION.md`, `AGENTS.md`, `GUIDELINES.md`, `TEST.md`,
     or their Articles and rule IDs (e.g., "per CQ-1", "see Article I").
   - Do not reference or draw on other lessons or their source material (e.g.,
     "as in Lesson 3", "see the previous lesson") — the content must stand on its own.
   - Do not use the word "lab" in learner-facing text (titles, headings, markdown,
     code comments, printed output, assignment text) — use "lesson" instead.
     File names still follow UX-4.

4. **Draft sections 1–9 first** (explanation of topic through environment setup) before writing
   code — the problem statement and tech stack should drive the implementation,
   not be reverse-engineered from it afterward. Write every learner-facing
   sentence in active voice, using "we" and "you", simple words, and a warm,
   human tone (UX-2 in `CONSTITUTION.md`; examples in `GUIDELINES.md`).

5. **Write code and explanations together** as you go: one logical step per
   cell, markdown explanation alongside each block, and a plain-language
   comment at the start of every logical block of code (CQ-2). **Every complex step gets a dedicated explainer** (input,
   what happens, output, why) in bullet points, plus a small Mermaid diagram
   when the step involves a flow, branching, or data changing shape (CQ-11). Minimize helper functions — prefer inline code unless the
   function is called multiple times or teaches a separate concept. Do not write
   the whole notebook first and add explanations after.
   **The first cell must be a single `!pip install <module> <module> ...` line**
   (pinned versions, matching Sections 6/9) so learners can install everything by
   running the first cell (CQ-10 in `CONSTITUTION.md`; practical notes in
   `GUIDELINES.md`). For `.py` script lessons, document `requirements.txt` in
   Section 9 instead.

6. **Validate per the five gates below before calling it done**:
   - Run the notebook top-to-bottom in a fresh kernel/environment built strictly
     from your own Section 9 instructions.
   - Attempt every exercise in the assignment file and verify the answer key.
   - Capture real output for Section 5 — do not describe expected output from
     memory.

7. **Run the Article VI Publish Gate as a literal checklist** and report the
   result of each item. Also confirm step 3 holds: no references to the
   governance files or other lessons, and no use of the word "lab" in
   learner-facing text.

8. **Name files per Article III (UX-4)** (see `CONSTITUTION.md`): each lesson ships a
   matching-slug `lesson-<topic-slug>.ipynb` OR `.py` plus `lesson-<topic-slug>.md`,
   and a `lesson-<topic-slug>-assignment.md` of knowledge-check exercises with an
   answer key (UX-7).

## Workflow: Testing a Lesson (Applying the Five Gates)

The five gates below are the end-to-end lesson validation workflow. Execute them in
order after creating or editing a lesson — **do not skip any gate.** A lesson cannot
pass validation if any gate fails. The gates are defined here in AGENTS.md; they
are no longer part of `CONSTITUTION.md` (see its version history).

**Where `TEST.md` fits:** `TEST.md` (the Comprehensive Testing Guide) is the
companion document for designing and running tests on any code the lesson or its
supporting files ship — general test-case design, the RED → GREEN → REFACTOR TDD
cycle, pytest commands, and troubleshooting. When a lesson has testable logic,
write its tests using TEST.md's framework; the gates below then validate the lesson
artifact itself (notebook/script + markdown) end to end. The two are
complementary, not alternatives.

**Test file format is non-negotiable:** Test cases MUST be authored in standalone
pytest `.py` files, never inside `.ipynb` notebooks — see TEST.md's "Mandatory
Test File Format" for the full rule and how to relocate tests found in a
notebook.

**Gate 1: Fresh Environment Setup**
1. Identify where the fresh environment will be created (venv, container, system).
2. Create that environment from scratch (do not use your current dev environment).
3. Copy Section 9's instructions exactly and execute them in the fresh environment.
   If the lesson omits Section 9 (it is optional for `.ipynb` lessons), run the
   notebook's first `!pip install` cell instead.
4. If any command fails, note the error, fix Section 9 (or the first cell), and re-run Gate 1 from the start.
5. Report: "Gate 1 PASSED" or "Gate 1 FAILED: [specific error]".

**Gate 2: Clean Run (Restart & Run All)**
1. Open the notebook in the environment from Gate 1.
2. Clear all outputs: `Kernel → Restart & Clear Output`.
3. Run the entire notebook: `Cell → Run All`.
4. Do not intervene — let every cell run or fail on its own.
5. If any cell fails, identify the failure, fix the cell and/or its dependencies, and re-run Gate 2.
6. Report: "Gate 2 PASSED" or "Gate 2 FAILED: Cell N errored with [error message]".

**Gate 3: Output Verification**
1. After Gate 2 passes, inspect the notebook's actual output (tables, plots, values).
2. Compare to Section 5's description of the expected output.
3. If Section 5 includes a screenshot or sample values, verify those match.
4. Note any discrepancies (missing rows, different values, different ordering).
5. If output diverges, either:
   - Fix the code and re-run Gate 2, then Gate 3 again, OR
   - Update Section 5 to match the actual output and include a screenshot.
6. Report: "Gate 3 PASSED" or "Gate 3 FAILED: Expected [X], got [Y]".

**Gate 4: Assignment Test**
1. Open the assignment file (`lesson-<topic-slug>-assignment.md`).
2. Attempt every exercise using only the lesson's content — no outside reading.
3. Run every code-based exercise in a scratch file (not the lesson notebook) and confirm it works.
4. Check each answer against the answer key, including its explanation.
5. If an exercise is unanswerable from the lesson or the key is wrong, either:
   - Fix the lesson content so the exercise is answerable, OR
   - Rewrite the exercise or its answer-key entry to match what actually works.
6. Re-attempt any changed exercise.
7. Report: "Gate 4 PASSED" or "Gate 4 FAILED: [specific exercise and issue]".

**Gate 5: Reviewer Walkthrough**
1. This gate requires a human reviewer. You cannot perform this gate yourself.
2. Prepare a summary report from Gates 1–4 and hand it to the reviewer.
3. Instruct the reviewer to:
   - Read the markdown file (all sections).
   - Follow Section 9 (or the first `!pip install` cell) to set up a fresh environment.
   - Run the notebook top-to-bottom in that environment.
   - Attempt the assignment exercises.
   - Report any confusion, errors, or unclear explanations.
4. Collect feedback from the reviewer.
5. Address feedback: fix the issues they found (code, documentation, or both).
6. Document the feedback and fixes in your test report.
7. Report: "Gate 5 PASSED (Reviewer: [name], Feedback: [summary])" or "Gate 5 FAILED: [unresolved feedback]".

**Testing report template:**
```
Lesson: [lesson-name]
Difficulty: [Beginner/Intermediate/Advanced]
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

Gate 5 (Reviewer Walkthrough): PASSED / FAILED
[Reviewer name, date, key feedback, how issues were resolved]

Overall: READY TO PUBLISH / NEEDS REWORK
```

**Recording pytest results to `.xlsx`:**

Every test run (any gate, any re-run) must also be saved as an `.xlsx` workbook so
the outcome is reviewable without re-running pytest. The markdown report above is
the human summary; the workbook is the machine-readable, per-test record. Use the
JUnit XML + `openpyxl` route (below) rather than a pytest plugin — it needs no
extra pytest dependencies and the `.xlsx` is fully under our control.

**Step 1 — emit machine-readable results with pytest's JUnit XML report:**

```bash
pytest test_agent.py -v --junitxml=test-results/junit-test_agent.xml
```

**Step 2 — convert to `.xlsx`** with `scripts/pytest_to_xlsx.py` (keep it in the
project root if there is no `scripts/` dir). Requires only `openpyxl`:

```bash
python scripts/pytest_to_xlsx.py test-results/junit-test_agent.xml test-results/test_agent_2026-08-10.xlsx
```

**Standard structure for the `.xlsx` entries:**

| Sheet | Column / Row | What goes here |
|-------|--------------|----------------|
| `Test Results` | `Test ID` | pytest node id, e.g. `test_agent.py::TestCalculatorTool::test_add_two_numbers` |
| | `Test Name` | the test method name, e.g. `test_add_two_numbers` |
| | `File` | module file, e.g. `test_agent.py` |
| | `Class` | test class, or blank if none |
| | `Status` | `PASSED` / `FAILED` / `ERROR` / `SKIPPED` (derive from JUnit XML) |
| | `Duration (s)` | per-test wall time from the XML `time` attribute |
| | `Failure Message` | first line of the failure/error message, blank if passed |
| `Run Summary` | Key/Value rows | `Total tests`, `Passed`, `Failed`, `Errors`, `Skipped`, `Pass rate (%)`, `Total duration (s)`, `Timestamp` |

One row per test case in `Test Results`, one row per metric in `Run Summary`.
File naming: `test_<slug>_<YYYY-MM-DD>.xlsx`. Reference script:

```python
"""pytest_to_xlsx.py — convert a pytest JUnit XML report to .xlsx.

Usage: python pytest_to_xlsx.py <junit-xml> <output.xlsx>
"""
import sys
import xml.etree.ElementTree as ET

from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill
from openpyxl.utils import get_column_letter

HEADER_FILL = PatternFill("solid", fgColor="DDEBF7")
STATUS_FILL = {
    "PASSED": PatternFill("solid", fgColor="C6EFCE"),
    "FAILED": PatternFill("solid", fgColor="FFC7CE"),
    "ERROR": PatternFill("solid", fgColor="FFC7CE"),
    "SKIPPED": PatternFill("solid", fgColor="FFEB9C"),
}


def status_of(testcase):
    if testcase.find("failure") is not None:
        return "FAILED"
    if testcase.find("error") is not None:
        return "ERROR"
    if testcase.find("skipped") is not None:
        return "SKIPPED"
    return "PASSED"


def message_of(testcase):
    for tag in ("failure", "error"):
        el = testcase.find(tag)
        if el is not None:
            first = (el.get("message") or el.text or "").strip()
            return first.splitlines()[0] if first else "(no message)"
    return ""


def autosize(ws):
    for col in range(1, ws.max_column + 1):
        width = max(len(str(ws.cell(row=r, column=col).value or ""))
                    for r in range(1, ws.max_row + 1))
        ws.column_dimensions[get_column_letter(col)].width = min(width + 2, 60)


def main(junit_path, out_path):
    root = ET.parse(junit_path).getroot()
    suites = list(root.iter("testsuite"))

    wb = Workbook()
    ws = wb.active
    ws.title = "Test Results"
    headers = ["Test ID", "Test Name", "File", "Class",
               "Status", "Duration (s)", "Failure Message"]
    ws.append(headers)
    for cell in ws[1]:
        cell.font = Font(bold=True)
        cell.fill = HEADER_FILL
    ws.freeze_panes = "A2"

    for suite in suites:
        for tc in suite.iter("testcase"):
            classname = tc.get("classname", "")
            parts = classname.split(".")
            file_, klass = parts[0], ".".join(parts[1:])
            status = status_of(tc)
            row = ws.max_row + 1
            ws.append([f"{classname}::{tc.get('name')}", tc.get("name"),
                       file_, klass, status,
                       float(tc.get("time", 0) or 0), message_of(tc)])
            ws.cell(row=row, column=5).fill = STATUS_FILL.get(status, HEADER_FILL)
    autosize(ws)

    total = sum(int(s.get("tests", 0)) for s in suites)
    failures = sum(int(s.get("failures", 0)) for s in suites)
    errors = sum(int(s.get("errors", 0)) for s in suites)
    skipped = sum(int(s.get("skipped", 0)) for s in suites)
    passed = total - failures - errors - skipped

    ws2 = wb.create_sheet("Run Summary")
    rows = [
        ("Total tests", total),
        ("Passed", passed),
        ("Failed", failures),
        ("Errors", errors),
        ("Skipped", skipped),
        ("Pass rate (%)", round(passed / total * 100, 2) if total else 0),
        ("Total duration (s)", round(sum(float(s.get("time", 0) or 0) for s in suites), 3)),
        ("Timestamp", suites[0].get("timestamp", "") if suites else ""),
    ]
    ws2.append(["Key", "Value"])
    for cell in ws2[1]:
        cell.font = Font(bold=True)
        cell.fill = HEADER_FILL
    for key, value in rows:
        ws2.append([key, value])
    autosize(ws2)

    wb.save(out_path)
    print(f"Wrote {out_path}")


if __name__ == "__main__":
    main(sys.argv[1], sys.argv[2])
```

---

## Workflow: Reviewing or Editing an Existing Lesson

1. Before claiming a lesson still works, re-run all five gates — dependency drift is common and silent.
2. If an edit changes line count, re-check it against Article II's reference line ceiling for the difficulty level — the limit is a reference, not a strict cap, so flag significant overages but don't force truncation.
3. If an edit changes any of Sections 1–11, re-check Article III (UX-3) consistency against at least one other published lesson at the same difficulty level.
4. If you edit the assignment file, re-attempt its exercises and verify the answer key still matches the (possibly changed) lesson.
5. Re-run all five gates after any non-trivial edit, not just the tests you think changed.
6. If you make structural changes (e.g., add a new cell early in the pipeline), report which gates needed re-testing.
7. If a lesson no longer runs, pull it or flag it with a visible "known issue" banner immediately — a broken lesson is worse than no lesson.

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
this one lesson 300 lines," "skip testing the exercise, we're on a deadline"):

1. Name the specific Article being asked to bend.
2. Give the real options: split into a series, get an explicit constitution
   amendment first (see Governance in `CONSTITUTION.md`), or proceed and flag the
   lesson as non-compliant until it's brought back in line.
3. Do not proceed silently under either interpretation — surface the tradeoff and
   let the user decide.

## Maintaining Difficulty Level Consistency

Lessons are part of a catalog. If one Beginner lesson explains every line and another
skips explanations, the consistency promise breaks. When reviewing or editing
(see `GUIDELINES.md` for the difficulty-level details):

- **Check against peer lessons.** Before publishing, skim one or two other lessons at
  the same difficulty level. Does the explanation density match? Does the code
  complexity seem consistent?
- **Flag divergence explicitly.** If this lesson's explanation is notably lighter or
  heavier than its peer difficulty level, call it out in the Publish Gate review
  — it's a legitimate gap to fix.
- **Calibrate the header line** to match the actual content, not the intended
  difficulty. If a lesson claims "Beginner" but requires deep domain knowledge, the
  header is wrong, not the lesson — update it.

## Citing the Constitution

When explaining a decision (line splits, comment density, why a lesson was flagged),
cite the Article by number — e.g., "split into 2a/2b per Article II's line
reference." This keeps the constitution the visible authority behind every
decision, rather than an agent's unstated judgment call.

## Including Mermaid Diagrams

**Default to including a Mermaid diagram** when Section 7 (Underlying Concepts)
involves any flow, pipeline, architecture, workflow, relationship, or decision
tree — don't wait for the user to ask. The rules are in `CONSTITUTION.md`
(Section 5) and the worked examples/checklist are in `GUIDELINES.md`. Key points:
- **Section 1 (Explanation of Topic) always gets one simple diagram** of the core idea, alongside bullet points and at least one everyday example.
- **Scan Section 7 as you draft it** and plan a diagram whenever the concept is visual.
- **Complex code steps in Section 10** get their own small diagram when a flow, branch, or data shape change needs to be seen (CQ-11).
- **Skip deliberately** only when a one-sentence explanation suffices.
- **7 blocks max per diagram** — if it needs more, split it into parts (see `GUIDELINES.md`).
- **Test the diagram** in markdown before publishing, and **pair it with prose**.
- Beginner lessons especially benefit from a visual pipeline — if in doubt, include one.

## Amending the Constitution

Only amend `CONSTITUTION.md` when the user explicitly asks for a governance
change. When you do:

- Follow the amendment process already defined in its Governance section
  (rationale, review, compatibility note on existing lessons).
- Bump the version per the semantic versioning rules already in that file.
- Update the Sync Impact Report comment at the top of the file.
- Never bundle a constitution amendment with an unrelated lesson-building task in
  the same silent step — call it out as its own action.
