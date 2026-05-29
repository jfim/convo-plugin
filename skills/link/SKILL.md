---
name: link
description: Use when the user asks for a link to the current conversation, or wants to reference/save/share this session. Generates a convo:// deep link from the session id.
---

# Linking to the current conversation

The current session id is in `$CLAUDE_SESSION_ID` and the project slug in
`$CLAUDE_CONVO_SLUG` (both set at session start by the convo plugin's hook).

Build a link to this conversation:

    convo://claude-code/$CLAUDE_CONVO_SLUG/$CLAUDE_SESSION_ID

If `$CONVO_REDIRECT_HOST` is set, emit the redirect form instead — it is the
clickable form for apps that won't open custom schemes directly (Obsidian, chat
clients, ...). It resolves to the same conversation via an https→convo:// 302:

    https://$CONVO_REDIRECT_HOST/claude-code/$CLAUDE_CONVO_SLUG/$CLAUDE_SESSION_ID

(See `deploy/redirect/` in the convo repo for how to host that redirect.)

To anchor to a specific message, append `#<uuid>` (to whichever form you
emit), where `<uuid>` is the
`uuid` field of the target record in the transcript at:

    ~/.claude/projects/$CLAUDE_CONVO_SLUG/$CLAUDE_SESSION_ID.jsonl

If the env vars are empty, the hook hasn't run — fall back to reading the
session id from the most recently modified `.jsonl` under
`~/.claude/projects/$CLAUDE_CONVO_SLUG/`.

Once you have the link, use it however the user asked (print it, drop it in a
file, etc.). The skill stays agnostic about the destination.
