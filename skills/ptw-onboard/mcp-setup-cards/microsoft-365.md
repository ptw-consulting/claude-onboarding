# Microsoft 365 MCP

**Status: needs live validation.** No Microsoft 365 MCP exists yet in PTW's own setup. The first M365 client install is the validation pass.

Connects Claude to Outlook (mail + calendar) and OneDrive.

## Time budget

5-8 minutes (more if validating live for the first time).

## Prerequisites the client should have done in prep

- Logged in to Outlook (web at outlook.office.com or desktop) with their work account
- OneDrive sync running on their machine

## Path A: Native Anthropic connector (check first)

In the Claude desktop app, open **Settings → Connectors**. Look for:
- **Outlook** or **Microsoft Outlook**
- **OneDrive**

If they exist, install the same way as the Google connectors — Connect → OAuth → confirm. Verify with:
- *"What's on my Outlook calendar tomorrow?"*
- *"Show me the last 3 emails in my Outlook inbox."*
- *"Find the most recent file I edited in OneDrive."*

If they exist and work, skip to "What to mention to the client" below.

## Path B: Microsoft Graph community MCP (fallback)

If no native connectors exist, install a community MCP that wraps Microsoft Graph. Two known options as of late 2025 / early 2026 (validate availability live):
- `@modelcontextprotocol/server-microsoft-graph` (if it exists at install time)
- A community `microsoft-graph-mcp` package — search npm

Install at user scope:

```sh
claude mcp add microsoft-graph -- npx -y <package-name>
```

This will require setting up a Microsoft Entra (Azure AD) app registration with delegated Graph permissions. **This is the ugly part — budget extra time for the first client.** Once a working app registration exists, document the client_id and tenant_id for reuse on future M365 clients.

## Path C: Chrome MCP fallback (always works)

If A and B both fail in the session, fall back to using `claude-in-chrome` against `outlook.office.com`. Slower (DOM navigation) but works for every Outlook feature. See `claude-in-chrome.md`.

This is the right call if the in-person session is running short — give the client a working Claude that talks to Outlook via the browser, and revisit the Graph integration in a follow-up.

## What to mention to the client

> "Claude can now read your Outlook mail, your calendar, and your OneDrive files when you ask. It won't act unprompted — every step goes through you."

If you fell back to Path C: tell them honestly that you used the Chrome path and it's slower, but you'll send a real fix in a follow-up.

## Notes for the operator

**Update this card after the first real M365 install.** Replace the speculative Path A/B sections with whatever actually worked. This is the validation pass.
