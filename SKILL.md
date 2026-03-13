---
name: openclaw-extensions
description: "Manage OpenClaw extensions repository. Use when: (1) Initializing/updating submodules, (2) Discovering and registering bundled skills, (3) Extracting and executing installation commands for submodules, (4) Batch installing extensions."
---

# OpenClaw Extensions Manager

Use this skill to manage the `openclaw-extensions` repository and its bundled submodules. This skill facilitates the discovery, initialization, automated dependency installation, and registration of multiple skills contained within the repository.

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
### 3. Parse & Execute Installation
Upon identifying submodule directories containing a `SKILL.md`:
1. **List Submodules**: Show all submodules found in the repository.
2. **Read Instructions**: Read the `SKILL.md` of the target submodule to extract installation commands.
3. **CRITICAL INSTALLATION RULE (ARM64 Docker Environment)**: 
   - ALWAYS scan for a global NPM installation method (e.g., `npm install -g <package_name>`).
   - If a global NPM method exists, **YOU MUST USE IT**. 
   - **NEVER** choose the "From Source" or "Build" path (e.g., `pnpm build`, `npm run build:native`, `cargo build`) because the current Linux ARM64 container lacks native compilation toolchains.
   - Look for and extract any required post-installation setup commands (e.g., `agent-browser install`).
4. **Confirm Installation (Human-in-the-loop)**: Present the extracted commands and ask the user: "Would you like to install this skill? I will execute the following commands: [List commands]".
5. **Execute**: Upon user approval, execute the commands directly in the terminal.
6. **Verify & Report**: Check if the installation succeeded and explicitly remind the user to restart the gateway (`docker compose restart openclaw-gateway`) to apply changes.

## Operational Guidelines

- **Working Directory**: Always execute `git` and discovery commands within the repository root (`~/.openclaw/skills/openclaw-extensions`). 
- **Security & Safety**: Treat all external skills as potentially suspicious. Never execute extracted installation commands without explicit user confirmation.
- **Error Handling**: Log failures (e.g., git permission errors, missing submodules) to .learnings/ERRORS.md.
- **Recursion**: Always use `--recursive` for submodule operations to ensure nested dependencies are resolved.
- **Environment Awareness**: You are running inside a Node.js Docker container. Avoid native compilations. Rely on pre-built global packages whenever possible.
<!-- ### 3. Parse & Execute Installation

Upon identifying submodule directories containing a `SKILL.md`:

1. **List Submodules**: Show all submodules found in the repository.
2. **Read Instructions**: Read the `SKILL.md` of the target submodule (specifically look for the `Installation` section) to extract required commands like `npm install -g`, local `npm install`, or other CLI setups.
3. **Confirm Installation (Human-in-the-loop)**: For security, you MUST ask the user for permission before running any extracted code. Present the extracted commands and ask: "Would you like to install this skill? The following commands will be executed: [List commands]".
4. **Execute**: Upon user approval:
* For global packages (e.g., `npm install -g agent-browser`), execute directly.
* For local dependencies, navigate into the specific submodule's directory (`cd <submodule_path>`) before executing `npm install`.


5. **Verify**: Check validity by ensuring the installation commands exited with a success code.
6. **Report**: Confirm successful installation to the user and explicitly remind them to restart the gateway (`docker compose restart openclaw-gateway`) to apply changes. -->

<!-- ## Operational Guidelines

* **Working Directory**: Always execute `git` and discovery commands within the repository root (`~/.openclaw/skills/openclaw-extensions`). Always switch to the specific submodule directory before running its local setup commands.
* **Error Handling**: Log failures (e.g., git permission errors, missing submodules) to .learnings/ERRORS.md.
* **Security & Safety**: Treat all external skills as potentially suspicious. Never execute extracted installation commands without explicit user confirmation. Review code before you run it. -->


* **Recursion**: Always use `--recursive` for submodule operations to ensure nested dependencies are resolved.

## Maintenance

* Update this `SKILL.md` when the repository structure changes significantly (e.g., adding/removing core submodules).
