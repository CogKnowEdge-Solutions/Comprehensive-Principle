# AGENTS.md — Agent Instructions for the Labs Project

This file tells an AI coding agent (Claude Code, or any agent working in this
repo) **how to act** on the rules in `CONSTITUTION.md`. The constitution is the
source of truth for *what* is required; this file is the operating procedure for
*how* an agent satisfies it. If the two ever conflict, `CONSTITUTION.md` wins —
update this file to match it, not the other way around.

## Authority and Boundaries

- Treat every MUST/MUST NOT in `CONSTITUTION.md` as a hard constraint, not a
  suggestion. Do not silently relax one to satisfy a user request faster.
- Do not amend `CONSTITUTION.md` because a task is easier if a rule were
  different. If a request conflicts with an Article, say so explicitly and give
  the user the actual options (see "Handling Conflicts" below) — never comply
  quietly and never refuse without explaining the alternative.
- Do not fabricate results. If Article III requires a clean-environment run, an
  Optional Exercise test, or verified output, you must actually execute those
  steps and report what really happened — not what should happen in theory.

## Workflow: Creating a New Lab

1. **Decide difficulty level first.** Before writing any code, determine whether
   this is a Beginner, Intermediate, or Advanced lab. This decision drives line
   limits, explanation density, code style, and library usage. If uncertain,
   ask the user or propose what you think and let them correct you.
   
2. **Confirm scope before writing code.** Given the difficulty level, estimate
   whether the build fits within the line ceiling:
   - Beginner: ≤110 lines
   - Intermediate: ≤150 lines
   - Advanced: ≤180 lines
   
   If it clearly won't fit, propose a split into a numbered series (`Lab 2a`,
   `Lab 2b`, ...) *before* writing code, not after hitting the limit.

3. **Start from the 12-section structure in Article I**, not a blank file, so no section is skipped or reordered.

4. **Draft sections 1–9 first** (title through environment setup) before writing
   code — the problem statement and tech stack should drive the implementation,
   not be reverse-engineered from it afterward. Include the difficulty header
   (e.g., "Difficulty: Intermediate | ~40 min | Requires Lab 3") under the title.

5. **Write code in the style matching the difficulty level** as you go: one logical
   step per cell, markdown explanation calibrated to the learner level, inline
   comments appropriate to the difficulty (heavy for Beginner, minimal for
   Advanced). Minimize helper functions — prefer inline code unless the function
   is called multiple times or teaches a separate concept. Do not write the whole
   notebook first and add explanations after.

6. **Validate per Article III before calling it done**:
   - Run the notebook top-to-bottom in a fresh kernel/environment built strictly
     from your own Section 9 instructions.
   - Actually perform the Optional Exercise (Section 11) and confirm it works.
   - Capture real output for Section 5 — do not describe expected output from
     memory.

7. **Run the Article VI Publish Gate as a literal checklist** and report the
   result of each item. Include a specific check: "Does the code complexity and
   explanation density match the stated difficulty level?" If not, note the gap.

8. **Name files per Article IV**: Use one of:
   - `lab-<topic-slug>.ipynb` + `lab-<topic-slug>.md` (for Jupyter notebooks)
   - `lab-<topic-slug>.py` + `lab-<topic-slug>.md` (for Python scripts)
   
   Both formats in same directory with matching slug.

## Workflow: Testing a Lab (Applying the Five Gates)

After creating or editing a lab, you must execute all five gates from `CONSTITUTION.md` Section 3 in order. **Do not skip any gate.** A lab cannot pass testing if any gate fails.

**Gate 1: Fresh Environment Setup**
1. Identify where the fresh environment will be created (venv, container, system).
2. Create that environment from scratch (do not use your current dev environment).
3. Copy Section 9's instructions exactly and execute them in the fresh environment.
4. If any command fails, note the error, fix Section 9, and re-run Gate 1 from the start.
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

**Gate 4: Optional Exercise Test**
1. Read Section 11 carefully. Understand exactly what modification is being asked for.
2. Edit the notebook to perform the modification (e.g., swap Weaviate for Milvus).
3. Run the modified cells.
4. Verify the output makes sense (it should be similar in structure to the original).
5. If the exercise fails, identify why and either:
   - Fix the lab code and Section 9 dependencies, OR
   - Rewrite Section 11 to reflect what actually works.
6. Re-perform the exercise with the fix.
7. Report: "Gate 4 PASSED" or "Gate 4 FAILED: [specific issue with the exercise]".

**Gate 5: Reviewer Walkthrough**
1. This gate requires a human reviewer. You cannot perform this gate yourself.
2. Prepare a summary report from Gates 1–4 and hand it to the reviewer.
3. Instruct the reviewer to:
   - Read the markdown file (all 12 sections).
   - Follow Section 9 to set up a fresh environment.
   - Run the notebook top-to-bottom in that environment.
   - Attempt the Optional Exercise.
   - Report any confusion, errors, or unclear explanations.
4. Collect feedback from the reviewer.
5. Address feedback: fix the issues they found (code, documentation, or both).
6. Document the feedback and fixes in your test report.
7. Report: "Gate 5 PASSED (Reviewer: [name], Feedback: [summary])" or "Gate 5 FAILED: [unresolved feedback]".

