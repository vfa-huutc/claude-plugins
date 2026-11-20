---
name: pr-update
description: |
  Update the title and description of the existing pull request for the current branch from its diff.
  Invoke on "/pr-update <base-branch>", "update the PR description", "refresh the PR body".
---

Update the existing PR for the current branch. Base branch: $ARGUMENTS

If $ARGUMENTS is empty, use the PR's own base (`gh pr view --json baseRefName`).

## Steps

1. `gh pr view --json number,url,title,body,baseRefName`. No PR → "No open PR found for this branch." and stop.
2. `git diff <base>...HEAD`. Read directly, do not save to a file.
3. Template: `.github/PULL_REQUEST_TEMPLATE.md` if present, otherwise keep the structure of the current body.
4. Compose the new title and body. Keep anything in the current body a human wrote that the diff can't
   reproduce (test notes, screenshots, reviewer notes). No AI attribution footer.
5. **Preview**: show the new title and body (or a clear diff vs the current one). Wait for an explicit OK.
6. Write the body to a scratchpad file, then `gh pr edit <n> --title "<title>" --body-file <file>`.
7. Return the PR URL.
