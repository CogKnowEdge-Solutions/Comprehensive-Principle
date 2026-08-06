# Comprehensive Principle — Labs Project Governance

A set of three governance files that define how agents and authors build, test, and publish labs in this catalog. Drop them into any lab project to enforce consistent structure, difficulty labeling, code quality, and testing across every lab.

## Files

| File | Purpose |
|------|---------|
| `files/CONSTITUTION.md` | The source of truth: 12-section lab structure, code quality rules (CQ-1…CQ-10), the five testing gates, difficulty standards, and the pre-publish checklist. Versioned with semantic versioning. |
| `files/AGENTS.md` | Operating procedures for an AI agent: how to create, test, review, and edit labs while staying compliant with the constitution. |
| `files/GUIDELINES.md` | Practical how-to: writing each section well, Mermaid diagrams, the `!pip install` first cell, the assignment file, common pitfalls, and quick-reference checklists. |

## How to Use in a Lab Project

1. **Fetch the files** into your lab repo (see fetch commands below).
2. `CONSTITUTION.md` — treat as the governing document. Every lab must follow the 12-section structure and clear the pre-publish checklist.
3. `AGENTS.md` — place at the repo root (or where your agent looks) so AI agents follow the create/test/review workflow automatically.
4. `GUIDELINES.md` — reference while building: section writing, difficulty signaling, Mermaid diagrams, dependency install cells, and assignment files.

## Fetch Commands

### Option A — clone the whole repo

```bash
git clone https://github.com/CogKnowEdge-Solutions/Comprehensive-Principle.git
```

### Option B — pull each file straight into a lab

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md
curl -o CONSTITUTION.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md
curl -o GUIDELINES.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md
```

### Option C — all three files in one command

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md
```

## What a Compliant Lab Looks Like

A lab ships as three files with the same slug in one folder:

- `lab-<topic-slug>.ipynb` (or `.py`) — the runnable code
- `lab-<topic-slug>.md` — the 12-section narrative
- `lab-<topic-slug>-assignment.md` — knowledge-check exercises with an answer key

The notebook starts with a single `!pip install` cell, includes at least one Mermaid diagram for any pipeline/flow concept, and passes all five gates (fresh environment, run-all, output verification, optional exercise, reviewer walkthrough) before publishing.
