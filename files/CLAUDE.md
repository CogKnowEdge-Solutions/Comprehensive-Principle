# CLAUDE.md — Using Comprehensive Principles with Claude Code

This guide helps Claude Code users and AI agents set up and use the Comprehensive Principles governance system in their lab projects.

## What is Claude Code?

Claude Code is Anthropic's CLI for Claude, available as:
- A terminal CLI (`claude`)
- A desktop app (Mac/Windows)
- A web app (claude.ai/code)
- IDE extensions (VS Code, JetBrains)

When you invoke Claude Code in your lab project directory, it automatically reads governance files — including `CONSTITUTION.md`, `AGENTS.md`, `GUIDELINES.md`, and `TEST.md` — and follows their rules during your entire session.

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
cp Comprehensive-Principle/files/*.md your-lab-project/
```

### Step 2 — Place files in your lab project root

After fetching, your lab project structure should look like:

```
your-lab-project/
├── AGENTS.md              # Claude Code reads this automatically
├── CONSTITUTION.md        # The governing document
├── GUIDELINES.md          # Practical how-to guide
├── TEST.md                # Testing framework
├── lab-<slug>.ipynb       # Your lab notebook or script
├── lab-<slug>.md          # Lab narrative (12 sections)
└── lab-<slug>-assignment.md # Knowledge-check exercises
```

### Step 3 — Invoke Claude Code

From your lab project directory:

```bash
claude
```

Claude Code will automatically:
- Read `AGENTS.md` and understand the required lab-building workflow
- Respect the rules in `CONSTITUTION.md` (line limits, section structure, difficulty levels)
- Apply the testing framework from `TEST.md`
- Reference `GUIDELINES.md` for style and best practices

## How Claude Code Uses These Files

### AGENTS.md

When you ask Claude Code to create, edit, or test a lab, it follows the workflows defined in `AGENTS.md`:

- **Creating a lab:** Validates difficulty level, confirms scope against line ceilings, structures the 12-section format, applies the five validation gates
- **Testing a lab:** Runs Gate 1 (Fresh Environment) through Gate 5 (Reviewer Walkthrough), captures test results to `.xlsx`, reports pass/fail status
- **Editing a lab:** Re-runs gates after changes, checks against peer labs for consistency, maintains difficulty-level alignment

You don't need to repeat instructions — just say "create a Beginner lab on X" and Claude Code will automatically:
- Decide whether the scope fits the line ceiling (≤110 lines for Beginner)
- Build from the 12-section template
- Run all five gates before declaring it ready

### CONSTITUTION.md

`CONSTITUTION.md` is the source of truth. It defines:

- **Article I:** The 12-section lab structure (required order and content)
- **Article II:** Line ceilings by difficulty (Beginner ≤110, Intermediate ≤150, Advanced ≤180)
- **Article III:** Code quality rules (CQ-1…CQ-10) and UX requirements
- **Article IV:** Difficulty levels and what they mean
- **Article V:** The assignment file format (knowledge-check exercises with answer keys)
- **Article VI:** The pre-publish checklist

Claude Code treats every MUST/MUST NOT in `CONSTITUTION.md` as a hard constraint. If you ask it to bend a rule, it will name the Article and explain the tradeoff rather than complying silently.

### GUIDELINES.md

`GUIDELINES.md` is the practical how-to. Reference it when you want advice on:

- Writing each of the 12 sections well
- Difficulty signaling and consistency across labs
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

The five gates in `AGENTS.md` use `TEST.md`'s framework. If a lab has testable logic, `TEST.md` shows how to write those tests.

## Common Workflows in Claude Code

### Workflow 1 — Create a new lab

```
You:     "Create a Beginner lab on Python list comprehensions."
Claude:  ✓ Confirms difficulty and estimated line count
         ✓ Starts from the 12-section template
         ✓ Drafts sections 1-9 (problem through environment setup)
         ✓ Writes code with appropriate explanation density
         ✓ Runs all five gates
         ✓ Reports gate results and publishes-readiness
```

Claude Code handles the entire workflow automatically — no need to ask it to "run the tests" or "restart the kernel" separately.

### Workflow 2 — Test an existing lab

```
You:     "Test lab-neural-nets.ipynb. Gate 1 through 5."
Claude:  ✓ Creates a fresh environment per Section 9
         ✓ Runs the notebook top-to-bottom (Gate 2)
         ✓ Verifies output matches Section 5 (Gate 3)
         ✓ Performs the Optional Exercise (Gate 4)
         ✓ Generates a test report and .xlsx results file
```

### Workflow 3 — Edit and re-validate

```
You:     "Update lab-nlp.ipynb: clarify the tokenization section."
Claude:  ✓ Makes the edit
         ✓ Checks line count against Article II
         ✓ Re-runs gates affected by the change
         ✓ Reports what needed re-testing and why
```

### Workflow 4 — Handle a conflict

```
You:     "Make this lab 200 lines — it's important."
Claude:  ✓ Names the Article (Article II: line ceiling)
         ✓ Offers real options:
           - Split into a numbered series (Lab 2a, 2b, ...)
           - Amend CONSTITUTION.md (with rationale and review process)
           - Flag it as non-compliant until amended
         ✓ Lets you decide
```

## Tips for Using Claude Code with Comprehensive Principles

### Tip 1 — Always place AGENTS.md at the project root

Claude Code will find and read `AGENTS.md` automatically if it's in the directory you invoke it from. This ensures consistent behavior across your team.

### Tip 2 — Use the pre-defined templates

When Claude Code creates a lab, it starts with the 12-section template from Article I. Don't ask it to reorganize sections — they're in the order that works pedagogically.

### Tip 3 — Commit the governance files

Include `CONSTITUTION.md`, `AGENTS.md`, `GUIDELINES.md`, and `TEST.md` in your repo's version control. This locks in the governance version for all future labs and edits.

```bash
git add CONSTITUTION.md AGENTS.md GUIDELINES.md TEST.md
git commit -m "Add Comprehensive Principles governance files v0.1.0"
```

### Tip 4 — Reference Articles by number

When Claude Code explains a decision (e.g., "split into 2a/2b"), it cites the Article (e.g., "per Article II's line ceiling"). Use this same language in code reviews and pull requests — it keeps the constitution visible as the authority.

### Tip 5 — Save test results to `.xlsx`

Every time Claude Code runs the gates, it saves results to an `.xlsx` file (e.g., `test_nlp_2026-08-17.xlsx`). This makes test outcomes reviewable without re-running pytest and auditable over time.

### Tip 6 — Check peer labs for consistency

Before publishing, ask Claude Code to compare your lab's explanation density, code complexity, and structure against another lab at the same difficulty level. This keeps the catalog cohesive.

## Governance Updates

If you want to amend `CONSTITUTION.md` (e.g., increase a line ceiling, change the section structure), follow the process documented in `CONSTITUTION.md`'s Governance section:

1. Propose the change and rationale
2. Review against existing labs for impact
3. Add a compatibility note documenting existing labs
4. Bump the version with semantic versioning
5. Update the Sync Impact Report comment

Claude Code will refuse a governance change that isn't properly proposed and will surface the amendment process explicitly.

## Troubleshooting

### "Claude Code won't create a lab that fits my scope"

Check Article II's line ceiling for your difficulty level:
- Beginner: ≤110 lines
- Intermediate: ≤150 lines
- Advanced: ≤180 lines

If your concept is too large, ask Claude Code to split it into a numbered series (`Lab 2a`, `Lab 2b`, etc.) before writing code.

### "A test gate is failing"

Check the gate-failure guidance in `AGENTS.md` under "Handling Gate Failures":
- **Gate 1 fails:** Section 9's environment setup is wrong — fix it and re-run
- **Gate 2 fails:** A cell errored — identify which one and fix it
- **Gate 3 fails:** Output doesn't match Section 5 — fix code or documentation
- **Gate 4 fails:** The Optional Exercise doesn't work — fix the lab or rewrite the exercise
- **Gate 5 fails:** A human reviewer found issues — address their feedback

### "My lab doesn't match the difficulty level"

Read the peer labs at your intended difficulty level. If your explanation is notably lighter or heavier, your difficulty header may be wrong — update it to match the actual content, not the intended level.

### "I need to bypass a rule"

Don't. Instead, ask Claude Code to explain the tradeoff and surface your options. You can:
- Amend the constitution (with rationale and review)
- Proceed and flag the lab as non-compliant
- Split or redesign the concept to fit within the rules

Silently bypassing rules breaks the governance promise for other authors and labs.

## Links to the Full Documents

- **`CONSTITUTION.md`** — The complete governing document (12 sections, code quality rules, difficulty standards, pre-publish checklist)
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
