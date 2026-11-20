---
name: pr-create
description: |
  Create a pull request for the current branch, filled from the diff and the repo's PR template.
  Invoke on "/pr-create <base-branch>", "create a PR", "open a pull request", "tạo PR".
---

Create a PR for the current branch. Base branch: $ARGUMENTS

If $ARGUMENTS is empty, ask "What is the base branch?" and wait.

## Steps

1. `gh pr list --head <current-branch> --json number,url --jq '.[0]'`. If a PR exists, show its URL,
   suggest `/pr-update`, stop.
2. `git status -sb`. If the branch has no upstream or is ahead, tell the user it needs pushing and ask
   before running `git push -u origin <branch>`.
3. `git log <base>..HEAD --oneline` and `git diff <base>...HEAD`. Read directly, do not save to a file.
4. Template: `.github/PULL_REQUEST_TEMPLATE.md` if present. Otherwise use sections
   `## Title`, `## Ticket(s)`, `## Why`, `## Changes`, `## How to test`.
5. Ticket number = first run of digits in the branch name; fill the ticket link from it.
6. Fill every section from the diff. Delete template placeholder/example text. No "Generated with Claude Code"
   footer or any AI attribution.
7. **Preview**: show the exact title, base, and body. Wait for an explicit OK. Apply edits the user asks for
   and preview again.
8. Write the body to a temp file in the scratchpad, then
   `gh pr create --base <base> --title "<title>" --body-file <file>`.
9. Return the PR URL.
