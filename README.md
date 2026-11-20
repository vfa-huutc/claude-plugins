# claude-plugins

Claude Code plugins for day-to-day git and GitHub work.

## Plugins

| Plugin | Skills |
|---|---|
| `git-workflow` | `commit`, `pr-create`, `pr-update`, `issue` |

## Enable for a whole team

Add to the project's `.claude/settings.json` (merge with existing keys) and commit it:

```json
{
  "extraKnownMarketplaces": {
    "huutc-plugins": {
      "source": { "source": "github", "repo": "vfa-huutc/claude-plugins" }
    }
  },
  "enabledPlugins": {
    "git-workflow@huutc-plugins": true
  }
}
```

Teammates are prompted to install when they open the project and trust the folder.

Manual install:

```
/plugin marketplace add vfa-huutc/claude-plugins
/plugin install git-workflow@huutc-plugins
```

Update: `/plugin marketplace update huutc-plugins`. Bump `version` in `plugin.json` on each release.

## Issue repo

`/issue` targets the current repo. If issues live elsewhere, put a line like
`Issues repo: <owner>/<repo>` in the project's `CLAUDE.md`, or pass `-R <owner>/<repo>`.

## Local test

```
claude --plugin-dir ./plugins/git-workflow
```
