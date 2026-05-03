# Document Governance Skill

[English](README.md) | [简体中文](README.zh-CN.md)

A portable AI-agent skill for organizing project documentation into a clear, low-context, maintainable structure.

Imagine you open an existing project and the first thing the agent sees is a wall of Markdown: old sprint plans, outdated decisions, API notes, deployment commands, long research notes, and TODOs all mixed together. The agent does not know what is current, what is historical, or what should be ignored.

This skill gives an agent a simple playbook for cleaning that up.

## Problems It Solves

Long-running projects often accumulate documentation that becomes hard for humans and AI agents to use:

- one file mixes current state, old plans, historical decisions, deployment notes, and raw research
- agents waste context reading obsolete sprint logs or superseded decisions
- specs, ADRs, runbooks, and research notes drift out of sync
- nobody knows whether a change should update a spec, an ADR, a progress file, or a runbook
- large research notes or exported files pollute everyday development context
- important guardrails exist, but are either over-read on every task or missed when they matter

This skill helps split docs by responsibility, define a reading order, and keep high-value context discoverable without making every session carry the whole history.

## When To Use It

Use this skill when:

- a repository's `docs/` folder has grown large, stale, or hard to navigate
- you want a low-token reading path for future AI coding sessions
- you need to separate current status, specs, ADRs, runbooks, research, and archive material
- historical plans or old decisions are confusing new contributors or agents
- you want clear rules for when specs, ADRs, or progress files should be updated
- you are moving documentation files and need to preserve references
- you need a guardrail/locked-file documentation policy for high-risk code paths
- you are preparing a project for handoff to another engineer or AI agent

It is especially useful for AI-assisted projects where context budget matters and stale docs can steer agents into obsolete work.

## When Not To Use It

You probably do not need this skill for:

- a tiny project with one or two short docs
- one-off README copy editing
- generating product documentation for end users
- replacing real technical specs or ADR content with generic templates

## Capabilities

Use it when you want an AI agent to:

- clean up a messy `docs/` folder
- define what each document should be responsible for
- split current state, specs, ADRs, runbooks, research, and archives
- reduce context bloat from large historical documents
- update documentation references after moving files
- decide when specs, ADRs, or progress files should be updated

## What It Teaches an Agent

The skill establishes a generic documentation architecture:

```text
docs/
  README.md                 # documentation map and reading order
  progress.md               # current facts, active work, guardrails
  decisions_index.md        # ADR status index
  decisions.md              # full ADR history
  specs/                    # current module contracts
  runbooks/                 # operations and verification procedures
  research/                 # research / reference entrypoints
  archive/                  # superseded plans and old logs
```

It also defines rules for:

- when to update specs
- when to write or update ADRs
- how to keep progress documents short
- how to handle guarded or high-risk files
- how to migrate documents safely without breaking links

## Installation

This repository is tool-neutral. Use it in any agent system that supports reusable instructions, skills, rules, memories, or knowledge files.

### Option A: Use as a skill folder

Copy the skill folder into your agent's skills/instructions directory:

```bash
cp -R document-governance /path/to/your/agent/skills/
```

Then start a new agent session and ask it to use `document-governance`.

### Option B: Paste into custom instructions

Open `document-governance/SKILL.md` and paste its contents into your agent's reusable instruction area.

### Option C: Reference it directly

Point your agent at `document-governance/SKILL.md` and ask it to follow the workflow.

## Example Prompts

```text
Use document-governance to reorganize this repo's docs folder.
```

```text
Audit our docs and propose a low-token reading order for future agents.
```

```text
Create a docs README, ADR index, specs README, runbooks README, and archive README.
```

```text
Decide which docs should be current-state docs, specs, ADRs, runbooks, research, or archive.
```

## Repository Layout

```text
document-governance/
  SKILL.md
```

## Notes

This skill is project-neutral. It does not assume a specific tech stack, company, product, or repository layout beyond conventional project documentation patterns.

## License

MIT
