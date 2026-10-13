---
name: save-transcript
description: Save a transcript of a video or audio the user studies as a note in Cognition. Use when the user asks to save, store or add the transcript of a lecture, video or recording to a workspace, such as "save the transcript of this lecture to my Biology course".
---

# Save a transcript

Cognition keeps the transcript as an ordinary note the user can read, search and make quizzes from.
You send the words; Cognition does not ask, and you do not say, how the transcript was made. Follow the
cognition-connector skill for permissions and refusals.

## 1. Pick the workspace

- Call `list_workspaces` and use the workspace the user named. If several could match, list them by
  name and ask. Do not guess.
- The tools take `workspaceType` and `workspaceId` together, always both. A transcript needs a
  workspace; there is no private one.

## 2. Prepare the transcript

- Prefer `segments`: `[{ startMs, endMs?, text }]`, in the order spoken. `startMs` and `endMs` are
  milliseconds from the **start of the recording**, never from the start of a part. Give `startMs`
  on every segment or on none. The note then keeps its timestamps.
- With no timings, send plain `text` with a blank line between paragraphs. Send `text` or `segments`,
  never both.
- Include `sourceUrl` (the recording's http or https link) and `language` (a code such as `en` or
  `pt-BR`) when you know them. For a YouTube link the timestamps open the video at that moment.
- Title it with the video or lecture's title. The note is named "Transcript — <title>".

## 3. Split a long transcript

One call takes at most 150,000 bytes of text and 2,000 segments. Split anything over either limit
into parts, cut between segments, in order.

- Send the first part with `add_transcript`. It answers with the note's id.
- Send each next part, in order, with `append_transcript`: the same workspace pair, the `noteId` the
  last answer gave, and the part. Keep `startMs` counted from the start of the recording; a part that
  starts before the last segment already saved is refused.
- A very long transcript may spill into a note titled "… (part 2)". The answer's id is the note the
  part landed in, so always send the next part to the id of the last answer.
- Never send a part twice, and never skip one. If a call fails, tell the user which part it was.

## 4. Link a bookmark (optional)

If the user says the transcript belongs to a bookmark in the workspace, call `list_bookmarks`, pick
the matching one and pass its `bookmarkId` to `add_transcript`. The bookmark then links to the note.
Do not link one the user did not mention.

## 5. Tell the user

- Say which workspace and note it was saved to, and how many parts it took.
- `add_transcript` and `append_transcript` run without a confirmation, because each can be undone.
  The user can undo each part, newest first, from Settings › Connected AI in Cognition. Never offer
  to undo from the chat.

## If a tool refuses

Relay the sentence in plain words. If it is the Cognition Pro upgrade message, give it as it is. Do not
work around a refusal, and do not retry the same call to push it through.
