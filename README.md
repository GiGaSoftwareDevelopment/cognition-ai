# Cognition for Claude Code

Use Cognition from Claude Code. Cognition is a study app: workspaces for your classes, notes,
quizzes and grades, deadlines, tasks, bookmarks, documents, transcripts and a journal. You sign in with your own
Cognition account, choose what Claude may do, and can see and undo everything it did from
[Settings › Connected AI](https://app.evolving-cognition.com/connected-ai).

The connector is part of Cognition Pro. See the plans at https://app.evolving-cognition.com/subscribe.

## Install

In Claude Code:

```
/plugin marketplace add GiGaSoftwareDevelopment/cognition-ai
/plugin install cognition@cognition-ai
```

Restart Claude Code if it asks, then run `/mcp`, choose **cognition** and sign in. Your browser
opens Cognition. Sign in, choose what to allow, and return to Claude Code.

## Other assistants

The same connector works in claude.ai, Claude Desktop, ChatGPT, Gemini and any MCP client. Add the
server URL as a custom connector:

```
https://mcp.evolving-cognition.com/mcp
```

Step-by-step help for each assistant: https://evolving-cognition.com/help/connect-ai

## What Claude can and cannot do

- It sees only your own Cognition data.
- It asks you before it edits or deletes anything. Saving a transcript, or a part of one, is a new
  note, so it needs no question; you can undo each part from Settings › Connected AI.
- In a workspace where you are the Owner or a Teacher, it can add an SVG drawing to the workspace's
  documents and change one; changing one asks you first. Any member's Claude can look at one.
- A change to more than one record waits for your review in Cognition.
- Every call is listed in Settings › Connected AI, where you can undo a single change for a limited
  time, or disconnect Claude.

## Skills

- **quiz-me**: Claude quizzes you on one of your Cognition quizzes, one question at a time, and
  saves your score to your Grades and Stats if you say so. With no quiz yet, Claude writes one from
  your notes and offers to save it to Cognition.
- **save-transcript**: Claude saves the transcript of a video or recording you study as a note in
  one of your workspaces, with its timestamps, sending a long one in parts. Run it as
  `/cognition:save-transcript`.
