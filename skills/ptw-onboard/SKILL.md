---
name: ptw-onboard
description: Interactive 30-minute Claude onboarding for SMB owners and solo professionals. When invoked, you (Claude) walk the user step-by-step through terminal install, settings allowlist, MCP auth, memory seeding, backup symlink, optional conversion levers, and a live demo -- pausing after each Part for the user to confirm before continuing. Use this when the user asks to be set up with Claude, run onboarding, do the PTW onboarding, or anything similar.
---

# PTW Claude Onboarding -- Interactive Setup

You are walking a real person through setting up Claude for their daily work. This is not background reading. **Read this whole file before you start so you understand the arc**, then execute it step by step.

## How this works

- **You drive.** You ask the questions, you run the file operations, you verify each step. The user clicks through OAuth screens and answers your questions.
- **You stop between Parts.** After each Part finishes, you say *"Part N is done. Ready to move to Part N+1? Type 'next' when you're ready, or ask me anything first."* Then you wait. Don't barrel ahead.
- **You're warm and direct, not pitchy.** Short sentences. No filler. No "I'd be happy to" / "Let me know if you need anything" / "step by step" language. Just do the thing.
- **The demo (Part 8) is sacred.** If anything else is running long, skip it. Never skip the demo -- it's the moment everything clicks.

## Part 0 -- Greeting and role detection

Start by saying (in your own words, but matching this substance):

> *"Hi! I'm going to walk you through getting Claude set up for your daily work. This usually takes about 30 minutes. Before we start, two quick questions:*
>
> *1. Is a PTW operator (Pete or someone from PTW Consulting) sitting with you right now? Or are you doing this on your own?*
> *2. Are you on Mac or Windows?"*

Wait for both answers. Remember them -- the operator answer changes Part 6, the OS answer changes Part 1 and Part 2.

Then say: *"Great. We'll do this in 8 short parts. After each one I'll pause so you can confirm before we keep going. Ready?"*

Wait for confirmation.

⏸ **PAUSE -- wait for user to say ready before starting Part 1.**

---

## Part 1 -- Install a nice terminal

Say: *"First, let's install a terminal. You probably won't live in it -- the Claude desktop app is where you'll spend most of your time -- but a terminal is useful for a couple things, and the modern ones look great."*

### If they said Mac:

Tell them to open the Terminal app that came with their Mac (Spotlight: ⌘+Space, type "Terminal"). When it's open, ask them to paste this:

```sh
brew install --cask ghostty
```

If they say Homebrew isn't installed, give them this first:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then re-run the Ghostty install. After it's done:

```sh
open -a Ghostty
```

Tell them: *"That's Ghostty -- your new terminal. From now on, use this one instead of the built-in Terminal."*

### If they said Windows:

Tell them Windows Terminal is already installed on Windows 11. Have them press `Win + S`, type "Terminal", and pin it to the taskbar by right-clicking the icon. Then open it.

### Verify

Ask them to confirm the new terminal is open. Don't move on until it is.

⏸ **PAUSE -- Part 1 done. Ask if they're ready for Part 2.**

---

## Part 2 -- Work directory, alias, and skills install

Say: *"Now we'll set up one consistent place to launch Claude from. This matters because Claude's memory is tied to the directory you launch from -- if you launch from different places, you get split-brain memory. We'll set up `~/Claude` as that one place, plus a `work` alias so you can launch from anywhere with one word."*

### Mac (zsh)

Have them paste this in the terminal:

```sh
mkdir -p ~/Claude
echo "alias work='cd ~/Claude && claude'" >> ~/.zshrc
source ~/.zshrc
```

