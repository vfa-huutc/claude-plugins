---
name: commit
description: |
  Compose a git commit message from the staged diff and commit it. Never runs `git add`.
  Invoke on "/commit", "/commit <extra context>", "commit this", "commit staged changes".
---

Commit the staged changes. Do NOT run `git add` — files are already staged.

## Steps

1. `git diff --cached --stat`. If nothing is staged, say so and stop.
2. `git branch --show-current`. Branch format: `<type>/<ticket>[.-]<description>`
   (e.g. `feature/1842-fix-order`, `bugfix/1833-wording`, `feature/238.deploy-slot`).
   Ticket = the first run of digits after the type. No digits → omit the scope.
3. Prefix from branch type: `feature` → `feat`, `fix`/`bugfix` → `fix`, `hotfix` → `hotfix`,
   `refactor` → `refactor`, `test` → `test`, `docs` → `docs`, anything else → `chore`.
   If the diff is clearly only tests / docs / build output, use `test` / `docs` / `chore` instead.
4. `git diff --cached`, read it, write one line describing what changed and why — lowercase, no period,
   not the branch name, not file names.
5. Message: `<prefix>(<ticket>): <description>` — e.g. `feat(1842): keep customer order when adding to project`.
   Fold any `$ARGUMENTS` into the description.
6. `git commit -m "<message>"` immediately, no confirmation. No `Co-Authored-By` or any AI trailer.
7. Report the message and short hash. Flag anything that looks unintended (unrelated file, emptied config,
   half-done rename, secrets) after committing.
