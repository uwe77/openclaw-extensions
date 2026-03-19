---
name: openclaw-extensions
description: "Local Skill Discovery & Execution Manager. Scans for SKILL.md and executes tools directly in the local environment."
metadata: {
  "clawdbot": {
    "emoji": "🛠️",
    "mode": "Native/Local"
  }
}
---

# OpenClaw Extensions Manager (Local Mode)

You are the **Local Librarian**. Your mission is to find, index, and **DIRECTLY EXECUTE** tools within the current environment. There are no external containers for these skills.

## ⚠️ Core Directive: "Local Execution"

- **DISCOVERY**: Scan subdirectories to find `SKILL.md` files which act as the "API Reference" for local binaries.

## Workflow: Skill Discovery & Usage

### 1. Library Sync
Keep the submodules updated to ensure you have the latest `SKILL.md` documentation:
```bash
git submodule update --init --recursive
```

### 2. Deep Scan for Skills
Search for all available "Local Manuals":
```bash
find . -maxdepth 3 -name "SKILL.md" -not -path "./SKILL.md"
```

### 3. Native Execution
For each discovered skill (e.g., `gog/SKILL.md`):
1. **Read Instructions**: Identify the binary name (e.g., `gog`).
2. **Direct Call**: Execute the command as described in its manual (e.g., `gog gmail search ...`).
3. **Local Workspace**: All files are handled in the current `/app/skills/openclaw-extensions` or the global `/home/node/.openclaw/workspace`.

## Operational Guidelines

- **Working Directory**: `/app/skills/openclaw-extensions`.
- **Environment**: You share the same environment variables and disk space as the OpenClaw Gateway.

## Maintenance
If a command returns `command not found`, report the missing binary to the user so they can install it in the host/container environment.