# Cognition for Claude Code

Use Cognition from Claude Code. Cognition is a study app: workspaces for your courses, notes,
quizzes, flashcards and grades, deadlines, tasks, bookmarks, documents, transcripts and a journal. You sign in with your own
Cognition account, choose what Claude may do, and can see and undo everything it did from
[Settings › Connected AI](https://app.evolving-cognition.com/connected-ai).

The connector is part of Cognition Pro. See the plans at https://app.evolving-cognition.com/subscribe.

## Install

In Claude Code:

```
/plugin marketplace add GiGaSoftwareDevelopment/cognition-ai
/plugin install evolving-cognition@cognition-ai
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

In claude.ai and ChatGPT, Cognition answers some requests with a card in the chat: a quiz you take
there, your due flashcards with their pictures and sounds, what's due this week, a workspace's
bookmarks with search and tags, an SVG document, and a confirm card for each change. Claude Code
answers the same requests in text.

## Works well with government data

Cognition holds no public statistics. Add the USAFacts Government Data Connector to your assistant
(from its connector settings), then ask for a quiz or flashcards on official figures, such as "quiz
me on U.S. demographics". The assistant fetches the figures, saves the quiz or cards to Cognition,
and names the source and its date in each question or card.

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
  your notes, or from official figures, and offers to save it to Cognition.
- **save-transcript**: Claude saves the transcript of a video or recording you study as a note in
  one of your workspaces, with its timestamps, sending a long one in parts. Run it as
  `/evolving-cognition:save-transcript`.
- **study**: Claude studies your flashcards with you, one card at a time. You answer in your own
  words, Claude tells you whether you were correct, close or wrong, and saves the session to your
  memory if you say so. It can also write cards from a note or from official figures. Run it as
  `/evolving-cognition:study`.
