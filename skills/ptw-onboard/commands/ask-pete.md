---
description: Send a tight, well-framed question to your PTW operator on Slack (or email fallback)
---

The user has invoked `/ask-pete` with this question or topic: $ARGUMENTS

## What to do

1. **Read the operator config** at `~/.claude/projects/{{project_dir}}/ptw-onboard-config.json`. If the file is missing, or if any of `operator_slack_channel_id`, `operator_email`, or `client_display_name` contain a placeholder like `<<...>>`, respond with:

   > This skill was installed without a PTW operator. The `/ask-pete` command needs a configured operator to route to. If you'd like the full experience, find PTW at https://ptwconsultingllc.com.

   Then stop. Do not attempt to send anything.

2. **Draft the question.** Make it tight: one or two paragraphs maximum. Include:
   - The actual question or topic (the user's input above)
   - The minimum context the operator needs to answer (project name, recent decisions, what they've tried, what's at stake)
   - The specific kind of response that would help (a recommendation, a sanity check, an introduction, a template, etc.)

   Pull project context from memory if relevant. Don't dump entire memory files — summarize.

3. **Show the draft to the user** and ask: *"Send to {{operator_display_name}}? (y / edit / cancel)"*

4. **Route the draft:**
   - **Slack path (preferred):** If `operator_slack_channel_id` is set and the Slack MCP is connected, use `mcp__claude_ai_Slack__slack_send_message` with `channel: {operator_slack_channel_id}` and the drafted body. Prefix with: `_From {{client_display_name}} via /ask-pete:_`
   - **Email fallback:** If Slack is unavailable, draft a Gmail/Outlook message to `operator_email` with subject `[ask-pete] {{first line of question}}` and the drafted body. Show the draft link, don't auto-send unless the user confirmed in step 3.

5. **Confirm to the user** that the message was sent (or drafted). Tell them how long the operator usually takes to respond. If response time isn't in the config, say "usually within a business day."

## Config schema reference

`~/.claude/projects/{{project_dir}}/ptw-onboard-config.json`:

```json
{
  "operator_display_name": "Pete Wild",
  "operator_slack_channel_id": "C0XXXXXXXXX",
  "operator_email": "pwild@ptwconsultingllc.com",
  "client_display_name": "Jane at Acme Co",
  "expected_response_time": "usually within a business day"
}
```

The operator fills this in during the in-person session.