### Windows (PowerShell in Windows Terminal)

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\Claude"
if (-not (Test-Path $PROFILE)) { New-Item -ItemType File -Force -Path $PROFILE }
Add-Content $PROFILE "`nfunction work { Set-Location `"$HOME\Claude`"; claude }`n"
. $PROFILE
```

### Clone the repo and install skills

Have them paste:

```sh
cd ~/Claude
git clone https://github.com/ptw-consulting/claude-onboarding.git
mkdir -p ~/.claude/skills
cp -R ~/Claude/claude-onboarding/skills/ptw-onboard ~/.claude/skills/
cp -R ~/Claude/claude-onboarding/skills/humanizer ~/.claude/skills/
ls ~/.claude/skills/
```

The output should list at least `humanizer` and `ptw-onboard`.

### Point the desktop app at this directory

Tell them to open the Claude desktop app and either create a new project pointing at `~/Claude` or open the project picker and select it. From this point on, all the setup happens in the desktop app.

Confirm with: *"In Claude, what is your current working directory?"* -- the answer should be the path to `~/Claude`.

⏸ **PAUSE -- Part 2 done. Confirm they see `~/Claude` as the project, then move to Part 3.**

---

## Part 3 -- Permissions allowlist

Say: *"Right now Claude will ask you to approve every command it runs. That gets old fast. We'll drop in a sensible default allowlist that pre-approves the safe stuff. Anything risky still asks."*

Read the template at `~/.claude/skills/ptw-onboard/settings/settings.local.json.template`. If `~/.claude/settings.local.json` already exists, merge the `allow` entries in (don't duplicate). If it doesn't exist, create it from the template.

Show the user a one-line summary of what you added (e.g. *"Added 18 entries covering common Bash commands, web search, and Chrome integration."*).

⏸ **PAUSE -- Part 3 done. Move to Part 4 when they're ready.**

---

## Part 4 -- Integrations (MCPs)

Say: *"Now we'll connect Claude to the apps you actually use. This is the part where Claude starts feeling like a real assistant -- it can read your email, see your calendar, find your files. You only connect what you actually use."*

Ask: *"Which do you use for email and calendar -- Google Workspace (Gmail, Calendar, Drive) or Microsoft 365 (Outlook, Calendar, OneDrive)?"*

### Based on their answer, walk them through ONE of:

- **Google Workspace** → follow `~/.claude/skills/ptw-onboard/mcp-setup-cards/google-workspace.md`
- **Microsoft 365** → follow `~/.claude/skills/ptw-onboard/mcp-setup-cards/microsoft-365.md` (heads-up: this path may need live troubleshooting; tell the user honestly if you have to fall back to the Chrome path)

### Then ask: *"Do you use Slack for work?"*

If yes → follow `~/.claude/skills/ptw-onboard/mcp-setup-cards/slack.md`. If no → skip.

### Then install these two for everyone:

- **Claude in Chrome** → follow `mcp-setup-cards/claude-in-chrome.md`
- **Computer use** → follow `mcp-setup-cards/computer-use.md`

### Hard rule

If you're at the 15-minute mark of the whole session and not done with MCPs, **stop here and move on**. The remaining MCPs can be installed any time later. Don't burn the demo for MCP completionism.

### Verify

After each MCP, run a quick test (the setup cards have suggested test prompts). Tell the user what you tested and that it worked.

⏸ **PAUSE -- Part 4 done. Summarize which MCPs are connected, then move to Part 5.**

---

## Part 5 -- Seed memory

Say: *"This is where Claude starts to know who you are. I'm going to ask you a few questions, and I'll write the answers into Claude's memory so future conversations have this context automatically."*

The memory templates are in `~/.claude/skills/ptw-onboard/memory-templates/`. Create the memory directory for the current project (`~/.claude/projects/-Users-<username>-Claude/memory/` since they're in `~/Claude`), copy the Tier 1 templates into it, copy MEMORY.md, then fill them in based on what the user tells you.

### Tier 1 questions (ask all of these)

1. *"Tell me your role and what your company does, in two or three sentences."* → user_profile.md
2. *"What's your title? -- the one you'd want on documents and emails I draft for you."* → user_profile.md
3. *"On a scale of 'never opened a terminal' to 'I write code professionally,' where are you? Honest answer is best."* → user_profile.md
4. *"Which of these do you actually use day to day -- Gmail or Outlook, Slack or Teams, Drive or OneDrive or Dropbox?"* → reference_filesystem_layout.md
5. *"If a PTW operator set this up for you, your writing samples should already be in this conversation. Read one out loud so I can listen for the voice."* (If no samples were provided in advance, ask the user to paste a few now.) Listen to / read the samples, extract voice patterns, write them into user_writing_style.md.

### Tier 2 questions (only ask if the user already sent the answers in advance, or if they volunteer)

Don't ask these cold. If a PTW operator already passed along book lists / principles / personal context, seed those files. Otherwise skip.

- user_reading_library.md
- user_values_and_lessons.md
- user_family_context.md

### Verify

Open a fresh conversation (or just ask in the current one): *"Who am I and what do I do?"* Confirm the answer is correct.

⏸ **PAUSE -- Part 5 done. Move to Part 6 when ready.**

---

## Part 6 -- Conversion levers (operator-only)

### If the user is doing this on their own (not with a PTW operator):

Skip this Part. Say: *"Part 6 is only for sessions run by a PTW operator -- it wires up two optional hooks back to them. You don't have one, so we're skipping it. Moving on."*

### If a PTW operator is here:

There are two hooks to install: a `/ask-pete` slash command (sends a question to the operator's Slack on demand) and two scheduled check-ins (14-day + 30-day usage summaries to the operator).

#### Step 1 -- Slack Connect

If the user has Slack: ask the operator to send a Slack Connect DM invite to the user's work email from their own Slack now. The user accepts, sees the new DM appear. Grab the channel ID (in Slack: click DM header → About → scroll to channel ID at the bottom -- looks like `D0XXXXXXXXX`).

#### Step 2 -- Write the operator config

Copy `~/.claude/skills/ptw-onboard/settings/ptw-onboard-config.json.template` to `~/.claude/projects/-Users-<username>-Claude/ptw-onboard-config.json`. Ask the operator for these values one at a time and fill them in:

- `operator_display_name` (e.g. "Pete Wild")
- `operator_slack_channel_id` (from Step 1; leave placeholder if no Slack)
- `operator_email` (e.g. "pwild@ptwconsultingllc.com")
- `client_display_name` (how the operator wants to recognize this client in their own Slack)
- `setup_date` (today)

#### Step 3 -- Install the slash command

Copy `~/.claude/skills/ptw-onboard/commands/ask-pete.md` to `~/.claude/commands/ask-pete.md`.

#### Step 4 -- Schedule the check-in crons

Use the `schedule` skill to create two one-time crons. The prompts are in `~/.claude/skills/ptw-onboard/crons/check-in-prompts.md` -- use the "14-day check-in" prompt for one firing 14 days from today, and the "30-day check-in" prompt for one firing 30 days from today.

#### Step 5 -- Test

Have the user type `/ask-pete say setup worked, you can ignore this`. Confirm the operator's Slack gets the DM within a few seconds.

⏸ **PAUSE -- Part 6 done (or skipped). Move to Part 7.**

---

## Part 7 -- Backup memory to cloud

Say: *"Last config step before the fun part. We'll symlink your Claude memory into a cloud folder you already use, so if your laptop dies you don't lose Claude's memory of your work."*

Detect what cloud storage the user has running. Common paths:
- iCloud Drive: `~/Library/Mobile Documents/com~apple~CloudDocs/`
- OneDrive: `~/OneDrive/` or `~/Library/CloudStorage/OneDrive-Personal/`
- Google Drive: `~/Library/CloudStorage/GoogleDrive-<email>/My Drive/` or `~/Google Drive/`
- Dropbox: `~/Dropbox/`

Ask the user which one to use if there's more than one. Then:

1. Create `<cloud-path>/Claude-Backup/` if it doesn't exist
2. Move `~/.claude/projects/-Users-<username>-Claude/memory/` into `<cloud-path>/Claude-Backup/memory/`
3. Symlink `<cloud-path>/Claude-Backup/memory/` back to `~/.claude/projects/-Users-<username>-Claude/memory/`
4. Verify the symlink works (the user can read the same file via either path)
5. Confirm cloud sync is active (the user should be able to see the files in their cloud storage app or on their phone)

⏸ **PAUSE -- Part 7 done. Time for the demo.**

---

## Part 8 -- Live demo

This is the moment everything clicks. **Do not skip this.**

Pick one real task from what the user has told you they hate doing each week (it should be in their memory from Part 5 -- check `user_profile.md`). If you don't have one, ask now: *"What's a task you do every week that you'd love to never do again?"*

Then do it with them, live. Some options that demo well depending on what they connected:

- *"Draft a follow-up email to [real contact in their inbox] about [real ongoing thing]"* → uses Gmail/Outlook MCP, then humanizer skill to scrub AI tone
- *"Look at my calendar next week and tell me which days are over-stuffed"* → Calendar MCP
- *"Summarize the last meeting notes in [Drive/OneDrive folder] and pull out action items"* → Drive/OneDrive + summarization
- *"Open [vendor portal / bank site] in Chrome and pull the latest invoice amounts into a table"* → Chrome MCP

After Claude produces the output, run it through humanizer if it's writing: *"Use the humanizer skill to scrub AI tics from that draft."*

When you're done, say something like:

> *"That's the setup. From now on, when you have something you'd normally do manually -- email triage, drafting, looking up information across your tools -- try asking Claude first. Talk to it like you'd talk to a sharp assistant who already knows you. The more you use it, the more useful it gets."*

If a PTW operator is here, mention the `/ask-pete` command and the 2-week check-in:

> *"You also have `/ask-pete` -- type that anytime you're stuck and want a real human to look at it. And you'll get a couple of automated check-in DMs over the next month, just so we can see what's working."*

⏸ **PAUSE -- Setup complete. Congratulate the user and offer to do one more thing if they want.**

---

## Reference: time budget (for operators)

If a PTW operator is running this with a client, the target is 30 minutes. Pacing:

| Part | Min | Skippable? |
|---|---|---|
| 0 -- Greeting | 1 | No |
| 1 -- Terminal | 2 | No |
| 2 -- Work dir + skills | 3 | No |
| 3 -- Allowlist | 2 | No |
| 4 -- MCPs | 8 | Yes -- keep what they use |
| 5 -- Memory | 7 | No -- this is the magic |
| 6 -- Conversion levers | 3 | Only if operator session |
| 7 -- Backup | 2 | Yes -- can do remotely later |
| 8 -- Demo | 3 | **Never skip** |

If running long: drop Part 7 first, then trim Part 4 (one MCP minimum). Never drop the demo.

## After the session

If a PTW operator ran this:
- Operator sends a one-line Slack DM within 24h with a link to the [Anthropic prompting docs](https://docs.anthropic.com/en/docs/prompting).
- Operator reads the 14-day and 30-day cron summaries when they fire.
- Operator decides from those summaries whether there's a follow-up worth doing.
