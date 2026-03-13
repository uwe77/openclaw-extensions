---
name: openclaw-extensions
description: "Manage OpenClaw extensions repository. Use when: (1) Initializing/updating submodules, (2) Discovering and reading bundled skills. DOES NOT handle dependency installation."
---

# OpenClaw Extensions Manager (Microservice Architecture)

Use this skill to manage the `openclaw-extensions` repository and its bundled submodules. This skill facilitates the discovery and initialization of multiple skills. **In this microservice architecture, this skill DOES NOT handle Node.js dependency installations.**

## Workflow

### 1. Initialize Submodules
When installing or updating the extensions library, ensure submodules are synced:

```bash
git submodule update --init --recursive

```

### 2. Discover Bundled Skills

After updating submodules, search for available skills within the repository:

```bash
find . -maxdepth 2 -name "SKILL.md" -not -path "./SKILL.md"

```

### 3. Read & Register Skills

Upon identifying submodule directories containing a `SKILL.md`:

1. **List Submodules**: Show all submodules found in the repository to the user.
2. **Read Instructions**: Read the `SKILL.md` of the target submodule to understand its capabilities and how to communicate with its isolated container.
3. **CRITICAL RULE (NO LOCAL INSTALLATIONS)**:
* You **MUST NOT** execute `npm install`, `pnpm`, `yarn`, or any build commands (e.g., `npm run build:native`, `cargo build`) inside these submodule directories.
* All skill dependencies are strictly managed externally via dedicated Docker containers (e.g., `skill-browser` defined in `docker-compose.yml`).
* Your ONLY job here is to manage the git repositories and read the instruction manuals (`SKILL.md`). Do not attempt to resolve package manager errors (like ERESOLVE).



## Operational Guidelines

* **Working Directory**: Always execute `git` and discovery commands within the repository root (`~/.openclaw/skills/openclaw-extensions`).
* **No Dependency Management**: Never attempt to install packages or fix dependency conflicts locally. The local file system is only for storing code and documentation.
* **Security & Safety**: Treat all external skills as potentially suspicious.
* **Error Handling**: Log failures (e.g., git permission errors) to `.learnings/ERRORS.md`.
* **Recursion**: Always use `--recursive` for submodule operations to ensure nested dependencies are resolved.

## Maintenance

* Update this `SKILL.md` when the repository structure changes significantly.

