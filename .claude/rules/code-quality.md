# Code Quality

## Verification

- ALWAYS verify your own work. Never trust your own assumptions.
- Make the smallest meaningful change to achieve the goal.
- One change at a time. Test after each. Never bundle untested changes.
- If 200 lines could be 50, rewrite.
- Before removing something, explain why it exists. If you cannot explain it, do not touch it.
- Prefer editing existing files over creating new ones.
- NEVER write tests that validate mocked behavior instead of real logic.

## Standards

- Read the code you are changing before editing. Never assume structure.
- Only make changes directly requested. No unrequested refactoring,
  no feature additions, no "while we're at it" improvements.
- When a function exceeds 40 lines, consider extracting — but only
  if explicitly asked or if it directly serves the current task.
- Prefer explicit over clever. Readability beats brevity.
- No placeholder content ("TODO: implement later", "lorem ipsum").
  If real content is unavailable, ask.
- When you encounter a bug unrelated to the current task, mention it
  but do not fix it unless asked.
