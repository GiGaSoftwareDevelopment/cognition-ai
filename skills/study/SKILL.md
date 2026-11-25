---
name: study
description: Study your Cognition flashcards with the AI as your study partner, in Cognition's flashcards card where the chat shows it or one card at a time. Use when the user wants to be tested on their flashcards, practice due cards or questions, make flashcards from a note or from official figures, or see how well they remember, such as "quiz me on my Biology flashcards" or "practice my due questions".
---

# Study

Cognition holds the cards and the answer key. Judging an answer costs Cognition nothing. Follow the
cognition-connector skill for confirmation, permissions and its In-chat cards section.

## 1. Find the set

- Call `list_workspaces` for the workspace the user means, and pass its `workspaceType` and
  `workspaceId` to every card tool, always both together. With no pair, the tools use the user's own
  Flashcards page.
- Call `list_card_sets`. If one set clearly matches what the user asked for, use it. If several could
  match, list them by name and cards due and ask the user to pick. Do not guess.
- If no set matches, say so, and offer to make cards from their notes (Make cards, below).
- Call `get_due_cards`, with the set when there is one. It returns each card's prompt and hint, never
  the answer. If nothing is due, say so and offer to practice due questions instead.
- `truncated: true` means more cards are due than this answer holds. `imageOcclusionDue` counts the
  Image Occlusion cards due, which are studied in the Cognition app: tell the student how many.

## 2. When Cognition's flashcards card shows

In claude.ai and ChatGPT, `get_due_cards` opens the flashcards card. The card shows each card with
its pictures and sounds, checks each answer and offers Save session.

- Do not read the cards out in the chat, never say you cannot show pictures or sounds, and do not ask
  to save.
- The card tells you what happens: the student is studying (how many studied and how they went), has
  finished (the session is not saved yet; the card offers to save it), saved the session, or chose
  not to save. Go by that.
- Never offer to save a session the card saved.

Claude Code shows no card. Without one, study in the chat, steps 3 to 6.

## 3. Ask one card at a time

- Show one prompt and its hint, then wait for the user's answer. Never show the next card early, and
  never reveal or hint at the answer.
- A picture or sound reads `[picture]` or `[sound]`: a marker, never a file path. Say the card has a
  picture or a sound, which the student can see or hear on the card in Cognition, and ask the
  prompt as written.
- If the user does not know, accept it as wrong and move on.

## 4. Judge each answer

- Call `check_card_answer` with the card and the user's answer. It returns the expected answer, the
  extra and the grading guidance.
- Judge the answer yourself against the expected answer and the guidance: **correct**, **close** or
  **wrong**. Say which, in one or two sentences: what was right and what was missing. Then show the
  expected answer.
- Keep each verdict with its `cardId`. Move to the next card.

## 5. Practice questions

- When the user asks for questions due, call `get_practice_questions`. Ask each one as quiz-me does:
  the question and its choices, one at a time, and "pick all that apply" when `multipleAnswers` is
  true. Map the reply to the listed choice ids.
- Check each with `check_quiz_answer`, tell the user whether they were right, name the correct
  choices and give the explanation when there is one.
- Keep each answer as its `quizId`, `questionId` and `choiceIds`. A skipped question is left out.

## 6. Finish and save

- Tell the user how it went in terms of memory: what they remembered and how many to study again. No
  scores, streaks or grades.
- Ask whether to save the session. Saying no is fine: nothing is recorded.
- To save, call `save_card_reviews` with all the card verdicts and practice answers. It does not save
  at once. It returns a confirmation that says when items come back and that saved reviews can't be
  undone, plus a `confirmationToken`. Show the user that and ask. Only after a clear yes, call
  `save_card_reviews` again with exactly the same arguments plus the `confirmationToken`. Claude Code
  shows its own confirmation form instead, which the user answers.

## Make cards

- You write every card yourself, in the chat, however long the note or lecture.
- From a note: find it with `list_notes` and read it with `get_note`; read a long note or lecture in
  parts and write its cards part by part. Write one idea per card, with a clear prompt and answer,
  and pass `source` with the note's id. In claude.ai and ChatGPT, where a card
  shows what was saved, say how many you wrote and ask whether to save them; the card lists them once
  saved. In Claude Code, show them and ask. After the user agrees, save them with `create_cards`. It
  costs nothing.
- **Cards on official figures:** Cognition holds no public statistics. If a government data
  connector is available (the USAFacts Government Data Connector, Data Commons), fetch the figures
  from it and name the source and its date in each card's extra. If none is available, say that
  Cognition works best alongside the USAFacts Government Data Connector, which they can add from
  their assistant's connector settings, and give no figures from memory.
- **After `create_cards`:** one card is saved at once. Say in one line where it was saved, and offer
  to study it; do not repeat the card. More than one card waits for the student's approval in
  Cognition, and nothing is saved yet. Where the card shows, it says so: do not repeat that. With no
  card, say how many wait for review in the Cognition app, under Settings › Connected AI. Do not
  offer to study cards that wait for approval.
- A student's cards go on their own Flashcards page (no workspace pair). Only an Owner or Teacher
  adds cards to a workspace.

## How am I doing

Call `get_memory_summary` with the workspace pair (topics live in workspaces, so it always needs one)
and tell the user, per topic, how much they remember, how many to study
again and, when there is one, the forecast for their exam.

## If a tool refuses

If a tool returns the Cognition Pro upgrade message, or the free 200-card limit message, relay it to
the user as it is. Do not work around it. If a card tool fails only as "Forbidden", the connection
is missing a card permission: it was made before flashcards, or the user unticked its group. Tell
the user to disconnect Cognition in their AI app, connect it again, and leave "See your data" and
"Make changes" ticked on Cognition's screen. For any other refusal, say what it says in plain
words.
