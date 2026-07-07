# Sacred Invariants

**A Codex skill for studying an unfamiliar codebase before changing it.**

This skill helps an AI agent slow down and first understand what a project is trying to protect. It is useful when the codebase is old, large, unclear, or new to the agent.

The goal is simple: read the code first, find the important rules, and write a clear handoff for the next agent.

## What It Helps With

- Understand what the project is for.
- Find the rules the code must not break.
- Separate real source data from cached or derived data.
- Notice when docs, tests, code, or human expectations disagree.
- Prepare a short handoff for agents that will later write code or tests.

## What It Does Not Do

- It does not write code.
- It does not write tests.
- It does not list every file in the project.
- It does not replace a normal bug fix or code review.
- It does not guess product intent before reading the code.

## When To Use It

Use this skill when an agent is about to work in a codebase it does not understand yet.

Good examples:

- starting work in an old project;
- reviewing a risky feature area;
- checking why a system has confusing rules;
- preparing a handoff for another agent;
- finding product rules before making changes.

Do not use it for small edits, simple refactors, or normal test fixes.

## What It Produces

The skill can produce artifacts like:

- `DISCOVERY.md` - what the agent found from the code;
- `LEAD_ARCHITECT_SYNTHESIS.md` - the main judgment about what matters;
- `QUESTIONS.md` - questions for the owner, asked after reading the code;
- `DRIFT.md` - differences between the code and what people think the code does;
- `INVARIANTS.md` - important rules the system should protect;
- `AGENT_HANDOFF.md` - a focused handoff for the next agent.

## Repository Contents

- `SKILL.md` - the skill instructions.
- `assets/` - templates for the synthesis, consolidated report, and downstream handoff.
- `agents/openai.yaml` - metadata for agent UIs.
- `LICENSE.md` - noncommercial license terms.
- `COMMERCIAL.md` - commercial licensing terms.
- `README.md` - this file.

This repository contains the published skill surface only.

## Using The Skill

Point Codex at this folder as a skill, then ask it to use `sacred-invariants` on a target codebase.
Some UIs may expose the same skill as `$sacred-invariants`.

Example:

```text
Use sacred-invariants on this repository before planning changes.
Focus on the data ownership and failure rules around the import flow.
```

The skill is meant for deliberate use. It should not run automatically just because a task mentions architecture or invariants.

## License

Copyright (c) 2026 Maksim Brashchenko.

This project is available for noncommercial use under the [PolyForm Noncommercial License 1.0.0](LICENSE.md). Commercial use requires a separate written commercial license; see [COMMERCIAL.md](COMMERCIAL.md).
