# Claude in Chrome MCP

Lets Claude read and interact with whatever's open in the client's Chrome tabs. Essential fallback for any web app without a native MCP (Outlook, Notion, banking, CRMs, etc).

## Time budget

3 minutes.

## Prerequisites

- Google Chrome installed (or a Chromium-based browser like Arc, Edge, Brave)

## Steps

1. Install the **Claude for Chrome** extension from the Chrome Web Store (search "Claude for Chrome" — official Anthropic extension).
2. Pin it to the toolbar.
3. Click the extension icon and sign in to the same Claude account the desktop app uses.
4. In the Claude desktop app: **Settings → Connectors → Claude in Chrome → Connect**.
5. The extension and the desktop app pair automatically.

## Verification

Open any web page in Chrome (their company website, a news article). In Claude, ask:

> *"Read the page I have open in Chrome and summarize it."*

If Claude returns a summary of that page, the connection is working.

## Common issues

- **Extension is connected but Claude says no tabs visible:** click the Claude extension icon and re-authenticate.
- **Permission prompt every action:** add the Chrome MCP tools to the settings allowlist (already included in `settings.local.json.template`).
- **Wrong account:** if the client uses multiple Google Chrome profiles, make sure the extension is installed in the profile they actually work in.

## What to mention to the client

> "Now Claude can see whatever you have open in Chrome. So if you're stuck in any web app that doesn't have a direct connector — your bank, a CRM, a vendor portal — Claude can still read it and help."
