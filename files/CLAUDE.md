# CLAUDE.md — Using Comprehensive Principles with Claude Code

## Instructions for Claude Code

This project builds lessons under the Comprehensive Principles governance.
Before any lesson task — creating, testing, reviewing, or editing — follow
`AGENTS.md` (your operating procedures) and `CONSTITUTION.md` (the rules), both
loaded below. Read `GUIDELINES.md` for worked examples and `TEST.md` for the
test-file format when you need them.

@AGENTS.md
@CONSTITUTION.md

The rest of this file is a guide for the people using Claude Code on this
project.

---

## Quick Start: Fetch and Use in Claude Code

### Step 1 — Fetch the governance files

Choose one of these options:

**Option A — fetch all files in one command:**

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md -o CLAUDE.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

**Option B — fetch individual files:**

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md
curl -o AGENTS.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md
curl -o CONSTITUTION.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md
curl -o GUIDELINES.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md
curl -o TEST.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md
```

**Option C — clone the whole repo:**

```bash
git clone https://github.com/CogKnowEdge-Solutions/Comprehensive-Principle.git
cp Comprehensive-Principle/files/*.md your-lesson-project/
```

### Step 2 — Place files in your lesson project root

After fetching, your lesson project structure should look like:

```
your-lesson-project/
├── CLAUDE.md                     # Claude Code loads this automatically; it pulls in AGENTS.md and CONSTITUTION.md
├── AGENTS.md                     # Agent operating procedures
├── CONSTITUTION.md               # The governing document (the rules)
├── GUIDELINES.md                 # Practical how-to guide, with examples
├── TEST.md                       # Required format for test files (pytest .py)
├── lesson-<slug>.ipynb           # Code + the explanation after each block (or .py)
├── lesson-<slug>.md              # The full lesson: title, header line, 11 sections
└── lesson-<slug>-assignment.md   # Knowledge-check exercises with an answer key
```

### Step 3 — Invoke Claude Code

From your lesson project directory:

```bash
claude
```

Claude Code loads `CLAUDE.md` at startup. Through the instructions and imports
at the top of this file, it will:
- Follow the lesson workflows in `AGENTS.md`
- Respect every rule in `CONSTITUTION.md` (lesson structure, code quality, writing voice, diagrams, checklist)
- Use `GUIDELINES.md` for worked examples and `TEST.md` for the test-file format

## How Claude Code Uses These Files

### AGENTS.md

When you ask Claude Code to create, edit, or test a lesson, it follows the workflows defined in `AGENTS.md`:

- **Creating a lesson:** Confirms scope against the ~150-line raw-code reference, starts from the `.md` skeleton, keeps the lesson self-contained (UX-8), drafts Sections 1–6 before code and Sections 7 and 11 after, runs Gates 1–4, and hands Gate 5 to a human reviewer
- **Testing a lesson:** Runs Gate 1 (Fresh Environment) through Gate 4 (Assignment Test), prepares the Gate 5 hand-off, and reports pass/fail status
- **Editing a lesson:** Keeps the code file and Section 10 in sync, re-runs gates after changes, checks against peer lessons for consistency

You don't need to repeat instructions — just say "create a lesson on X" and Claude Code will automatically:
- Decide whether the scope fits the ~150-line raw-code reference (reference, not a hard cap)
- Build from the `.md` skeleton in Article I
- Run Gates 1–4 and report the lesson as ready for review (Gate 5 needs a person)

### CONSTITUTION.md

`CONSTITUTION.md` is the source of truth. It defines:

- **Article I:** Lesson files, what goes in the code file vs. the `.md`, the 11 sections, and the `.md` skeleton
- **Article II:** Code quality rules (CQ-1…CQ-11), including the ~150-line raw-code reference (CQ-1, for scoping, not a strict cap), and the shared Code Quality Standards
- **Article III:** UX requirements: writing voice, file naming, the assignment file, and self-contained lessons (UX-1…UX-8)
- **Article IV:** Performance requirements (runtime, compute, and cost disclosure)
- **Article V:** Mermaid diagrams
- **Article VI:** The pre-publish checklist

Claude Code treats every rule in `CONSTITUTION.md` as a hard constraint (only values marked as a *reference* or *suggestion* may be exceeded, and it flags when they are). If you ask it to bend a rule, it will name the Article and explain the tradeoff rather than complying silently.

### GUIDELINES.md

`GUIDELINES.md` is the practical how-to. Reference it when you want advice on:

- Writing voice, acronyms, and real-world examples
- Writing each of the 11 sections well
- Notebook layout (code, then its explanation)
- Creating Mermaid diagrams for pipelines and workflows
- The `!pip install` first cell and dependency management
- The assignment file skeleton and answer-key format
- Common pitfalls and anti-patterns

### TEST.md

`TEST.md` defines one rule: any test cases are written as standalone pytest `.py` files and run with `pytest` — never inside a notebook. Results are read from the pytest output; they are not saved to any other file or format.

## Common Workflows in Claude Code

### Workflow 1 — Create a new lesson

```
You:     "Create a lesson on Python list comprehensions."
Claude:  ✓ Confirms estimated raw-code line count
         ✓ Starts from the .md skeleton
         ✓ Drafts Sections 1–6 (and 8–9 if needed)
         ✓ Writes the code, each block followed by its explanation, and mirrors it in Section 10
         ✓ Writes Sections 7 and 11 once the code works
         ✓ Runs Gates 1–4 and the pre-publish checklist
         ✓ Reports results: "ready for review" with the Gate 5 hand-off
```

Claude Code handles Gates 1–4 automatically — no need to ask it to "run the tests" or "restart the kernel" separately. Gate 5 needs a person: share the hand-off with a reviewer, then pass their feedback back to Claude Code.

### Workflow 2 — Test an existing lesson

```
You:     "Test lesson-neural-nets.ipynb, Gates 1 through 4."
Claude:  ✓ Creates a fresh environment per Section 9 (or the first !pip install cell)
         ✓ Runs the notebook top-to-bottom (Gate 2)
         ✓ Verifies output matches Section 5 (Gate 3)
         ✓ Attempts the assignment and checks the answer key (Gate 4)
         ✓ Writes a test report and prepares the Gate 5 hand-off
```

### Workflow 3 — Edit and re-validate

```
You:     "Update lesson-nlp.ipynb: clarify the tokenization section."
Claude:  ✓ Makes the edit in the notebook and mirrors it in Section 10
         ✓ Checks the raw-code line count against CQ-1's reference
         ✓ Re-runs gates affected by the change
         ✓ Reports what needed re-testing and why
```

### Workflow 4 — Handle a conflict

```
You:     "Make this lesson 200 lines — it's important."
Claude:  ✓ Notes the reference line count (CQ-1, Article II)
         ✓ Offers options:
           - Split into a numbered series (Lesson 2a, 2b, ...) if content divides
           - Proceed with the longer lesson (line limits are a reference, not a hard cap — CQ-1) and flag the overage for review
           - Amend CONSTITUTION.md if the limit itself should change
         ✓ Lets you decide
```

## Tips for Using Claude Code with Comprehensive Principles

### Tip 1 — Keep CLAUDE.md and AGENTS.md at the project root

Claude Code loads `CLAUDE.md` from the directory you start it in, and `CLAUDE.md` pulls in `AGENTS.md` and `CONSTITUTION.md`. Keep all of them at the project root so every teammate's session behaves the same way.

### Tip 2 — Use the skeletons

When Claude Code creates a lesson, it starts from the `.md` skeleton in Article I and the assignment skeleton in `GUIDELINES.md`. Don't ask it to reorganize or renumber sections — they're in the order that works pedagogically.

### Tip 3 — Commit the governance files

Include `CLAUDE.md`, `CONSTITUTION.md`, `AGENTS.md`, `GUIDELINES.md`, and `TEST.md` in your repo's version control. This locks in the governance version for all future lessons and edits. Use the version from the footer of `CONSTITUTION.md` in the message:

```bash
git add CLAUDE.md CONSTITUTION.md AGENTS.md GUIDELINES.md TEST.md
git commit -m "Add Comprehensive Principles governance files (constitution vX.Y.Z)"
```

### Tip 4 — Reference Articles by number

When Claude Code explains a decision (e.g., "split into 2a/2b"), it cites the Article and rule (e.g., "per CQ-1 in Article II"). Use this same language in code reviews and pull requests — it keeps the constitution visible as the authority. These citations never appear inside a lesson (UX-8).

### Tip 5 — Check peer lessons for consistency

Before publishing, ask Claude Code to compare your lesson's explanation density, code complexity, and structure against another published lesson. This keeps the catalog cohesive.

## Governance Updates

If you want to amend `CONSTITUTION.md` (e.g., change the line reference, change the section structure), follow the process documented in `CONSTITUTION.md`'s Governance section:

1. Propose the change and its rationale
2. Review it yourself or name a reviewer
3. Add a compatibility note on existing lessons
4. Bump the version with semantic versioning: add a line to the version history at the top and update the version footer
5. Update the other governance files wherever they restate the changed rule

Claude Code only amends the constitution when you explicitly ask, and it surfaces the amendment process before making the change.

## Troubleshooting

### "Claude Code won't create a lesson that fits my scope"

Check CQ-1's line reference: about 150 lines of raw code (comments, docstrings, blank lines, and the `!pip` line don't count).

This is a reference for scoping, not a hard cap. If your concept is larger, ask Claude Code to split it into a numbered series (`Lesson 2a`, `Lesson 2b`, etc.) — or proceed with a longer lesson if the scope genuinely requires it.

### "A test gate is failing"

Check the gate-failure guidance in `AGENTS.md` under "Handling Gate Failures":
- **Gate 1 fails:** The setup (Section 9, or the notebook's first `!pip install` cell) is wrong — fix it and re-run
- **Gate 2 fails:** A cell errored — identify which one and fix it
- **Gate 3 fails:** Output doesn't match Section 5 — fix code or documentation
- **Gate 4 fails:** An assignment exercise can't be solved from the lesson or its answer key is wrong — fix the lesson or rewrite the exercise
- **Gate 5 fails:** A human reviewer found issues — pass their feedback to Claude Code to address

### "My lesson feels lighter or heavier than others"

Read one or two published peer lessons. If your explanation is notably lighter or heavier, bring it in line with the Code Quality Standards in `CONSTITUTION.md`, and make sure the header's time estimate and prerequisites match the actual content.

### "I need to bypass a rule"

Don't. Instead, ask Claude Code to explain the tradeoff and surface your options. You can:
- Amend the constitution (with rationale and review)
- Proceed and flag the lesson as non-compliant
- Split or redesign the concept to fit within the rules

Silently bypassing rules breaks the governance promise for other authors and lessons.

## Links to the Full Documents

- **`CONSTITUTION.md`** — The complete governing document (lesson structure and skeleton, code quality rules and standards, writing rules, diagrams, pre-publish checklist)
- **`AGENTS.md`** — Full agent operating procedures (create, test, review workflows; the five gates; test reporting)
- **`GUIDELINES.md`** — Practical how-to guide (writing voice, each section, notebook layout, Mermaid diagrams, common pitfalls)
- **`TEST.md`** — Required test-file format (pytest `.py` files, never notebooks)

## Need Help?

If Claude Code is unclear about a rule or gate:
- Cite the Article and rule (e.g., "per UX-2 in Article III")
- Ask Claude Code to explain the tradeoff
- Check the relevant Article in the full documents

For issues with Claude Code itself:
- Use `/help` in an interactive Claude Code session
- Report issues at https://github.com/anthropics/claude-code/issues
- Check the Claude Code documentation in the app or web interface
