---
name: Filesystem layout
description: Where the user's work lives on their machine and in the cloud — use to find the right home for new files and to look up existing ones
type: reference
---

| Type | Location |
|---|---|
| Email | {{Gmail | Outlook}} ({{web | desktop app}}) |
| Calendar | {{Google Calendar | Outlook Calendar}} |
| Cloud files | {{Drive | OneDrive | Dropbox | iCloud}} at `{{path}}` |
| Local files | `~/Documents/{{...}}` |
| Notes | {{Notion | Apple Notes | OneNote | other}} |
| Messaging | {{Slack | Teams | iMessage}} |

## Active projects

- {{Project name}} → {{location}}
- {{Project name}} → {{location}}

## Backup

Claude memory is symlinked from `~/.claude/projects/{{project_dir}}/memory/` to `~/Claude-Backup/memory/`, which is synced via {{iCloud | OneDrive | Drive | Dropbox}}.

## How to apply

- When creating a new file for an existing project, place it in the right location from this table without asking.
- When the user references a project, use this table to find it instead of searching blindly.
- If a new location is introduced, append it here.
