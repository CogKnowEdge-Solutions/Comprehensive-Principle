# CLAUDE.md — Using Comprehensive Principles with Claude Code


## Quick Start: Fetch and Use in Claude Code

### Step 1 — Fetch the governance files

Choose one of these options:

**Option A — fetch all files in one command:**

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

**Option B — fetch individual files:**

```bash
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
├── AGENTS.md              # Claude Code reads this automatically
├── CONSTITUTION.md        # The governing document
├── GUIDELINES.md          # Practical how-to guide
├── TEST.md                # Testing framework
├── lesson-<slug>.ipynb       # Your lesson notebook or script
├── lesson-<slug>.md          # Lesson narrative (11 sections)
└── lesson-<slug>-assignment.md # Knowledge-check exercises
```

### Step 3 — Invoke Claude Code

From your lesson project directory:

```bash
claude
```

Claude Code will automatically:
- Read `AGENTS.md` and understand the required lesson-building workflow
- Respect the rules in `CONSTITUTION.md` (line references, section structure, difficulty levels)
- Apply the testing framework from `TEST.md`
- Reference `GUIDELINES.md` for style and best practices

## How Claude Code Uses These Files

### AGENTS.md

When you ask Claude Code to create, edit, or test a lesson, it follows the workflows defined in `AGENTS.md`:

- **Creating a lesson:** Confirms scope against line references, structures the 11-section format, keeps the content self-contained (no references to the governance files or other lessons; says "lesson", never "lab", in learner-facing text), applies the five validation gates
- **Testing a lesson:** Runs Gate 1 (Fresh Environment) through Gate 5 (Reviewer Walkthrough), captures test results to `.xlsx`, reports pass/fail status
- **Editing a lesson:** Re-runs gates after changes, checks against peer lessons for consistency, maintains difficulty-level alignment

You don't need to repeat instructions — just say "create a Beginner lesson on X" and Claude Code will automatically:
- Decide whether the scope fits the line reference (≤110 lines for Beginner; reference, not a hard cap)
- Build from the 11-section template
- Run all five gates before declaring it ready

### CONSTITUTION.md

`CONSTITUTION.md` is the source of truth. It defines:

- **Article I:** The 11-section lesson structure (required order and content)
- **Article II:** Line references by difficulty (Beginner ≤110, Intermediate ≤150, Advanced ≤180) — reference for scoping, not a strict cap (CQ-1)
- **Article III:** Code quality rules (CQ-1…CQ-11) and UX requirements
- **Article IV:** Difficulty levels and what they mean
- **Article V:** The assignment file format (knowledge-check exercises with answer keys)
- **Article VI:** The pre-publish checklist

Claude Code treats every MUST/MUST NOT in `CONSTITUTION.md` as a hard constraint. If you ask it to bend a rule, it will name the Article and explain the tradeoff rather than complying silently.

### GUIDELINES.md

`GUIDELINES.md` is the practical how-to. Reference it when you want advice on:

- Writing each of the 11 sections well
- Difficulty signaling and consistency across lessons
- Creating Mermaid diagrams for pipelines and workflows
- The `!pip install` first cell and dependency management
- Assignment file structure and answer-key format
- Common pitfalls and anti-patterns

### TEST.md

`TEST.md` covers the testing framework:

- General test-case design and the RED → GREEN → REFACTOR TDD cycle
- How to write tests as standalone pytest `.py` files (never inside `.ipynb`)
- Running tests and parsing output
- Troubleshooting failed tests
- Saving test results to `.xlsx` for reviewability

The five gates in `AGENTS.md` use `TEST.md`'s framework. If a lesson has testable logic, `TEST.md` shows how to write those tests.

## Common Workflows in Claude Code

### Workflow 1 — Create a new lesson

```
You:     "Create a Beginner lesson on Python list comprehensions."
Claude:  ✓ Confirms difficulty and estimated line count
         ✓ Starts from the 11-section template
         ✓ Drafts sections 1-9 (explanation of topic through environment setup)
         ✓ Writes code with appropriate explanation density
         ✓ Runs all five gates
         ✓ Reports gate results and publishes-readiness
```

Claude Code handles the entire workflow automatically — no need to ask it to "run the tests" or "restart the kernel" separately.

### Workflow 2 — Test an existing lesson

```
You:     "Test lesson-neural-nets.ipynb. Gate 1 through 5."
Claude:  ✓ Creates a fresh environment per Section 9
         ✓ Runs the notebook top-to-bottom (Gate 2)
         ✓ Verifies output matches Section 5 (Gate 3)
         ✓ Attempts the assignment and checks the answer key (Gate 4)
         ✓ Generates a test report and .xlsx results file
```

### Workflow 3 — Edit and re-validate

```
You:     "Update lesson-nlp.ipynb: clarify the tokenization section."
Claude:  ✓ Makes the edit
         ✓ Checks line count against Article II's reference
         ✓ Re-runs gates affected by the change
         ✓ Reports what needed re-testing and why
```