**Testing report template:**
```
Lab: [lab-name]
Difficulty: [Beginner/Intermediate/Advanced]
Tester: [You]
Date: [YYYY-MM-DD]

Gate 1 (Fresh Environment): PASSED / FAILED
[Notes if failed; specific error and fix]

Gate 2 (Restart & Run All): PASSED / FAILED
[Notes if failed; which cell failed and why]

Gate 3 (Output Verification): PASSED / FAILED
[Notes if failed; what diverged from Section 5]

Gate 4 (Optional Exercise): PASSED / FAILED
[Notes if failed; what went wrong with the exercise]

Gate 5 (Reviewer Walkthrough): PASSED / FAILED
[Reviewer name, date, key feedback, how issues were resolved]

Overall: READY TO PUBLISH / NEEDS REWORK
```

---

## Workflow: Reviewing or Editing an Existing Lab

1. Before claiming a lab still works, re-run all five gates — dependency drift is common and silent.
2. If an edit changes line count, re-check it against Article II's ceiling for the difficulty level.
3. If an edit changes any of Sections 1–12, re-check Article IV consistency against at least one other published lab at the same difficulty level.
4. Re-run all five gates after any non-trivial edit, not just the tests you think changed.
5. If you make structural changes (e.g., add a new cell early in the pipeline), report which gates needed re-testing.

## Handling Gate Failures

If a gate fails, **do not skip it** or move forward with an incomplete test. A lab cannot publish without all gates passing.

**If Gate 1 fails (fresh environment setup):**
- Identify the specific command or step that failed.
- Fix Section 9 to correct the issue.
- Re-run Gate 1 from the start (not from the failed step).
- Do not proceed to Gate 2 until Gate 1 fully passes.

**If Gate 2 fails (notebook run):**
- Identify which cell failed and why.
- Fix the cell or its dependencies (could be in an earlier cell, Section 9, or a library issue).
- Re-run Gate 1 to ensure the fresh environment still works.
- Re-run Gate 2 from the top.
- Do not proceed to Gate 3 until Gate 2 fully passes.

**If Gate 3 fails (output mismatch):**
- Decide: is the code right or the documentation wrong?
- If code is right, update Section 5 with actual output and screenshot.
- If documentation is right, fix the code.
- Re-run Gate 2, then Gate 3 again.
- Document the discrepancy and how it was resolved.

**If Gate 4 fails (optional exercise):**
- Identify what went wrong with the exercise.
- Either fix the lab code and dependencies, or rewrite Section 11 to reflect what actually works.
- Re-run the exercise to confirm it works.
- If the issue is significant, report it to the user with options for resolution.

**If Gate 5 fails (reviewer feedback):**
- Address the reviewer's feedback: fix code, documentation, or both.
- Document each piece of feedback and how it was resolved.
- Re-run the gates affected by your fixes.
- Report back to the reviewer for confirmation if major changes were made.

**When to escalate to the user:**
- A gate fails and the fix is ambiguous (e.g., "the output doesn't match, but I'm not sure if the code or docs are wrong").
- A gate fails due to missing information (e.g., "I don't have the API key for this service").
- The user explicitly asks to skip a gate or bypass a test — surface the request explicitly and explain what quality risk it creates.

---

## Handling Conflicts Between a Request and the Constitution

When a user asks for something that would violate an Article (e.g., "just make
this one lab 300 lines," "skip testing the exercise, we're on a deadline"):

1. Name the specific Article being asked to bend.
2. Give the real options: split into a series, get an explicit constitution
   amendment first (see Governance in `CONSTITUTION.md`), or proceed and flag the
   lab as non-compliant until it's brought back in line.
3. Do not proceed silently under either interpretation — surface the tradeoff and
   let the user decide.

## Maintaining Difficulty Level Consistency

Labs are part of a catalog. If one Beginner lab explains every line and another
skips explanations, the consistency promise breaks. When reviewing or editing:

- **Check against peer labs.** Before publishing, skim one or two other labs at
  the same difficulty level. Does the explanation density match? Does the code
  complexity seem consistent?
- **Flag divergence explicitly.** If this lab's explanation is notably lighter or
  heavier than its peer difficulty level, call it out in the Publish Gate review
  — it's a legitimate gap to fix.
- **Calibrate the header line** to match the actual content, not the intended
  difficulty. If a lab claims "Beginner" but requires deep domain knowledge, the
  header is wrong, not the lab — update it.

## Citing the Constitution

When explaining a decision (line splits, comment density, why a lab was flagged),
cite the Article by number — e.g., "split into 2a/2b per Article II's line
ceiling." This keeps the constitution the visible authority behind every
decision, rather than an agent's unstated judgment call.

## Recommending Mermaid Diagrams

When a lab's Section 7 (Underlying Concepts) is dense or hard to follow, suggest adding a Mermaid diagram:

- **Read Section 7.** If it has multiple paragraphs about how pieces connect, relationships, or workflows, offer to create a diagram.
- **Suggest, don't impose.** "This section explains the RAG pipeline in prose — would a diagram help? I can create one showing documents → chunking → embedding → retrieval → generation."
- **Test the diagram.** If you create a diagram, render it in markdown to confirm it displays correctly before suggesting it.
- **Pair diagram with prose.** Always ensure the prose explanation still comes first and the diagram supplements it (not replaces it).

Diagrams are optional but powerful for complex concepts. Beginner labs especially benefit from a visual pipeline or architecture diagram.

## Amending the Constitution

Only amend `CONSTITUTION.md` when the user explicitly asks for a governance
change. When you do:

- Follow the amendment process already defined in its Governance section
  (rationale, review, compatibility note on existing labs).
- Bump the version per the semantic versioning rules already in that file.
- Update the Sync Impact Report comment at the top of the file.
- Never bundle a constitution amendment with an unrelated lab-building task in
  the same silent step — call it out as its own action.
