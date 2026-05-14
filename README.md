# Keycloak MUI Theme Skill

This repository contains a Codex skill for creating, modifying, and reviewing Keycloak themes built with Material UI and Storybook.

## What It Enforces

- Default theme scope is `login` only.
- Other Keycloak scopes, such as `admin`, `account`, or `email`, are supported only when the user explicitly requests them.
- New themes must be built from the correct official Keycloak source contract for the target Keycloak version.
- FTL-based scopes such as `login` and `email` must follow official FTL files.
- React console-based scopes such as `admin` and `account` must follow official React console routes, shell, page modules, state models, and API interaction patterns.
- If a request does not specify a concrete FTL file or subset for an FTL-based scope, all official FTL files for the selected scope should be implemented.
- Admin/account themes are not judged by FTL coverage. They require Storybook coverage for the implemented official React console modules.
- UI should use MUI-native components, colors, typography, spacing, and `@mui/icons-material` icons.
- Buttons on the same page or logical section should use consistent sizing.
- MUI dependencies and components should stay on the latest stable version compatible with the project.
- Every implemented or modified theme scope must include Storybook coverage.
- Every implemented official FTL page or React console module should have corresponding Storybook stories.
- Storybook should use the latest stable release channel, with matching `storybook` and `@storybook/*` package majors.
- Stories should cover official Keycloak original page structures and states, not only custom happy paths.
- Admin Storybook stories should use `Admin/<Domain>/<Page>/<State>` naming and include shell, route, permission, data, form, error, and destructive-action states.
- GitHub Actions should use current action versions and Node24-compatible runtimes to avoid Node20 deprecation warnings.

## Skill Layout

```text
keycloak-mui-theme/
  SKILL.md
  agents/
    openai.yaml
```

`SKILL.md` contains the operational rules Codex loads when the skill is triggered.

`agents/openai.yaml` contains UI-facing metadata such as the display name, short description, and default prompt.

OpenCode reads the same `SKILL.md` file. It ignores `agents/openai.yaml`, which is Codex-specific metadata.

## Codex Installation

### Windows

Install the skill by copying the `keycloak-mui-theme` folder into your Codex skills directory:

```powershell
Copy-Item -Recurse -Force `
  -LiteralPath .\keycloak-mui-theme `
  -Destination "$HOME\.codex\skills\keycloak-mui-theme"
```

### Linux/macOS

Install the skill by copying the `keycloak-mui-theme` folder into your Codex skills directory:

```bash
mkdir -p "$HOME/.codex/skills"
cp -R ./keycloak-mui-theme "$HOME/.codex/skills/keycloak-mui-theme"
```

Restart Codex after installation so it can discover the new skill.

## OpenCode Installation

OpenCode discovers skills from project-local and global skill directories. Use one of these locations:

- Project-local: `.opencode/skills/keycloak-mui-theme/SKILL.md`
- Global: `~/.config/opencode/skills/keycloak-mui-theme/SKILL.md`
- Agent-compatible project-local: `.agents/skills/keycloak-mui-theme/SKILL.md`
- Agent-compatible global: `~/.agents/skills/keycloak-mui-theme/SKILL.md`

### Windows

For a global OpenCode install:

```powershell
New-Item -ItemType Directory -Force `
  -Path "$HOME\.config\opencode\skills" | Out-Null

Copy-Item -Recurse -Force `
  -LiteralPath .\keycloak-mui-theme `
  -Destination "$HOME\.config\opencode\skills\keycloak-mui-theme"
```

For a project-local OpenCode install:

```powershell
New-Item -ItemType Directory -Force `
  -Path ".\.opencode\skills" | Out-Null

Copy-Item -Recurse -Force `
  -LiteralPath .\keycloak-mui-theme `
  -Destination ".\.opencode\skills\keycloak-mui-theme"
```

### Linux/macOS

For a global OpenCode install:

```bash
mkdir -p "$HOME/.config/opencode/skills"
cp -R ./keycloak-mui-theme "$HOME/.config/opencode/skills/keycloak-mui-theme"
```

For a project-local OpenCode install:

```bash
mkdir -p ./.opencode/skills
cp -R ./keycloak-mui-theme ./.opencode/skills/keycloak-mui-theme
```

If OpenCode skill permissions are restricted, allow this skill in `opencode.json`:

```json
{
  "permission": {
    "skill": {
      "keycloak-mui-theme": "allow"
    }
  }
}
```

## Updating An Installed Copy

After editing the source skill, sync it into the installed Codex location.

### Windows

```powershell
Copy-Item -Force `
  -LiteralPath .\keycloak-mui-theme\SKILL.md `
  -Destination "$HOME\.codex\skills\keycloak-mui-theme\SKILL.md"

Copy-Item -Force `
  -LiteralPath .\keycloak-mui-theme\agents\openai.yaml `
  -Destination "$HOME\.codex\skills\keycloak-mui-theme\agents\openai.yaml"
```

### Linux/macOS

```bash
cp ./keycloak-mui-theme/SKILL.md "$HOME/.codex/skills/keycloak-mui-theme/SKILL.md"
cp ./keycloak-mui-theme/agents/openai.yaml "$HOME/.codex/skills/keycloak-mui-theme/agents/openai.yaml"
```

For OpenCode, sync the skill folder to the OpenCode location you chose. For example:

```bash
cp ./keycloak-mui-theme/SKILL.md "$HOME/.config/opencode/skills/keycloak-mui-theme/SKILL.md"
```

Restart Codex or OpenCode if you need updated metadata, trigger descriptions, or skill discovery to be picked up immediately.

## Example Requests

- "Create a Keycloak login theme with MUI."
- "Review this Keycloakify login theme for MUI consistency."
- "Add admin theme support and generate all Storybook stories."
- "Upgrade Storybook for this Keycloak theme project."
- "Update GitHub Actions to avoid Node20 deprecation warnings."

## Validation

At minimum, confirm that:

- `keycloak-mui-theme/SKILL.md` exists.
- The frontmatter contains `name: keycloak-mui-theme` and a clear `description`.
- `keycloak-mui-theme/agents/openai.yaml` exists.
- No template TODO markers remain.

If the skill-creator validation dependencies are available, run:

```powershell
python C:\Users\admin\.codex\skills\.system\skill-creator\scripts\quick_validate.py .\keycloak-mui-theme
```
