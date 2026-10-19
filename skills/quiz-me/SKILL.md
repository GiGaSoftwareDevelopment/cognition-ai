---
name: quiz-me
description: Run a Cognition quiz as a conversation. Use when the user wants to be quizzed, practise or test themselves on a Cognition quiz or subject, such as "quiz me on my Stats notes" or "let's practise my Biology quiz".
---

# Quiz me

Cognition holds the quiz and is the answer key. You ask the questions, one at a time, and Cognition
checks each answer. Follow the cognition-connector skill for confirmation and permissions.

## 1. Find the quiz

- Call `list_quizzes`. With no arguments it lists the user's private quizzes. For a quiz in a class,
  call `list_workspaces` first and pass that workspace's `workspaceType` and `workspaceId`, always
  both together.
- If one quiz clearly matches what the user asked for, use it. If several could match, list them by
  name and question count and ask the user to pick. Do not guess.
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
    (for example "Quiz — Lecture 4 (00:00–25:00)"), with a one-line description, and run it as
    below. It follows the cognition-connector skill's confirmation rules.
  - Only if no, quiz them from your own questions in this chat.
- Call `get_quiz` with the `quizId` and the same workspace pair (none for a private quiz).

## 2. Ask one question at a time

- Show one question with its choices, then wait for the user's answer. Never show the next question
  early, and never reveal or hint at an answer. `get_quiz` does not mark the correct choices.
- A true/false question has the two choices `true` and `false`.
- A question with `multipleAnswers: true` takes every choice that applies. Say "pick all that apply".
  Any other question takes exactly one choice.
- Map the user's reply (a letter, a number or the choice text) to the choice ids `get_quiz` listed.
  If the reply is unclear, ask again.

## 3. Check each answer

- Call `check_quiz_answer` with `quizId`, the same workspace pair, the `questionId` and the picked
  `choiceIds` (an array, even for one choice).
- Tell the user whether they were right or wrong, name the correct choice or choices, and give the
  explanation when the result has one. Then move to the next question.
- Keep every answer as its `questionId` and `choiceIds`. A question the user skips counts as wrong:
  leave it out of the saved answers and say it was skipped.

## 4. Finish and save

- When the questions are done, tell the user how many they got right out of the total.
- Ask whether to save the result. Saying no is fine: it was practice, and nothing is recorded.
- To save, call `save_quiz_result` with `quizId`, the workspace pair and all the answers. It does not
  save at once. It returns a confirmation with the score and "Saved results can't be undone", plus a
  `confirmationToken`. Show the user that score and ask. Only after a clear yes, call
  `save_quiz_result` again with exactly the same arguments plus the `confirmationToken`.
- A chat attempt is untimed. Once saved, the result shows in the user's Grades and Stats in
  Cognition.

## If a tool refuses

If a tool returns the Cognition Pro upgrade message, relay it to the user as it is. Do not work
around it. For any other refusal, say what it says in plain words.
