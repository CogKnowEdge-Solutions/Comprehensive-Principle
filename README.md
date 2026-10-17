# Comprehensive Principle — Lessons Project Governance

A set of five governance files that define how agents and authors build, test, and publish lessons in this catalog. Drop them into any lesson project to enforce consistent structure, code quality, and testing across every lesson.

## Files

| File | Purpose |
|------|---------|
| `files/CONSTITUTION.md` | The source of truth (Articles I–VI): lesson files and the `.md` skeleton, the 11-section structure, code quality rules (CQ-1…CQ-11) and standards, writing and UX rules (UX-1…UX-8), diagrams, and the pre-publish checklist. Versioned with semantic versioning. |
| `files/AGENTS.md` | Operating procedures for an AI agent: how to create, test, review, and edit lessons while staying compliant with the constitution. Defines the five-gate lesson validation workflow. |
| `files/CLAUDE.md` | Loaded automatically by Claude Code: points it at `AGENTS.md` and `CONSTITUTION.md`, plus a quick-start guide for people using Claude Code. |
| `files/GUIDELINES.md` | Practical how-to: writing voice, each section, notebook layout, Mermaid diagrams, the `!pip install` first cell, the assignment skeleton, common pitfalls, and quick-reference checklists. |
| `files/TEST.md` | The required test-file format: test cases are standalone pytest `.py` files, never notebook cells. |

## How to Use in a Lesson Project

1. **Fetch the files** into your lesson repo (see fetch commands below).
2. `CONSTITUTION.md` — treat as the governing document. Every lesson must follow the Article I structure and clear the Article VI pre-publish checklist.
3. `AGENTS.md` — place at the repo root (or where your agent looks) so AI agents follow the create/test/review workflow. It also defines the five validation gates, which the checklist depends on.
4. `GUIDELINES.md` — reference while building: writing voice, section writing, notebook layout, Mermaid diagrams, dependency install cells, and assignment files.
5. `TEST.md` — follow if the lesson ships test cases: pytest `.py` files only.

## Fetch Commands

### Option A — clone the whole repo

```bash
git clone https://github.com/CogKnowEdge-Solutions/Comprehensive-Principle.git
```

### Option B — all files except `CLAUDE.md` (manual authors, or AI agents other than Claude Code)

`AGENTS.md` is included because it defines the five validation gates that the constitution's checklist requires.

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

### Option C — all files for Claude Code users

Use this if you're building lessons with Claude Code. Includes `CLAUDE.md`, which Claude Code loads automatically and which pulls in `AGENTS.md` and `CONSTITUTION.md`.

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md -o CLAUDE.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

### Option D — pull individual files straight into a lesson

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md
curl -o CLAUDE.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md
curl -o CONSTITUTION.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md
curl -o GUIDELINES.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md
curl -o TEST.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md
```

## For Claude Code Users

If you're using Claude Code to build or maintain lessons with this governance:

1. **Fetch all five files (Option C)** — `CLAUDE.md` is what Claude Code loads automatically; it tells Claude Code to follow `AGENTS.md` and `CONSTITUTION.md` and imports both.
2. **Place all five files at your lesson project root.**
3. **Invoke Claude Code from your lesson directory:**
   ```bash
   claude
   ```
   Claude Code will then scope against the line reference, enforce the lesson structure, run Gates 1–4, hand Gate 5 to a human reviewer, and manage the full lesson lifecycle.

See `files/CLAUDE.md` for detailed workflows, tips, and guidance specific to Claude Code.

## What a Compliant Lesson Looks Like

A lesson ships as three files with the same slug in one folder:

- `lesson-<topic-slug>.ipynb` (or `.py`) — the runnable code, each block followed by its explanation, and nothing else
- `lesson-<topic-slug>.md` — the full lesson: the title, then the 11 sections (Section 10 mirrors the code file)
- `lesson-<topic-slug>-assignment.md` — knowledge-check exercises with an answer key

The notebook starts with a single `!pip install` cell. The `.md` has a Mermaid diagram in Section 1 and in Section 7 for any pipeline/flow concept. Before publishing, the lesson passes all five gates defined in `AGENTS.md`: fresh environment, run-all, output verification, assignment test, and a human reviewer walkthrough.
