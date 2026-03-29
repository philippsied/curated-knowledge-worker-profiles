---
name: plugin-profile
description: >-
  Manages Claude Code plugin profiles and project-level skills to control
  context window usage. Switch between predefined plugin/skill sets.

  Use /plugin-profile <command> to manage profiles.

  Commands:
  - /plugin-profile init — Interactive project setup: profile, skills, git, speckit, ralph
  - /plugin-profile list — Show available profiles with plugin/skill counts
  - /plugin-profile show — Show currently active plugins
  - /plugin-profile skills — Show skills in the current project
  - /plugin-profile backup — Save current settings as timestamped backup
  - /plugin-profile <name> — Apply a profile globally (minimal, highquality, saas, uiux, landingpage, founder, research, fullstack, all)

  Use when:
  - Starting work on a specific project type
  - Bootstrapping a new project (init)
  - Context window feels bloated or slow
  - Switching between different work contexts
  - Need design/founder/landing-page specific tooling
---

# Plugin Profile Manager

Manages plugin profiles by modifying `enabledPlugins` in `~/.claude/settings.json` and skill symlinks in the current project's `.claude/skills/` directory.

## Setup

**Profiles file:** `~/.claude/skills/plugin-profile/profiles.json`
**Settings file:** `~/.claude/settings.json`
**Skills source:** `~/.agents/skills/` (global skill repository)
**Skills target:** `.claude/skills/` (per project, managed symlinks)

## Profile Hierarchy

```
minimal (5) ─ Core Essentials
├── highquality (14) ─ Universal Best-of Plugins
│   ├── saas (16) ─ Web SaaS Development
│   │   └── fullstack (34) ─ Full Engineering Stack
│   ├── uiux (21) ─ UI/UX Design Focus
│   │   └── landingpage (26) ─ Landing Page Creation
│   └── founder (24) ─ Startup/Founder
├── research (9) ─ Research Workflow
└── all (~72) ─ Everything
```

## Command Routing

Parse the user's argument after `/plugin-profile`:

| Argument | Action |
|----------|--------|
| `init` | Interactive project setup: profile, skills, git, speckit, ralph |
| `list` | Show all profiles with plugin + skill counts |
| `show` | Show active plugins grouped by marketplace |
| `skills` | Show skills in current project |
| `backup` | Create timestamped backup |
| No argument | Show help text with hierarchy diagram |
| Any other string | Treat as profile name to apply globally |

## Command: `list`

1. Read `~/.claude/skills/plugin-profile/profiles.json`
2. Read `~/.claude/settings.json` and count currently active plugins (value === true)
3. For each profile, resolve the full plugin list (following `extends`) and the full skills list
4. Display a table:

```
| Profile     | Plugins | Skills | Description                              | Active? |
|-------------|---------|--------|------------------------------------------|---------|
| minimal     | 5       | 0      | Core essentials only                     |         |
| highquality | 14      | 0      | Universal curated best-of set            |         |
| saas        | 16      | 2      | Next.js/Vercel SaaS development          | ← current |
| uiux        | 21      | 3      | UI/UX Design — colors, typography, a11y  |         |
| landingpage | 26      | 5      | Landing Page — design + content + SEO    |         |
| founder     | 24      | 1      | Startup/Founder — business, product      |         |
| research    | 9       | 0      | Research and analysis workflow            |         |
| fullstack   | 34      | 2      | Full engineering stack                   |         |
| all         | ~72     | all    | Everything enabled                       |         |
```

To determine "Active?": compare the set of currently enabled plugins against each profile's resolved plugin list. Mark the closest match.

## Command: `show`

1. Read `~/.claude/settings.json`
2. Extract all keys from `enabledPlugins` where value is `true`
3. Group by marketplace (the part after `@`)
4. Display grouped list with total count

Example output:
```
Active plugins (16):

claude-plugins-official (7):
  - commit-commands
  - semgrep
  - explanatory-output-style
  - claude-md-management
  - vercel
  - playwright
  - code-review

voltagent-subagents (2):
  - voltagent-core-dev
  - voltagent-qa-sec

[...]
```

## Command: `skills`

Show skills in the current project's `.claude/skills/` directory:

