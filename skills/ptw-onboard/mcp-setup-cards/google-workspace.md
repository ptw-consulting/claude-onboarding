# Google Workspace MCP

Connects Claude to Gmail, Google Calendar, and Google Drive. Use for clients on Google Workspace (most professional services SMBs).

**Skip if:** client is on Microsoft 365. Go to `microsoft-365.md` instead.

## Time budget

3-5 minutes total (3 MCPs, OAuth each).

## Prerequisites the client should have done in prep

- Logged in to Gmail, Google Calendar, and Drive in their default browser

## Steps

1. In the Claude desktop app, open **Settings → Connectors** (or **Tools and Connectors**, depending on app version).
2. Find and enable each in turn:
   - **Gmail**
   - **Google Calendar**
   - **Google Drive**
3. For each, click **Connect**. A browser tab opens to Google's OAuth screen. The client clicks through and grants permission.
4. Back in Claude, each connector should show as "Connected" with a green dot.

## Verification

Ask Claude one of these in a new conversation:
- *"What's on my calendar tomorrow?"*
- *"Show me the last 3 emails from [a real contact]."*
- *"Find the most recent Google Doc I edited."*

Successful response = MCP is wired correctly.

## Common issues

- **OAuth tab opens but doesn't return to Claude:** the client probably has multiple Google accounts signed in. Sign out of all but the work account, retry.
- **"Connected" but Claude says it can't see emails:** check the permission scopes during OAuth — Google sometimes presents a "select what to share" screen that defaults to too little.
- **Connector doesn't appear in the list:** the desktop app may need updating. Check **Settings → About** for the latest version.

## What to mention to the client

> "Now Claude can read your inbox, your calendar, and your Drive when you ask. It won't do anything unprompted — every action goes through you."