### Workflow 4 — Handle a conflict

```
You:     "Make this lesson 200 lines — it's important."
Claude:  ✓ Notes the reference line count (Article II: line reference)
         ✓ Offers options:
           - Split into a numbered series (Lesson 2a, 2b, ...) if content divides
           - Proceed with the longer lesson (line limits are a reference, not a hard cap — CQ-1) and flag the overage for review
           - Amend CONSTITUTION.md if the limit itself should change
         ✓ Lets you decide
```

## Tips for Using Claude Code with Comprehensive Principles

### Tip 1 — Always place AGENTS.md at the project root

Claude Code will find and read `AGENTS.md` automatically if it's in the directory you invoke it from. This ensures consistent behavior across your team.

### Tip 2 — Use the pre-defined templates

When Claude Code creates a lesson, it starts with the 11-section template from Article I. Don't ask it to reorganize sections — they're in the order that works pedagogically.

### Tip 3 — Commit the governance files

Include `CONSTITUTION.md`, `AGENTS.md`, `GUIDELINES.md`, and `TEST.md` in your repo's version control. This locks in the governance version for all future lessons and edits.

```bash
git add CONSTITUTION.md AGENTS.md GUIDELINES.md TEST.md
git commit -m "Add Comprehensive Principles governance files v0.1.0"
```

### Tip 4 — Reference Articles by number

When Claude Code explains a decision (e.g., "split into 2a/2b"), it cites the Article (e.g., "per Article II's line reference"). Use this same language in code reviews and pull requests — it keeps the constitution visible as the authority.

### Tip 5 — Save test results to `.xlsx`

Every time Claude Code runs the gates, it saves results to an `.xlsx` file (e.g., `test_nlp_2026-08-17.xlsx`). This makes test outcomes reviewable without re-running pytest and auditable over time.

### Tip 6 — Check peer lessons for consistency

Before publishing, ask Claude Code to compare your lesson's explanation density, code complexity, and structure against another lesson at the same difficulty level. This keeps the catalog cohesive.

## Governance Updates

If you want to amend `CONSTITUTION.md` (e.g., change a line reference, change the section structure), follow the process documented in `CONSTITUTION.md`'s Governance section:

1. Propose the change and rationale
2. Review against existing lessons for impact
3. Add a compatibility note documenting existing lessons
4. Bump the version with semantic versioning
5. Update the Sync Impact Report comment

Claude Code will refuse a governance change that isn't properly proposed and will surface the amendment process explicitly.

## Troubleshooting

### "Claude Code won't create a lesson that fits my scope"

Check Article II's line reference for your difficulty level:
- Beginner: ≤110 lines
- Intermediate: ≤150 lines
- Advanced: ≤180 lines

These are references for scoping, not hard caps (CQ-1). If your concept is larger, ask Claude Code to split it into a numbered series (`Lesson 2a`, `Lesson 2b`, etc.) — or proceed with a longer lesson if the scope genuinely requires it.

### "A test gate is failing"

Check the gate-failure guidance in `AGENTS.md` under "Handling Gate Failures":
- **Gate 1 fails:** Section 9's environment setup is wrong — fix it and re-run
- **Gate 2 fails:** A cell errored — identify which one and fix it
- **Gate 3 fails:** Output doesn't match Section 5 — fix code or documentation
- **Gate 4 fails:** An assignment exercise can't be solved from the lesson or its answer key is wrong — fix the lesson or rewrite the exercise
- **Gate 5 fails:** A human reviewer found issues — address their feedback

### "My lesson doesn't match the difficulty level"

Read the peer lessons at your intended difficulty level. If your explanation is notably lighter or heavier, your difficulty header may be wrong — update it to match the actual content, not the intended level.

### "I need to bypass a rule"

Don't. Instead, ask Claude Code to explain the tradeoff and surface your options. You can:
- Amend the constitution (with rationale and review)
- Proceed and flag the lesson as non-compliant
- Split or redesign the concept to fit within the rules

Silently bypassing rules breaks the governance promise for other authors and lessons.

## Links to the Full Documents

- **`CONSTITUTION.md`** — The complete governing document (11 sections, code quality rules, difficulty standards, pre-publish checklist)
- **`AGENTS.md`** — Full agent operating procedures (create, test, review workflows; all five gates; test reporting)
- **`GUIDELINES.md`** — Practical how-to guide (writing each section, Mermaid diagrams, common pitfalls)
- **`TEST.md`** — Comprehensive testing guide (test-case design, TDD cycle, pytest commands)

## Need Help?

If Claude Code is unclear about a rule or gate:
- Cite the Article (e.g., "per Article III's UX requirements")
- Ask Claude Code to explain the tradeoff
- Check the relevant section in the full documents

For issues with Claude Code itself:
- Use `/help` in an interactive Claude Code session
- Report issues at https://github.com/anthropics/claude-code/issues
- Check the Claude Code documentation in the app or web interface

---

**Version 1.0 — August 2026**