1. List all entries in `.claude/skills/` (current working directory)
2. For each entry, determine if it's:
   - **Managed**: A symlink pointing to `~/.agents/skills/` → created by profile manager
   - **Manual**: A directory or symlink pointing elsewhere → NOT managed
3. List available skills in `~/.agents/skills/` (all directories there)
4. Display:

```
Skills in .claude/skills/ (3):
  Managed (by profile):
    ✓ ui-ux-pro-max → ~/.agents/skills/ui-ux-pro-max
    ✓ seo-audit → ~/.agents/skills/seo-audit
    ✓ vercel-composition-patterns → ~/.agents/skills/vercel-composition-patterns
  Manual (not managed):
    ○ my-custom-skill (directory)

Available in ~/.agents/skills/ (6):
    audit-saas-gtm
    next-best-practices
    seo-audit (active)
    ui-ux-pro-max (active)
    vercel-composition-patterns (active)
    [...]
```

## Command: `backup`

1. Read `~/.claude/settings.json`
2. Generate timestamp: YYYYMMDD-HHmmss format
3. Write contents to `~/.claude/settings.backup.<timestamp>.json`
4. Confirm: "Backup saved to ~/.claude/settings.backup.<timestamp>.json"

## Command: `<profile-name>` (Apply Profile)

This is the main operation. Follow these steps EXACTLY:

### Step 1: Validate profile name

Read `~/.claude/skills/plugin-profile/profiles.json`. If the profile name is not found in `profiles`, show available profiles and abort.

### Step 2: Create automatic backup

Before ANY changes, create a backup (same as `backup` command). This is mandatory and cannot be skipped.

### Step 3: Resolve profile plugins

Profiles can extend other profiles via the `extends` key. Resolve the full plugin list:

```
function resolvePlugins(profileName, profiles):
    profile = profiles[profileName]
    plugins = Set(profile.plugins)

    if profile.extends:
        parentPlugins = resolvePlugins(profile.extends, profiles)
        plugins = parentPlugins.union(plugins)

    return plugins
```

Special case: If `profile.plugins === "__ALL__"`, collect ALL keys from the current `settings.enabledPlugins` and set them all to `true`.

### Step 4: Update settings.json (plugins)

1. Read `~/.claude/settings.json` and parse as JSON
2. Create new `enabledPlugins` object:
   - Start with ALL existing keys from `settings.enabledPlugins`
   - Set every value to `false`
   - For each plugin in the resolved profile list, set its value to `true`
3. Replace `settings.enabledPlugins` with the new object
4. **CRITICAL: Preserve ALL other keys** (sandbox, filesystem, permissions, extraKnownMarketplaces, effortLevel, autoCompact, etc.) — do NOT modify anything except `enabledPlugins`
5. Write the complete JSON back to `~/.claude/settings.json` with 2-space indentation

### Step 5: Manage project skills (symlinks)

Resolve the full skills list for the profile (following `extends`, same pattern as plugins):

```
function resolveSkills(profileName, profiles):
    profile = profiles[profileName]
    skills = Set(profile.skills || [])

    if profile.extends:
        parentSkills = resolveSkills(profile.extends, profiles)
        skills = parentSkills.union(skills)

    return skills
```

Special case: If `profile.skills === "__ALL__"`, list all directories in `~/.agents/skills/` and link them all.

**Then manage symlinks in the current project:**

1. Create `.claude/skills/` directory if it does not exist: `mkdir -p .claude/skills`
2. **Inventory existing symlinks:**
   - List all entries in `.claude/skills/`
   - Classify each as **managed** (symlink target contains `/.agents/skills/`) or **manual** (everything else)
3. **Calculate diff:**
   - **To add:** Skills in the resolved list that have no managed symlink yet
   - **To remove:** Managed symlinks that are NOT in the resolved list
4. **Create new symlinks:**
   - For each skill to add: `ln -sf ~/.agents/skills/<skill> .claude/skills/<skill>`
   - Before creating, verify the skill exists in `~/.agents/skills/`. If not → warn, skip
5. **Remove stale symlinks:**
   - For each managed symlink to remove: `rm .claude/skills/<skill>`
   - **NEVER touch manual entries** (directories or symlinks to other targets)
