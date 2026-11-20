---
name: issue
argument-hint: "create <summary> | update <n> <what to change> | comment <n> <what to say> | view <n>"
description: |
  Create, update, comment on, or read a GitHub issue with `gh`, always previewing before posting.
  Invoke on "/issue ...", "create an issue", "open a ticket", "update issue 123", "comment on issue 123",
  "tạo issue", "sửa issue 123", "comment vào issue 123", "trả lời comment trong issue".
---

Manage a GitHub issue. Request: $ARGUMENTS

## Target repo

In order: repo the user names (`-R <owner/repo>`) → `Issues repo:` line in the project's `CLAUDE.md` →
the current repo (`gh repo view --json nameWithOwner`).
Pass `-R <repo>` on every `gh issue` call.

## Rules (all actions)

- **Preview before posting**: show the exact title / body / comment text that will be sent and wait for an
  explicit OK. Edits → preview again. Never post on assumption.
- **Language**: write in the language of the issue (create: the language the user asks in, unless told
  otherwise; comment: the language of the comment being answered).
- No "Generated with Claude Code" footer or any AI attribution.
- Send text via `--body-file <scratchpad file>`, never inline `--body` (quoting breaks Markdown).
- Never close, reopen, delete, transfer, or change assignees/labels/milestone unless asked.
- Do not `@mention` anyone unless the user asks.

## view <n>

`gh issue view <n> -R <repo> --json number,title,body,state,labels,assignees,comments,url`.
Summarise: title, state, what is asked, open questions, latest comments.

## create

1. Gather: what is wrong / wanted, where (screen, API, file), how to reproduce, expected vs actual.
   Ask only for what is missing and can't be found in the code or conversation.
2. Search duplicates: `gh issue list -R <repo> --state all --search "<keywords>" --limit 10`.
   Show likely duplicates and ask whether to continue.
3. Body: use `.github/ISSUE_TEMPLATE/*` if present; otherwise
   `## Summary`, `## Steps to reproduce` (bugs), `## Expected`, `## Actual`, `## Notes`.
4. Preview → OK → `gh issue create -R <repo> --title "<title>" --body-file <file>`
   (add `--label` / `--assignee` only if the user asked).
5. Return the URL.

## update <n>

1. `gh issue view <n> -R <repo> --json title,body`.
2. Change only what was asked; keep the rest of the body verbatim.
3. Preview as a diff of title/body → OK → `gh issue edit <n> -R <repo> [--title ...] --body-file <file>`.
4. Return the URL.

## comment <n>

1. `gh issue view <n> -R <repo> --json title,body,comments`. If replying to a specific comment, quote the
   relevant line(s) with `>`.
2. Draft the comment: answer directly, cite code as `path:line` or a permalink when relevant.
3. Preview → OK → `gh issue comment <n> -R <repo> --body-file <file>`.
4. Return the comment URL.
