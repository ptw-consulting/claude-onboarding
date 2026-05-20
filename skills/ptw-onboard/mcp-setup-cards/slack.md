# Slack MCP + Slack Connect setup

Connects Claude to the client's Slack workspace. Also sets up the Slack Connect DM that the `/ask-pete` command and the check-in crons will route through.

**Skip if:** client doesn't use Slack. The conversion levers fall back to email.

## Time budget

5-7 minutes (3 min MCP, 2-4 min Slack Connect dance).

## Prerequisites the client should have done in prep

- Logged in to their Slack workspace in their browser
- Permission to install integrations in their workspace (most SMB owners are admin; if not, this won't work — skip to email fallback)

## Part 1: Connect Slack as an MCP

In the Claude desktop app: **Settings → Connectors → Slack → Connect**. OAuth dance, choose the workspace, grant scopes.

Verify with: *"What's the most recent message in #general?"*

## Part 2: Set up the Slack Connect DM with the operator

This is the channel the cron and `/ask-pete` will message.

1. From the **operator's** Slack, start a Slack Connect DM with the client's work email. (Slack: **Tools → Slack Connect → Create connection** or right-click in the DM list.) Send the invite.
2. The client accepts the invite in their Slack — it shows up as a new DM with the operator. The header says "External" / has a Slack Connect badge.
3. In the client's Slack, grab the channel ID:
   - Click the DM header → **About** or **View channel details** → scroll to bottom → copy the Channel ID (looks like `D0XXXXXXXXX` for a DM or `C0XXXXXXXXX` for a channel).
4. Paste that channel ID into `~/.claude/projects/<proj>/ptw-onboard-config.json` as `operator_slack_channel_id`.

## Verification

After the config is saved, ask Claude in a new conversation:

> *"Test the /ask-pete config by sending a 'setup test, please ignore' message to the operator."*

Or just run `/ask-pete say setup worked, you can ignore this`. The operator should see a Slack DM within a few seconds.

## Common issues

- **Slack Connect requires both workspaces to allow external DMs.** Free Slack tiers can do this. If the client's workspace has blocked external DMs, an admin has to allow it (or skip to email fallback).
- **Channel ID is wrong format:** double-check — must start with `D` (direct message) or `C` (channel), not `T` (team/workspace) or `U` (user).
- **Message sends but operator doesn't see it:** check that the operator is the right side of the Slack Connect — invites from external workspaces can sit unaccepted.

## What to mention to the client

> "This is the channel I'll get notified on when you use `/ask-pete`, plus you'll see automatic 2-week and 1-month check-ins from your Claude — short summaries of how you've been using it. No surveillance — it's looking at your local usage patterns, not your messages."
