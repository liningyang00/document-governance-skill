---
name: document-governance
description: Organize and maintain project documentation architecture. Use when an AI agent is asked to clean up docs, define document structure, reduce context/token bloat, create README/ADR/spec/runbook/research/archive conventions, migrate or classify project docs, fix documentation references, or decide when specs/ADRs/progress files should be updated.
---

# Document Governance

Use this skill to turn a messy documentation folder into a low-token, maintainable system with clear entrypoints, ownership, and update rules.

## Core Principle

Separate documents by job:

- **Current state** answers: "What is true now, and what is next?"
- **Specs** answer: "How does this module work today?"
- **ADRs** answer: "Why did we choose this?"
- **Runbooks** answer: "How do I operate or verify this?"
- **Research/reference** answers: "What did we learn, and how strong is the proof?"
- **Archive** answers: "What happened historically, but should not guide daily work?"

Do not let one document become all of these.

## Default Structure

Recommend or create this generic structure unless the repository already has a better convention:

```text
docs/
  README.md                 # documentation map and reading order
  progress.md               # current facts, active work, guardrails
  decisions_index.md        # ADR status index
  decisions.md              # full ADR history
  specs/                    # current module contracts
    README.md
  runbooks/                 # operations and verification procedures
    README.md
  research/                 # research and reference entrypoints
    README.md
    reference_index.md
  archive/                  # superseded plans, old logs, historical notes
    README.md
```

## Reading Policy

Define an explicit reading order:

1. Read `docs/progress.md` first.
2. Read only the relevant `docs/specs/*.md` for the module being changed.
3. Read `docs/decisions_index.md` before consulting full ADRs.
4. Read runbooks only for deployment, testing, maintenance, or incident tasks.
5. Read research/reference material only when the task needs benchmarking, background research, or historical investigation.
6. Treat large exports, screenshots, raw notes, and old plans as opt-in context.

## Document Roles

### `docs/README.md`

Make this the top-level map. Include:

- Daily reading order.
- Directory purposes.
- Spec update rules.
- Where to put new docs.
- Links to runbooks, ADR index, research index, and archive.

Do not turn it into a project history.

### `docs/progress.md`

Keep this short. Include:

- Current architecture facts.
- Active work and open backlog.
- Current guardrails or "read before touching" triggers.
- Recent deployment or verification summary only if useful.

Exclude:

- Long sprint logs.
- Old implementation plans.
- Detailed historical reasoning.
- Raw research notes.

### `docs/specs/*.md`

Specs are current contracts. Update specs when a change affects:

- API parameters, responses, errors.
- Database fields, JSON shapes, data semantics.
- State machines, lifecycle states, events, queues.
- Cross-layer data contracts.
- Permissions, quotas, billing, limits.

Do not update specs for:

- Pure styling or copy changes.
- Internal refactors with unchanged inputs/outputs.
- Logs, tests, or deployment scripts.
- Bug fixes that do not change the contract.

### ADRs

Keep full ADR history in one or more ADR files, but create `decisions_index.md` with:

- ADR id.
- Status: `Active`, `Locked`, `Superseded`, or `Archive`.
- Topic.
- When to read.
- Replacement ADR for superseded decisions.

Use ADRs for "why"; use specs for "what is true now."

### Runbooks

Move operational procedures to `docs/runbooks/`:

- Deployment.
- Testing and QA.
- Environment variables and secrets.
- Incident response.
- Maintenance audits.

Runbooks should be procedural, not philosophical.

### Research / Reference

Move research-heavy material to `docs/research/`.

Add an index that says:

- What each file contains.
- Whether it is active reference material, background research, or archive-ish.
- Which conclusions have become ADR/spec constraints.
- Where raw notes or exported reference files live.

Research conclusions that affect implementation must be promoted into an ADR or spec. Do not make ordinary development depend on reading raw notes.

### Archive

Move obsolete plans, sprint logs, old parameter tables, and superseded conclusions to `docs/archive/`.

Archive files should be discoverable, but never part of the default reading path.

## Guardrail Documents

For projects with high-risk code paths, define a guardrail or locked-files document.

Rules:

- Do not read the full guardrail document on every task.
- Always read it before touching listed files or protected capabilities.
- If editing the guardrail document itself, treat that as a guarded change.
- Explain only the core capabilities actually relevant to the proposed change.
- Do not mechanically report every possible protected capability.

Example language:

```text
This touches a guarded file. Relevant impact: may affect preview/export consistency.
Not involved: billing, authentication, and data retention. Proposed validation: ...
```

## Migration Workflow

When reorganizing docs:

1. Inventory files with paths, sizes, and obvious roles.
2. Identify heavily referenced documents before moving them.
3. Prefer compatibility for files with many deep links or section references.
4. Move low-risk documents into `runbooks/`, `research/`, or `archive/`.
5. Add entrypoint files before deleting or moving content.
6. Update references in docs and code comments.
7. Run a missing-link scan for local `docs/*.md` links.
8. Summarize what moved and what stayed for compatibility.

If a large document is heavily cited, keep it in place and add it to the correct index instead of moving it.

## Missing-Link Scan

Use a lightweight check like this after path changes:

```bash
python3 - <<'PY'
from pathlib import Path
import re

files = [Path("README.md")] if Path("README.md").exists() else []
files += list(Path("docs").rglob("*.md"))

pat = re.compile(r'`?(docs/[A-Za-z0-9_./\\-]+\\.md)`?')
missing = []

for f in files:
    text = f.read_text(encoding="utf-8", errors="ignore")
    for m in pat.finditer(text):
        ref = m.group(1)
        if not Path(ref).exists():
            missing.append((str(f), ref))

for source, ref in missing:
    print(f"{source}: missing {ref}")
print("missing_count", len(missing))
PY
```

## Deliverable Checklist

When asked to implement this structure, aim to deliver:

- `docs/README.md`
- `docs/decisions_index.md`
- `docs/specs/README.md`
- `docs/runbooks/README.md`
- `docs/research/README.md`
- `docs/research/reference_index.md` when research/reference material exists
- `docs/archive/README.md`
- Updated cross-references
- A final summary of the new reading path

Keep all language project-neutral unless the user asks for project-specific wording.
