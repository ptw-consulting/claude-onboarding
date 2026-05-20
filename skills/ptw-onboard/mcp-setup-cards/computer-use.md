# Computer use MCP

Lets Claude see the client's screen and click / type in native desktop apps. The fallback for anything that isn't in a browser — Notes, Numbers, Photos, Maps, third-party native apps.

**Important:** every action goes through a permission prompt the first time per app. The client should know to expect this.

## Time budget

2-3 minutes.

## Prerequisites

- macOS 13+ or Windows 11
- Client willing to grant screen recording + accessibility permission to Claude

## Steps

1. In the Claude desktop app: **Settings → Connectors → Computer use → Connect**.
2. macOS will prompt for **Screen Recording** and **Accessibility** permissions. The client must approve both in **System Settings → Privacy & Security**.
3. Windows 11 has an equivalent permissions prompt; click through.
4. Quit and reopen Claude once after granting permissions (macOS requires this).

## Verification

Ask Claude:

> *"Take a screenshot of my desktop and tell me what apps are open."*

A successful response = MCP is working.

## What to mention to the client

> "Claude can now see your screen and control native apps when you ask. It only does what you tell it to — but every new app it touches will pop up a permission dialog the first time. Just approve those as they come up."

## Safety notes the operator should share

- Claude won't move money, send messages, or click "delete" without confirming first. Computer-use is designed conservatively.
- The client can disconnect screen sharing anytime in **System Settings → Privacy & Security**.
- If the client uses 1Password or another password manager, those windows are intentionally hidden from screen capture — Claude can't see passwords being typed.
