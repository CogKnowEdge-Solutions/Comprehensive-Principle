# Comprehensive Principle — Lessons Project Governance

A set of five governance files that define how agents and authors build, test, and publish lessons in this catalog. Drop them into any lesson project to enforce consistent structure, difficulty labeling, code quality, and testing across every lesson.

## Files

| File | Purpose |
|------|---------|
| `files/CONSTITUTION.md` | The source of truth: 11-section lesson structure, code quality rules (CQ-1…CQ-11), difficulty standards, and the pre-publish checklist. Versioned with semantic versioning. |
| `files/AGENTS.md` | Operating procedures for an AI agent: how to create, test, review, and edit lessons while staying compliant with the constitution. Defines the five-gate lesson validation workflow. |
| `files/CLAUDE.md` | Quick start guide for Claude Code users: how to fetch these files, use them in Claude Code workflows, common workflows, and troubleshooting. |
| `files/GUIDELINES.md` | Practical how-to: writing each section well, Mermaid diagrams, the `!pip install` first cell, the assignment file, common pitfalls, and quick-reference checklists. |
| `files/TEST.md` | Comprehensive testing guide: general test-case design, the TDD cycle (RED → GREEN → REFACTOR), running tests, and troubleshooting. |

## How to Use in a Lesson Project

1. **Fetch the files** into your lesson repo (see fetch commands below).
2. `CONSTITUTION.md` — treat as the governing document. Every lesson must follow the 11-section structure and clear the pre-publish checklist.
3. `AGENTS.md` — place at the repo root (or where your agent looks) so AI agents follow the create/test/review workflow automatically.
4. `GUIDELINES.md` — reference while building: section writing, difficulty signaling, Mermaid diagrams, dependency install cells, and assignment files.
5. `TEST.md` — consult while testing: test-case design, the TDD cycle, and pytest commands.

## Fetch Commands

### Option A — clone the whole repo

```bash
git clone https://github.com/CogKnowEdge-Solutions/Comprehensive-Principle.git
```

### Option B — core files only (manual authorship, no automation)

Use this if you're not using an AI agent and want just the governance framework.

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

### Option C — all files for Claude Code users

Use this if you're building lessons with Claude Code. Includes `CLAUDE.md` (quick-start guide) and `AGENTS.md` (automation workflows).

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md -o CLAUDE.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

### Option D — all files for other AI agents (no Claude Code)

Use this if you're using an AI agent other than Claude Code. Includes `AGENTS.md` (automation workflows) but excludes `CLAUDE.md`.

```bash
curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md -o AGENTS.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md -o CONSTITUTION.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md -o GUIDELINES.md \
  && curl -s https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md -o TEST.md
```

### Option E — pull individual files straight into a lesson

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/AGENTS.md
curl -o CLAUDE.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CLAUDE.md
curl -o CONSTITUTION.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/CONSTITUTION.md
curl -o GUIDELINES.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/GUIDELINES.md
curl -o TEST.md https://raw.githubusercontent.com/CogKnowEdge-Solutions/Comprehensive-Principle/main/files/TEST.md
```

## For Claude Code Users

If you're using Claude Code to build or maintain lessons with this governance:

1. **Fetch `CLAUDE.md`** — it's a quick-start guide tailored for Claude Code workflows, showing how to invoke Claude Code, common workflows (create, test, edit), tips for consistency, and troubleshooting.
2. **Place all five files at your lesson project root** — Claude Code will automatically read `AGENTS.md` and follow the workflows defined there.
3. **Invoke Claude Code from your lesson directory:**
   ```bash
   claude
   ```
   Claude Code will then follow the five-gate testing workflow, reference line ceilings during scoping, enforce section structure, and manage the full lesson lifecycle.

See `files/CLAUDE.md` for detailed workflows, tips, and guidance specific to Claude Code.

## What a Compliant Lesson Looks Like

A lesson ships as three files with the same slug in one folder:

- `lesson-<topic-slug>.ipynb` (or `.py`) — the runnable code
- `lesson-<topic-slug>.md` — the 11-section narrative
- `lesson-<topic-slug>-assignment.md` — knowledge-check exercises with an answer key

The notebook starts with a single `!pip install` cell, includes at least one Mermaid diagram for any pipeline/flow concept, and passes all five gates (fresh environment, run-all, output verification, assignment test, reviewer walkthrough) defined in AGENTS.md before publishing.
