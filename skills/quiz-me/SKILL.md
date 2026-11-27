---
name: quiz-me
description: Run a Cognition quiz, in Cognition's quiz card where the chat shows it or as a conversation. Use when the user wants to be quizzed, practise or test themselves on a Cognition quiz or subject, such as "quiz me on my Stats notes", "let's practise my Biology quiz" or "quiz me on U.S. demographics".
---

# Quiz me

Cognition holds the quiz and is the answer key. Follow the cognition-connector skill for
confirmation, permissions and its In-chat cards section.

## 1. Find the quiz

- Call `list_quizzes`. With no arguments it lists the user's private quizzes. For a quiz in a workspace,
  call `list_workspaces` first and pass that workspace's `workspaceType` and `workspaceId`, always
  both together.
- If one quiz clearly matches what the user asked for, open it with `get_quiz` right away. Never ask
  whether to save a quiz that is already in Cognition: it is saved already, and the card or the chat
  quiz offers to save the score at the end. If several could match, list them by name and question
  count and ask the user to pick. Do not guess.
- If the user names a subject but no quiz matches, say so, and offer to make one from their notes.
  Make it yourself: find the note with `list_notes`, read it with `get_note`, and write the
  questions from it. A long note comes back as a list of parts: read the parts you need with their
  `from` and `to` (all of them for a whole lecture). For a long lecture, cover the part the user
  wants, or ask which part.
  - Ask how many questions, or write 5 to 10.
  - Use `multiple_choice` with one clearly correct answer and plausible wrong choices, or
    `true_false`. Each question tests one idea from the note; no duplicates.
  - Give each a short explanation grounded in the note, citing the moment (MM:SS) for a transcript.
  - Then, before asking any question, ask whether to save the quiz to Cognition: saved, the score
    counts toward their Grades and memory.
    - If yes, save it with `create_quiz` in the note's workspace, named after the note and any part
      (for example "Quiz — Lecture 4 (00:00–25:00)"), with a one-line description. Then say in one
      line where it was saved and how many questions it has, and offer to take it. Where Cognition's
      card shows, it lists them, answers hidden, so do not list them again. With no card, name the
      topics they cover in a line or two, without the answers.
    - Only if no, quiz them from your own questions in this chat.
- **A quiz on official figures** ("quiz me on U.S. demographics"): Cognition holds no public
  statistics. If a government data connector is available (the USAFacts Government Data Connector,
  Data Commons), fetch the figures from it and write the questions from them, naming the source and
  its date in each explanation. If none is available, say that Cognition works best alongside the
  USAFacts Government Data Connector, which they can add from their assistant's connector settings,
  and give no figures from memory. Ask whether to save the quiz you wrote, as above.
- Call `get_quiz` with the `quizId` and the same workspace pair (none for a private quiz), for a found
  quiz and for one you just saved. A long quiz comes in parts: `questionCount` is how many questions
  it has, and `nextFrom`, when more remain, is the `from` to pass for the next part. Read the next
  part when you reach the end of this one.

## 2. When Cognition's quiz card shows

In claude.ai and ChatGPT, `get_quiz` opens the quiz card. The card asks the questions, checks each
answer, shows the score and offers Save score.

- Do not ask its questions, check its answers or ask to save in the chat.
- The card tells you what happens: the student is taking the quiz (how many answered and correct),
  has finished (the score is not saved yet; the card offers to save it), saved the score, chose not
  to save, or added questions. Go by that.
- Never offer to save a score the card saved: a second save adds a second attempt.
- If the student asks you something about the quiz in the chat, answer without giving away an
  answer the card has not shown.

Claude Code shows no card. Without one, run the quiz in the chat, steps 3 to 5.

## 3. Ask one question at a time

- Show one question with its choices, then wait for the user's answer. Never show the next question
  early, and never reveal or hint at an answer. `get_quiz` does not mark the correct choices.
- A true/false question has the two choices `true` and `false`.
- A question with `multipleAnswers: true` takes every choice that applies. Say "pick all that apply".
  Any other question takes exactly one choice.
- Map the user's reply (a letter, a number or the choice text) to the choice ids `get_quiz` listed.
  If the reply is unclear, ask again.

## 4. Check each answer

- Call `check_quiz_answer` with `quizId`, the same workspace pair, the `questionId` and the picked
  `choiceIds` (an array, even for one choice).
- Tell the user whether they were right or wrong, name the correct choice or choices, and give the
  explanation when the result has one. Then move to the next question.
- Keep every answer as its `questionId` and `choiceIds`. A question the user skips counts as wrong:
  leave it out of the saved answers and say it was skipped.

## 5. Finish and save

- When the questions are done, tell the user how many they got right out of the total.
- Ask whether to save the result. Saying no is fine: it was practice, and nothing is recorded.
- To save, call `save_quiz_result` with `quizId`, the workspace pair and all the answers. It does not
  save at once. It returns a confirmation with the score and "Saved results can't be undone", plus a
  `confirmationToken`. Show the user that score and ask. Only after a clear yes, call
  `save_quiz_result` again with exactly the same arguments plus the `confirmationToken`. Claude Code
  shows its own confirmation form instead, which the user answers.
- A chat attempt is untimed. Once saved, the result shows in the user's Grades and Stats in
  Cognition.

## If a tool refuses

If a tool returns the Cognition Pro upgrade message, relay it to the user as it is. Do not work
around it. For any other refusal, say what it says in plain words.
