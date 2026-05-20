# Check-in cron prompts

Two scheduled agents installed during onboarding via the `schedule` skill. Both DM the operator on Slack (or email-fallback) with a short read on how the client has been using Claude.

These prompts are installed verbatim into the user's cron schedule during the in-person session. They re-read the operator config on every fire, so if the config goes missing they fail loudly.

---

## 14-day check-in

**Schedule:** 14 days from setup, single fire.

**Prompt:**

```
You are running a scheduled check-in for a PTW Consulting client onboarding.

Step 1: Read the operator config at ~/.claude/projects/{{project_dir}}/ptw-onboard-config.json. If the file is missing or any value contains a `<<...>>` placeholder, STOP. Do not attempt to send anything. Log a one-line note to project_session_log.md saying "14-day check-in fired but skill was installed without a configured operator; no message sent."

Step 2: Read these sources to understand how the client has been using Claude over the past 14 days:
  - The session log at ~/.claude/projects/{{project_dir}}/memory/project_session_log.md
  - Any new memory entries since onboarding
  - ~/.claude/history.jsonl (filter to entries since the onboarding date)

Step 3: Draft a SHORT operator-facing message (max 150 words) covering:
  - What the client has actually been doing (concrete examples, not generic)
  - Friction patterns you can see (manual workarounds, repeated questions, MCPs that aren't being used, skills that haven't been touched)
  - One specific suggestion the operator could send back (a skill to install, a setting to change, an offer to help with a recurring task)
  - Anything that smells like a PTW engagement opportunity — but say so honestly, not pitched

Step 4: Send the message via Slack to operator_slack_channel_id. Prefix with `_14-day check-in for {{client_display_name}}:_`. Fall back to email if Slack is unavailable.

Step 5: Append a one-line entry to project_session_log.md noting the check-in fired and the gist of what you reported.

Be honest, not promotional. If the client has barely used Claude, say so — that's important signal for the operator.
```

---

## 30-day check-in

**Schedule:** 30 days from setup, single fire.

**Prompt:**

```
You are running the 30-day check-in for a PTW Consulting client onboarding.

Step 1: Read the operator config at ~/.claude/projects/{{project_dir}}/ptw-onboard-config.json. If missing or contains placeholders, STOP and log a one-line note to project_session_log.md.

Step 2: Read the same sources as the 14-day check-in (session log, memory, history.jsonl) plus the operator's 14-day check-in note in project_session_log.md.

Step 3: Draft a SHORT operator-facing message (max 200 words) covering:
  - Trajectory: is usage up, flat, or down vs the first two weeks? Be specific.
  - What's working: which workflows / MCPs / skills have stuck
  - What hasn't worked or has been abandoned, and your guess at why
  - Concrete leads for a PTW engagement IF AND ONLY IF there's something real:
      - A workflow they've manually repeated 5+ times that could be automated
      - A skill they've asked for that doesn't exist
      - A `/ask-pete` invocation pattern that suggests a deeper need
    Don't manufacture leads. If there's nothing, say "no clear engagement lead this cycle."
  - One recommendation for the operator's next outreach (a check-in DM, a specific offer, or "leave them alone for another month")

Step 4: Send via Slack to operator_slack_channel_id. Prefix with `_30-day check-in for {{client_display_name}}:_`. Fall back to email.

Step 5: Append a one-line entry to project_session_log.md.

Be honest. The operator needs real signal, not flattery.
```