6. **If the profile has no `skills` field** (null/undefined, NOT empty array): do NOT modify any symlinks. Leave existing state as-is.
7. **If the profile has `skills: []`** (empty array): remove all managed symlinks, leave manual ones.

### Step 6: Display summary

Show a before/after comparison:

```
Profile applied: uiux

Plugins:
  Before: 16 active → After: 21 active
  Enabled (7): frontend-design, bencium-innovative-ux-designer, design-audit,
               typography, a11y-audit, web-dev-tools, playwright
  Disabled (2): vercel, ...

Skills (.claude/skills/):
  Added (3): ui-ux-pro-max, vercel-composition-patterns, seo-audit
  Removed (0): —
  Manual (1): my-custom-skill (unchanged)

⚠ Plugin changes take effect after restarting Claude Code.
  Skill changes are effective immediately.
  Backup: ~/.claude/settings.backup.<timestamp>.json
  To restore: /plugin-profile all
```

## Error Handling

| Error | Response |
|-------|----------|
| Unknown profile name | "Profile '<name>' not found. Available: minimal, highquality, saas, uiux, landingpage, founder, research, fullstack, all" |
| profiles.json missing or unparseable | "Error: Could not read profiles from ~/.claude/skills/plugin-profile/profiles.json" |
| settings.json parse failure | "Error: Could not parse ~/.claude/settings.json — aborting, no changes made" |
| Backup write fails | "Error: Could not create backup — aborting, no changes made to settings" |
| Plugin in profile not in settings | Add as new key with value `true` (warn but proceed) |
| Skill in profile not in ~/.agents/skills/ | "Warning: Skill '<name>' not found in ~/.agents/skills/ — skipping" |
| .claude/skills/ creation fails | "Warning: Could not create .claude/skills/ — skill management skipped" |
| No argument given | Show help text with hierarchy diagram and available commands |

## Command: `init` (Interactive Project Setup)

This command bootstraps a project with the optimal plugin/skill configuration. It writes to the **project-level** `.claude/settings.json` (not global), achieving per-project plugin isolation.

**How it works:** Project-level `enabledPlugins` replaces the global object entirely (REPLACE behavior). Only the plugins listed in the project settings are loaded. This gives full isolation without touching global settings.

**Prerequisite:** Global `~/.claude/settings.json` must contain an `enabledPlugins` key (can be `{}` or the current state). If absent, project-level plugins are silently ignored.

### Step 1: Project Analysis (automatic, no dialog)

Detect the current state silently:

```
- .git/ exists?           → remember: skip_git_init = true
- .claude/ exists?        → if .claude/settings.json exists: warn user, ask to overwrite
- .claude/skills/ exists? → inventory managed vs manual symlinks
- package.json exists?    → detect framework (next, astro, nuxt, vite, etc.)
```

If `.claude/settings.json` already exists, use `AskUserQuestion` to confirm overwrite:
- "Overwrite" — replace enabledPlugins, keep other keys
- "Abort" — stop init, no changes

### Step 2: AskUserQuestion — Project Type (single-select)

```
"Welcher Projekttyp?"

Options (from profiles.json → projectTypes):
  - SaaS              → profile: saas
  - Landing Page      → profile: landingpage
  - UI/UX             → profile: uiux
  - API / Backend     → profile: highquality
  - Research          → profile: research
  - Full Stack        → profile: fullstack
  - Custom            → profile: minimal
```

### Step 3: AskUserQuestion — Tooling (multi-select)

```
"Welche Tools sollen vorbereitet werden?"

Options:
  - [x] Git init          (pre-selected if .git/ does not exist, hidden if .git/ exists)
  - [ ] Speckit           (specify init .)
  - [ ] Ralph Loop        (autonomous implementation — adds ralph addon plugins)
```

**Ralph + Speckit combination:** Both can be selected together. Ralph is useful for autonomous implementation based on Speckit specs. The dialog must allow this combination.

### Step 4: Resolve profile + addons

1. Look up the profile name from `projectTypes[selectedType].profile`
2. Resolve the full plugin list using `resolvePlugins()` (same as for `<profile-name>` command)
3. Resolve the full skills list using `resolveSkills()`
4. If Ralph Loop was selected:
   - Read `addons.ralph` from `profiles.json`
   - Add its plugins to the resolved set
   - Add its skills to the resolved set
