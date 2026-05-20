# Claude Onboarding

A 30-minute, sit-down setup that gets a non-technical user from "I downloaded Claude" to "Claude is wired into my actual workflow." Built by [PTW Consulting](https://ptwconsultingllc.com).

## What this is

Most people who install Claude poke around for a week, never set up memory, never connect integrations, and quit. Setup is the activation cliff.

This is the setup we'd run in person. It's opinionated: a prep checklist a few days ahead, then a single 30-minute session where we install one terminal, configure a sensible permissions allowlist, connect the integrations you actually use, and seed Claude's memory with your voice, your stack, and your work.

When the session ends, you have a Claude that drafts emails in your voice, knows where your files live, and can talk to Gmail / Outlook / Drive / Calendar / Slack on your behalf.

## What you get

- **A pre-session checklist** so the 30 minutes isn't burned on installs
- **A guided in-person script** (`skills/ptw-onboard/SKILL.md`) that walks the operator through the setup step by step
- **Memory templates** — generic versions of the files PTW uses for its own work, ready to fill in
- **A permissions allowlist** so Claude stops asking you to approve every command
- **Setup cards** for Google Workspace, Microsoft 365, Slack, Chrome, and desktop control
- **The `humanizer` skill** — strips AI tics from any text Claude writes, immediately makes drafted emails sound like you
- **Two optional conversion hooks** that work only if you got this from a PTW operator (see "What gets shared" below)

## What's on your machine, what's optional, what gets shared

Worth being clear about this up front.

**Stays on your machine:**

- Your memory files (your profile, your writing style, your project notes, your session log) live in `~/.claude/projects/.../memory/` — local files on your laptop. The skill never sends them anywhere.
- The settings allowlist is a local config file.
- Skills are local files. They don't phone home.

**Optional — only if you set it up:**

- **Cloud backup of memory.** Part 7 of the setup script offers to symlink your memory directory into a cloud folder you already use (iCloud, OneDrive, Drive, or Dropbox). This is so a dead laptop doesn't lose Claude's memory of your work. It goes to *your* cloud account, in a folder you choose. PTW doesn't see it. Skip this step if you don't want it.
- **Integrations (MCPs).** Gmail, Calendar, Drive, Outlook, OneDrive, Slack — each one you connect goes through the normal OAuth flow with the provider. Permissions stay between you, the provider, and Anthropic. PTW doesn't get access to any of them.

**Gets shared with your PTW operator — only if a PTW operator installed this for you and you used the optional hooks:**

- **`/ask-pete`** — when you type this command and confirm the draft, Claude sends *only the message you confirmed* to the operator's Slack (or email). It does not send memory files, conversation history, or anything you didn't approve.
- **14-day and 30-day check-in summaries** — short notes (under 200 words each) that summarize *how you've been using Claude* based on your local session log and usage patterns. They include things like *"used Calendar MCP 18 times this week, drafted 6 emails using the humanizer skill, asked /ask-pete twice about contract templates."* They do not include the contents of your emails, your Slack messages, your files, or your conversations with Claude. The full prompt the check-in runs is in `skills/ptw-onboard/crons/check-in-prompts.md` — read it if you want to see exactly what it looks at and what it sends.

If you self-installed this without a PTW operator, the `/ask-pete` command and the check-ins fail safely — they have nowhere to send to and tell you so.

**Conversations with Claude itself** are governed by Anthropic's terms for whichever plan you're on. That's separate from this skill — it's how Claude works regardless of how you set it up.

## Who this is for

- SMB owners and operators
- Solo professionals (attorneys, accountants, consultants)
- Anyone who's tried Claude and bounced off the setup

If you're a developer who lives in VS Code, this is probably more handholding than you need — but the memory templates and setup cards are still useful.

## Getting started

### Option A: A PTW operator is running this with you in person

You don't need to read further. They'll send you a prep email a few days ahead with the checklist, then walk you through it.

### Option B: You found this on the internet and want to self-install

You can. Most of it works standalone — the parts that don't (the `/ask-pete` command, the check-in summaries) just fail safely.

```sh
git clone https://github.com/ptw-consulting/claude-onboarding.git
cd claude-onboarding

# Read the prep checklist first
open prep-email/pre-session-checklist.md

# When you're ready, walk through the script
open skills/ptw-onboard/SKILL.md
```

### One convention worth knowing

Claude memory is keyed off the working directory you launch from. **Pick one directory and launch from there always** — different launch points create split-brain memory. The script sets up `~/Claude` as that directory and adds a `work` alias (zsh on Mac, PowerShell function on Windows) that drops you there and launches Claude in one step.

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

MIT. Use it, fork it, adapt it to your own consulting practice. If you're a fellow consultant building something similar, we'd love to see what you make.
