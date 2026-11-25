# Cognition for your AI assistant

Use Cognition from the Claude apps, Cowork, Claude Code, ChatGPT or any assistant that supports MCP.
Cognition is a study app: workspaces for your courses, notes, quizzes, flashcards and grades,
deadlines, tasks, bookmarks, documents, transcripts and a journal. You sign in with your own
Cognition account, choose what your assistant may do, and can see and undo everything it did from
[Settings › Connected AI](https://app.evolving-cognition.com/connected-ai). Your assistant cannot
transcribe a video from the chat; see [Videos and transcripts](#videos-and-transcripts).

The connector is part of Cognition Pro. See the plans at https://app.evolving-cognition.com/subscribe.

## Install

**Claude Code.** Run:

```
/plugin marketplace add GiGaSoftwareDevelopment/cognition-ai
/plugin install evolving-cognition@cognition-ai
```

Restart Claude Code if it asks, then run `/mcp`, choose **cognition** and sign in. Your browser
opens Cognition. Sign in, choose what to allow, and return to Claude Code.

**The Claude apps and Cowork.** Open Customize › Plugins and add Evolving Cognition. Then choose
**Connect** and sign in to Cognition. You can also add the server URL below as a custom connector:
Customize › Connectors › **+** › **Add custom connector**.

**ChatGPT, Gemini and any other assistant.** Add the server URL as a custom connector in your
assistant's connector settings, then connect and sign in to Cognition.

```
https://mcp.evolving-cognition.com/mcp
```

Step-by-step help for each assistant: https://evolving-cognition.com/help/connect-ai

Where the assistant can show cards, Cognition answers some requests with one: a quiz you take there,
your due flashcards with their pictures and sounds, what's due this week, a workspace's bookmarks
with search and tags, an SVG document, and a confirm card for each change. Elsewhere it answers the
same requests in text.

## Works well with government data

Cognition holds no public statistics. Add the USAFacts Government Data Connector to your assistant
(from its connector settings), then ask for a quiz or flashcards on official figures, such as "quiz
me on U.S. demographics". The assistant fetches the figures, saves the quiz or cards to Cognition,
and names the source and its date in each question or card.

## What your assistant can and cannot do

- **Your assistant does the thinking.** It writes your quizzes, flashcards and summaries itself, in
  the chat, and saves them to Cognition. Cognition's own AI, which generates cards and quizzes in
  the app, is not used from the chat.
- It sees only your own Cognition data.
- It asks you before it edits or deletes anything. Saving a transcript, or a part of one, is a new
  note, so it needs no question; you can undo each part from Settings › Connected AI.
- In a workspace where you are the Owner or a Teacher, it can add an SVG drawing to the workspace's
  documents and change one; changing one asks you first. Any member's assistant can look at one.
- A change to more than one record waits for your review in Cognition.
- Every call is listed in Settings › Connected AI, where you can undo a single change for a limited
  time, or disconnect your assistant.

## Videos and transcripts

- Your assistant cannot transcribe a video, and Cognition does not transcribe one from the chat.
- To keep a video, your assistant saves its link as a bookmark, and you confirm. Then you transcribe
  it in the Cognition app or with
  [Cognition for Chrome](https://chromewebstore.google.com/detail/cognition-for-chrome/jjmgmbicjepgnpoimjkifbglidjmekne).
- If you already have a transcript, pasted or from YouTube itself, your assistant saves it as a note,
  linked to its bookmark if you have one (the **save-transcript** skill).
- Nothing in the chat uploads a file to Cognition, such as a video, a recording or a PDF. Add those
  in the Cognition app.

## Skills

- **quiz-me**: your assistant quizzes you on one of your Cognition quizzes, one question at a time,
  and saves your score to your Grades and Stats if you say so. With no quiz yet, it writes one from
  your notes, or from official figures, and offers to save it to Cognition.
- **save-transcript**: your assistant saves a transcript you give it, such as one you paste, as a
  note in one of your workspaces, with its timestamps, sending a long one in parts. Run it as
  `/evolving-cognition:save-transcript`.
- **study**: your assistant studies your flashcards with you, one card at a time. You answer in your
  own words, it tells you whether you were correct, close or wrong, and saves the session to your
  memory if you say so. It writes cards from a note or from official figures itself, and saves them
  to Cognition. Run it as `/evolving-cognition:study`.