5. Final result: a complete set of plugin names and skill names

### Step 5: Write project files

**IMPORTANT: Sandbox constraint.** Claude Code's built-in sandbox protects `.claude/settings.json` and `.claude/skills/` from direct writes at the OS kernel level. This cannot be disabled. The `init` command must use a two-phase approach:

**Phase A: Generate** — Claude generates all file contents and shell commands
**Phase B: Apply** — User executes a single copy-paste block in their terminal

Execute in this order:

**5a. Generate `.claude/settings.json` content**

Write the complete JSON to a temp file:

```bash
# Claude writes to temp (sandbox-allowed)
Write tool → $TMPDIR/plugin-profile-init-settings.json
```

Content format:
```json
{
  "enabledPlugins": {
    "plugin-a@marketplace-x": true,
    "plugin-b@marketplace-y": true
  }
}
```

Rules:
- **ONLY write `enabledPlugins`** — no sandbox, no permissions, no extraKnownMarketplaces
- Every plugin in the resolved set gets value `true`
- Plugins NOT in the set are simply omitted (not set to false)
- If `.claude/settings.json` already exists and user confirmed overwrite: read existing JSON first, generate new JSON with only `enabledPlugins` replaced, preserve all other keys
- Write with 2-space indentation

**5b. Generate shell commands block**

Produce a single, ready-to-paste shell block that the user executes in their terminal:

```bash
# /plugin-profile init — <projectType> [+ ralph]
mkdir -p .claude/skills
cp "$TMPDIR/plugin-profile-init-settings.json" .claude/settings.json
ln -sf ~/.agents/skills/<skill-1> .claude/skills/<skill-1>
ln -sf ~/.agents/skills/<skill-2> .claude/skills/<skill-2>
# ... one ln per skill
```

Add git init and speckit if selected:
```bash
git init                    # only if selected + .git/ absent
specify init .              # only if selected
```

Present the complete block to the user with clear instructions:
```
Führe diesen Block in deinem Terminal aus:
```

**5c. Wait for user confirmation**

After the user executes the commands, verify:
- `ls -la .claude/settings.json` — file exists
- `ls -la .claude/skills/` — symlinks exist and point correctly
- `readlink .claude/skills/*` — targets are correct

### Step 6: Display summary

After the user confirms execution, display:

```
Project initialized: uiux + ralph

Plugins (.claude/settings.json):
  Profile: uiux (21 plugins)
  Addon: ralph (+1 plugin)
  Total: 22 plugins active (project-level, replaces global)

Skills (.claude/skills/):
  Added (3): ui-ux-pro-max, vercel-composition-patterns, seo-audit
  Manual (0): —

Tooling:
  Git: already initialized / initialized / skipped
  Speckit: initialized / skipped
  Ralph Loop: plugins loaded / skipped

⚠ Starte Claude Code neu, damit die Plugin-Änderungen wirksam werden.
  Skills sind sofort aktiv.
  Aktive Plugins prüfen: /plugin-profile show
  Projekt-Isolation aufheben: enabledPlugins aus .claude/settings.json entfernen
```

### Init Error Handling

| Error | Response |
|-------|----------|
| Unknown project type | Show available types from projectTypes and abort |
| Addon not found in profiles.json | "Warning: Addon '<name>' not found — skipping" |
| .claude/settings.json exists, user says Abort | "Init aborted. No changes made." |
| specify command not found | "Warning: 'specify' not found — skipping speckit init. Install with: npm i -g @specifydev/cli" |
| Skill not found in ~/.agents/skills/ | "Warning: Skill '<name>' not found — skipping" |

## Important Notes

- Changes to project `settings.json` require restarting Claude Code in that project
- Skill symlink changes are effective immediately (no restart needed)
- The `extraKnownMarketplaces` section is NEVER modified — marketplace registrations stay intact
- **Project-level `enabledPlugins` replaces global entirely** — only listed plugins are active
- Global settings remain the fallback for projects without their own `.claude/settings.json`
- Use `/plugin-profile all` to restore everything globally if something goes wrong
- To remove project-level isolation: delete `enabledPlugins` from `.claude/settings.json`
- **Only symlinks pointing to `~/.agents/skills/` are managed.** All other entries in `.claude/skills/` are left untouched.
