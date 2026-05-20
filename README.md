# Claude Onboarding

A 30-minute, in-person setup that gets Claude wired into anyone's workflow. Built by [PTW Consulting](https://ptwconsultingllc.com).

## What this is

Most people who install Claude poke around for a week, never set up memory or integrations, and quit. Setup is the activation cliff.

This is the setup we'd run in person. A short prep checklist a few days ahead, then a single 30-minute session: install one terminal, drop in a permissions allowlist, connect the integrations you actually use, and seed Claude's memory with your voice and your stack.

When the session ends, Claude drafts in your voice, knows where your files live, and can work with Gmail / Outlook / Drive / Calendar / Slack.

## Contents

- **A pre-session checklist** so the 30 minutes isn't burned on installs
- **An in-person script** (`skills/ptw-onboard/SKILL.md`) the operator reads off
- **Memory templates** -- generic versions of the files PTW uses for its own work
- **A permissions allowlist** so Claude stops asking to approve every command
- **Setup cards** for Google Workspace, Microsoft 365, Slack, Chrome, and desktop control
- **The `humanizer` skill** -- strips AI tics from anything Claude writes
- **Two optional conversion hooks** that only work if a PTW operator installed this for you (see below)

## What's on your machine, what's optional, what gets shared

Worth being clear up front.

**Stays on your machine:**

- Memory files (profile, writing style, project notes, session log) live in `~/.claude/projects/.../memory/` -- local files on your laptop. The skill never sends them anywhere.
- The settings allowlist is a local config file.
- Skills are local files. They don't phone home.

**Optional -- only if you set it up:**

- **Cloud backup of memory.** Part 7 of the setup script symlinks your memory directory into a cloud folder you already use (iCloud, OneDrive, Drive, or Dropbox), so a dead laptop doesn't lose Claude's memory of your work. It goes to *your* cloud account, in a folder you choose. PTW doesn't see it. Skip it if you don't want it.
- **Integrations (MCPs).** Gmail, Calendar, Drive, Outlook, OneDrive, Slack -- each one you connect goes through the normal OAuth flow with the provider. PTW gets no access.

**Gets shared with your PTW operator -- only if a PTW operator installed this for you and you used the optional hooks:**

- **`/ask-pete`** -- when you type this command and confirm the draft, Claude sends *only the message you confirmed* to the operator's Slack (or email). It doesn't send memory files, conversation history, or anything else you didn't approve.
- **14-day and 30-day check-in summaries** -- short notes (under 200 words each) summarizing how you've been using Claude, based on your local session log. Something like *"used Calendar 18 times this week, drafted 6 emails through the humanizer, asked /ask-pete twice about contract templates."* They don't include the contents of your emails, Slack messages, files, or conversations with Claude. The full prompt is at `skills/ptw-onboard/crons/check-in-prompts.md` if you want to see exactly what gets sent.

If you self-installed this without a PTW operator, the `/ask-pete` command and the check-ins fail safely -- they have nowhere to send and tell you so.

**Conversations with Claude itself** are covered by Anthropic's terms for whichever Claude plan you're on. That's separate from this skill.

## Who this is for

- SMB owners and operators
- Solo professionals (attorneys, accountants, consultants)
- Anyone who's tried Claude and bounced off the setup

If you're a developer who lives in VS Code, this is probably more handholding than you need. The memory templates and setup cards are still useful.

## Getting started

### Option A: A PTW operator is running this with you in person

You don't need to read further. They'll send a prep email a few days ahead with the checklist, then walk you through it.

### Option B: You found this on the internet and want to self-install

Most of it works standalone. The parts that don't (the `/ask-pete` command, the check-in summaries) just fail safely.

```sh
git clone https://github.com/ptw-consulting/claude-onboarding.git
cd claude-onboarding

# Read the prep checklist first
open prep-email/pre-session-checklist.md

# When you're ready, walk through the script
open skills/ptw-onboard/SKILL.md
```

### One convention worth knowing

Claude memory is keyed off the working directory you launch from. **Pick one directory and launch from there always** -- different launch points create split-brain memory. The script sets up `~/Claude` as that directory and adds a `work` alias (zsh on Mac, PowerShell function on Windows) that drops you there and launches Claude.

If you self-installed and want a real setup, [find PTW at ptwconsultingllc.com](https://ptwconsultingllc.com).

## Repo layout

```
claude-onboarding/
├── prep-email/                  # What to send the client a few days ahead
├── skills/
│   ├── ptw-onboard/             # The 30-min in-person script + templates
│   └── humanizer/               # Voice-calibration skill, ships standalone
└── LICENSE
```

## License

MIT. Fork it, adapt it to your own consulting practice. If you build something similar, I'd love to see it.
