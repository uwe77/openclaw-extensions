---
name: openclaw-extensions
description: "Git-based manager for OpenClaw skills library. Use for submodule sync and skill discovery. DOES NOT handle runtime dependencies."
---

# OpenClaw Extensions Manager (Microservice Mode)

This skill manages the `openclaw-extensions` git repository. It acts as a **Librarian**, not a **Builder**. Its role is to ensure documentation and code are synced so you can read instructions on how to call external Dockerized skills.

## ⚠️ Critical Architecture Constraints

- **READ-ONLY DEPENDENCIES**: You are STRICTLY FORBIDDEN from running `npm install`, `pnpm`, `yarn`, `cargo`, or any build tools here.
- **DOCKER-CENTRIC**: All runtime logic (like browsers or tools) is managed via `docker-compose.yml`. Your job is to find the "Manual" (`SKILL.md`) for those services.
- **NO TROUBLESHOOTING**: If you see `ERESOLVE` or missing package errors, **IGNORE THEM**. These are expected in this isolated architecture.

## Workflow

### 1. Submodule Synchronization
Ensure all skill repositories are pulled and synced to their correct versions:
```bash
git submodule update --init --recursive

```

### 2. Skill Discovery & Registration

Search for available "Instruction Manuals" to learn how to interact with specialized containers:

```bash
find /app/skills/openclaw-extensions -maxdepth 3 -name "SKILL.md" -not -path "/app/skills/openclaw-extensions/SKILL.md"

```

### 3. Service Verification

After reading a `SKILL.md` for a new skill (e.g., `agent-browser`):

1. **Match Container**: Check if the required container (e.g., `skill-browser`) is running using `docker ps`.
2. **Read Instructions**: Follow the execution syntax (e.g., `docker exec ...`) defined in that specific `SKILL.md`.
3. **DO NOT BUILD**: If the `SKILL.md` mentions a "Build" step, **SKIP IT**. Assume the user has already provisioned the container.

## Operational Guidelines

* **Working Directory**: `/app/skills/openclaw-extensions` (within the Docker environment).
* **Git Hygiene**: If a submodule is in a "detached HEAD" state, do not attempt to fix it unless explicitly asked to update versions.
* **Error Logging**: Log any Git-related permission or network issues to `.learnings/ERRORS.md`.
* **Conflict Resolution**: Never attempt to resolve `package-lock.json` or `pnpm-lock.yaml` conflicts.

## Maintenance

Update this manager instruction if the directory structure for extensions changes.