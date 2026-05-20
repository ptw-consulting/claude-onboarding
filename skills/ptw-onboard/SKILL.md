---
name: ptw-onboard
description: PTW Consulting's 30-minute in-person Claude onboarding for SMB owners and solo professionals. Walks the operator through terminal install, settings allowlist, MCP auth, memory seeding, backup symlink, conversion levers, and a live demo. Use when running an onboarding session with a client.
---

# PTW Claude Onboarding — Operator Script

This is the 30-minute in-person walkthrough. Run this in the **client's own** Claude desktop app (or terminal) on **their** machine. Time-box each step. If you blow the budget, skip — protect the final demo at all costs; it's the buy-in moment.

## Before you arrive

- The client should have done the prep checklist (see `prep-email/pre-session-checklist.md`)
- You should have read their writing samples once already
- Pull up your operator config values from 1Password: your Slack Connect channel ID (you'll grab a new one per client in Part 5), your email, your display name
- Pick the **first project** they sent in prep — you'll use the project name for the memory directory

## Variables to substitute throughout

- `{{client_first_name}}` → e.g. `jane`
- `{{project_dir}}` → directory hash like `-Users-jane-Documents-acme` (Claude auto-generates this from the cwd you launch it from)
- `{{project_name}}` → the first project they sent in prep, e.g. `acme`

---

## Part 1 — Terminal (0:00–0:02)

This is the "feel tech-y" moment. We're not living in the terminal; the desktop app is the daily-use surface. But install a nice terminal so they have it.

### Mac

```sh
# Install Ghostty (free, fast, beautiful native terminal)
brew install --cask ghostty
open -a Ghostty
```

If they don't have Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` first.

### Windows

Windows Terminal ships with Windows 11. Just pin it:
1. Press `Win + S`, type "Terminal", right-click → Pin to taskbar.
2. Open it. Done.

**Say to the client:** *"This is what developers see when they work. We're not going to live here — your day-to-day is the Claude desktop app. But you'll use this to install a couple of things in the next two minutes."*

---

## Part 2 — Work directory, alias, clone, install skills (0:02–0:05)

### 2a — Set up the work directory and alias

**Why this matters:** Claude project memory is keyed off the working directory you launch from. Launching from different places creates split-brain memory. Pick one directory, launch from there always.

We use `~/Claude` as the work directory. Local, not cloud-synced (we handle backup via symlink in Part 7 — putting the whole work dir in iCloud/OneDrive can fight with file-watcher behavior mid-conversation).

### Mac (zsh)

```sh
# Create the work directory
mkdir -p ~/Claude

# Add a 'work' alias to ~/.zshrc
echo "alias work='cd ~/Claude && claude'" >> ~/.zshrc
source ~/.zshrc
```

### Windows (PowerShell)

```powershell
# Create the work directory
New-Item -ItemType Directory -Force -Path "$HOME\Claude"

# Add a 'work' function to the PowerShell profile
if (-not (Test-Path $PROFILE)) { New-Item -ItemType File -Force -Path $PROFILE }
Add-Content $PROFILE "`nfunction work { Set-Location `"$HOME\Claude`"; claude }`n"
. $PROFILE
```

Now `work` from anywhere → drops them in `~/Claude` and launches Claude (terminal mode).

**Say to the client:** *"From now on, when you want to launch Claude from the terminal, just type `work`. You probably won't need to — the desktop app is your daily home — but the alias is here if you want it."*

### 2b — Clone the repo and install skills

```sh
# Clone the onboarding repo into the work directory
cd ~/Claude
git clone https://github.com/ptw-consulting/claude-onboarding.git

# Make sure the user-level skills dir exists
mkdir -p ~/.claude/skills

# Install the two skills
cp -R ~/Claude/claude-onboarding/skills/ptw-onboard ~/.claude/skills/
cp -R ~/Claude/claude-onboarding/skills/humanizer ~/.claude/skills/

# Verify
ls ~/.claude/skills/
```

**Expected output:** at least `humanizer` and `ptw-onboard` in the list.

### 2c — Point the desktop app at the same work directory

Open the Claude desktop app. Create a new project (or open one) with `~/Claude` as its working directory. This is the "home" project — everything from this point uses the same memory, the same project_session_log, the same skills.

**Confirm:** the desktop app's project picker shows `Claude` (or `~/Claude`) as the active project. If you're unsure, ask Claude in the desktop app: *"What is your current working directory?"* — it should answer with the path to `~/Claude`.

Switch back to the Claude desktop app for the rest of the session. The terminal won't come up again.

---

## Part 3 — Settings allowlist (0:05–0:07)

In the Claude desktop app, open a new conversation in any project, and ask Claude:

> *"Merge the contents of ~/.claude/skills/ptw-onboard/settings/settings.local.json.template into ~/.claude/settings.local.json. If settings.local.json doesn't exist, create it from the template. Don't duplicate any allow entries that already exist."*

Claude will read the template, read the existing settings (if any), and write the merged file.

**Say to the client:** *"This is a list of safe commands Claude can run without asking you to approve each one. It's conservative — anything risky still asks first."*

---

## Part 4 — MCPs (0:07–0:15)

In the Claude desktop app: **Settings → Connectors** (or **Tools & Connectors**).

Install **in this order**, skipping any that don't apply:

1. **Google Workspace** *(Gmail + Calendar + Drive)* — see `mcp-setup-cards/google-workspace.md`
   **OR**
   **Microsoft 365** *(Outlook + Calendar + OneDrive)* — see `mcp-setup-cards/microsoft-365.md`
2. **Slack** — only if they use it. See `mcp-setup-cards/slack.md`.
3. **Claude in Chrome** — for everyone. See `mcp-setup-cards/claude-in-chrome.md`.
4. **Computer use** — for everyone. See `mcp-setup-cards/computer-use.md`.

**Hard time-box:** if you're at 0:15 and not done, skip the rest. The remaining MCPs can be installed any time later. **Do not sacrifice the demo for MCP completionism.**

---

## Part 5 — Seed memory (0:15–0:22)

In a Claude desktop app conversation, paste this prompt (substitute the client variables):

> *"I'm setting up Claude memory for {{client_first_name}} ({{project_name}} project). Create the memory directory at `~/.claude/projects/<auto>/memory/` if it doesn't exist, then copy the templates from `~/.claude/skills/ptw-onboard/memory-templates/` into it and fill them in based on what I tell you next. Start with the Tier 1 templates (user_profile, user_writing_style, reference_filesystem_layout, project_session_log, feedback_session_log_updates) and MEMORY.md."*

Then walk through these prompts out loud, letting the client answer in their own words. Claude fills the templates as you go.

**Tier 1 questions (always ask):**

1. *"Tell me about your role and your company in 2-3 sentences. What do you do, and what does the company do?"* → user_profile.md
2. *"What's your title — how should we sign things on your behalf?"* → user_profile.md
3. *"On a scale of 'never touched a terminal' to 'I write code,' where are you? Honest answer is best — Claude calibrates explanations off this."* → user_profile.md
4. *"Which of these do you use day to day? Gmail or Outlook? Slack or Teams? Drive, OneDrive, Dropbox, or local files? Notes app?"* → reference_filesystem_layout.md
5. *"Read me one of the writing samples you sent. I want to listen for the voice as you read it."* (Then summarize the voice patterns aloud and let Claude write them into user_writing_style.md based on the samples.) → user_writing_style.md

**Tier 2 — only if they sent these in the prep email:**

- Books → user_reading_library.md
- Principles → user_values_and_lessons.md
- Family/personal → user_family_context.md

If they didn't send these, skip. Do not awkwardly ask.

**Verify memory works.** Open a fresh conversation and ask: *"Who am I and what am I working on?"* Claude should answer correctly from memory.

---

## Part 6 — Conversion levers + Slack Connect (0:22–0:25)

This is what makes the install useful to you as a sales channel. Skip honestly if the client isn't going to be a PTW prospect — but for the friends/family pilots, do it.

### Step 1: Slack Connect DM

If the client has Slack: from **your** Slack, send a Slack Connect DM invite to their work email. Have them accept on their phone or browser now.

Grab the channel ID from their Slack (DM → about → channel ID at the bottom). Copy it.

### Step 2: Write the operator config

In Claude:

> *"Copy `~/.claude/skills/ptw-onboard/settings/ptw-onboard-config.json.template` to `~/.claude/projects/<this_project_dir>/ptw-onboard-config.json`. Then update these fields with values I'll dictate now."*

Dictate:
- `operator_display_name`: your name (e.g. `Pete Wild`)
- `operator_slack_channel_id`: the Slack Connect channel ID from Step 1 (or leave the placeholder if no Slack)
- `operator_email`: `pwild@ptwconsultingllc.com`
- `client_display_name`: how you want to recognize this client in your own Slack (e.g. `Jane at Acme`)
- `setup_date`: today's date

### Step 3: Install `/ask-pete` slash command

> *"Copy `~/.claude/skills/ptw-onboard/commands/ask-pete.md` to `~/.claude/commands/ask-pete.md`. The user can now type /ask-pete in any conversation."*

### Step 4: Install the 14-day and 30-day check-in crons

Use the `schedule` skill:

> *"Use the schedule skill to create two cron jobs. First: a one-time cron firing 14 days from now, running the prompt in `~/.claude/skills/ptw-onboard/crons/check-in-prompts.md` under the '14-day check-in' section. Second: a one-time cron firing 30 days from now, running the '30-day check-in' prompt from the same file."*

Verify both crons are scheduled with the `schedule` skill's list command.

**Test the wiring before you leave:** run `/ask-pete say setup worked, you can ignore this`. Confirm a Slack DM lands in your phone within a few seconds.

---

## Part 7 — Backup (0:25–0:27)

Symlink the memory directory into the client's cloud storage so a dead laptop doesn't lose their Claude.

Because we launch Claude from `~/Claude` (Part 2), the memory directory is at `~/.claude/projects/-Users-<username>-Claude/memory/`. We're symlinking THAT path — not any other project's memory.

Ask Claude:

> *"Set up an auto-backup for memory. The active project memory is at `~/.claude/projects/-Users-<username>-Claude/memory/` (the project tied to ~/Claude). Figure out the right cloud storage path on this machine (iCloud Drive, OneDrive, Google Drive, or Dropbox — check what the user has). Create a `Claude-Backup/memory/` directory inside it. Then move the memory directory into that backup folder and symlink it back. Confirm both the symlink and the cloud sync are working."*

Verify on the client's phone or another device that they can see the memory files in their cloud storage app.

---

## Part 8 — Live demo (0:27–0:30)

**Pull up the "task you hate" they sent in the prep email and do it now.**

This is the buy-in moment. They watch Claude do a thing they actually hate. They leave the session convinced.

Some hated-task examples that demo well:

- *"Draft a follow-up email to [a real contact in their Gmail / Outlook] about [a real thing they're working on]"* — uses Gmail/Outlook MCP, then humanizer skill
- *"Look at my calendar next week and tell me which days are over-stuffed"* — uses Calendar MCP, surfaces real conflicts
- *"Summarize the last meeting notes in [Drive folder] and pull the action items into a Slack DM"* — chains multiple MCPs
- *"Open this vendor portal in Chrome and pull the latest invoice amounts into a table"* — Chrome MCP

Run humanizer as a second pass on any drafted text: *"Use the humanizer skill to scrub AI tics from that draft."*

**Say at the end:** *"This is day one. Talk to Claude every day for a week, and use /ask-pete if you hit anything that doesn't work. I'll get a short summary in two weeks of how you've been using it — we can decide from there if there's anything worth building on top."*

---

## After the session — operator follow-ups

- Within 24 hours: send a one-line "great seeing you" Slack DM with a link to the [Anthropic prompting docs](https://docs.anthropic.com/en/docs/prompting). Sets up the relationship as ongoing.
- Day 14: read the cron's auto-summary. Decide if there's a follow-up worth doing.
- Day 30: read the second summary. If there's a clear engagement opportunity, draft a proposal using the PTW `create-proposal` skill (operator-side). If not, leave them alone for another month.
- Track conversion: which clients turn into paid engagements, which stay free users. Pull the plug on the whole program if <1 in 5 converts after 5 setups.

## Time budget cheat sheet

| Part | Min | Skippable? |
|---|---|---|
| 1 — Terminal | 2 | No — quick win |
| 2 — Work dir + alias + skills | 3 | No — alias prevents split-brain memory later |
| 3 — Settings allowlist | 2 | No |
| 4 — MCPs | 8 | Yes — keep what they actually use |
| 5 — Memory | 7 | No — this is the magic |
| 6 — Conversion levers | 3 | Only if not a PTW prospect |
| 7 — Backup | 2 | Yes — can do remotely later |
| 8 — Demo | 3 | **NEVER SKIP** |

If you're running long: kill Part 7 first, then Part 4 (install one MCP minimum), then Part 6 if not a prospect. Never kill the demo.
