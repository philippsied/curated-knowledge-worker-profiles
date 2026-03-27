# Git Conventions

## Commit Messages

- Write in imperative mood: "Add feature" not "Added feature"
- Prefix: feat:, fix:, refactor:, docs:, test:, chore:
- Keep the first line under 72 characters

## Rules

- Do not commit generated files, build artifacts, or secrets.
- Do not amend or force-push without explicit confirmation.
- When asked to commit, stage only files related to the current task.
- Track empty directories with `.gitkeep` — Git ignores empty folders.
- Remove `.gitkeep` once real files exist in the directory.
- For new project structures: place `.gitkeep` in every empty directory BEFORE committing.
- Create a meaningful `.gitignore` BEFORE the first commit — OS files (.DS_Store),
  editor files, secrets (.env), build artifacts.
