---
name: dependency-automation
version: "1.0"
last_updated: 2026-08-07
id: dependency-automation
one_line_purpose: Review Renovate configuration and automated dependency updates.
entry_point: docs/skills/dependency-automation/SKILL.md
category: ci-ops
mcp_compliance_level: partial
optimization_status: draft
status: active
dependencies: []
tags: [renovate, dependencies, automation, automerge]
description: >-
  Describes the Renovate configuration, automerge workflow, and
  authentication model for automated updates. Use when changing dependency
  automation config or triaging an automated update pull request.
metadata:
  type: procedure
  source-of-truth:
    - renovate.json
    - .github/workflows/renovate-automerge.yml
---

# Dependency automation

## Procedure

1. Read the repository configuration and the affected workflow.
2. Validate configuration changes with the repository's configured validator.
3. Preserve the configured authentication model; never add personal access
   tokens or credentials.
4. Confirm the pull request targets the development branch.
5. Run the default gate.

```bash
just check
pre-commit run --all-files
```

Do not document an automation rule until it is present in source configuration.

## When to Use

Use for Renovate or dependency-automation behavior.

## When NOT to Use

Do not use for Manual package changes that automation does not own.

## Core Process

Read configuration, validate it, and preserve the configured auth model.

## Common Rationalizations

- "A shortcut is harmless." Follow the source-of-truth and verification rules instead.

## Red Flags

- Adding tokens or documenting rules absent from source.
- Adding a second Renovate config file. Renovate stops at the first config
  file it finds in its fixed resolution order (`renovate.json` →
  `renovate.jsonc` → `renovate.json5` → `.github/renovate.json` →
  `.github/renovate.jsonc` → `.github/renovate.json5`); a root `renovate.json`
  silently shadows any `.github/renovate*` file and CI path filters scoped to
  the shadowed file never validate the config actually in effect (#1006).
  Keep exactly one canonical file — the repo root `renovate.json`.

## Verification

- [ ] The selected source and focused command were checked.
- [ ] The repository default gate passes.
