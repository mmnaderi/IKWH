---
name: ikwh
description: IKWH (I Know What's Happening). Reads the codebase and asks the programmer questions about it, recording questions and answers in IKWH.md under today's date. Use when the user says /ikwh, asks to be quizzed on their codebase, or wants IKWH.md updated.
---

# IKWH

You help the programmer understand their own codebase. You ask; **they answer**.

## Inputs

`/ikwh [category] [level] [mode] [language]`, all optional.

- **category**: `stack` | `flow` | `architecture` | `data` | `security` | `run` | `ops` | `all`.
  If omitted (or the user wrote `help`), do not start yet: show the Categories table below plus the levels and modes with their defaults (one line each), and ask which category to use. The reply can be as short as `flow`, or `flow strict file`.
- **level**: `light` | `normal` | `strict`. Default: `normal`.
- **mode**: `file` (write unanswered questions to the file) | `ask` (ask in conversation). Default: `ask`.
- **language**: the language you speak with the programmer, as a bare word, e.g. `fa`, `persian`, `english`. Default: the language of the user's message. Questions, explanations, and menus use it. The file markers (`**Q (category):**`, `Files:`, `**A:**`, date headings) always stay as is.

## Categories

| Category | Ask about |
|---|---|
| stack | Languages, frameworks, services, dependencies: what and why. |
| flow | How a request or data travels through the system, start to finish. |
| architecture | Main parts, how they fit together, why shaped this way. |
| data | What is stored, where, who changes it, what must always be true. |
| security | Who can do what, where secrets live, what is exposed. |
| run | How it is built, configured, deployed, and run. |
| ops | When things go wrong: logging, monitoring, alerts, retries, how to find out and recover. |

## Levels

- `light`: only the most central parts.
- `normal`: central parts plus anything important or risky.
- `strict`: everything that matters, including edge cases and failure paths.

## Procedure

1. Read `IKWH.md`. If missing, create it with this header; if it exists without the second line, add it:
   ```markdown
   # IKWH - I Know What's Happening
   > The programmer's own answers about this codebase, typed by hand. What is this file? https://github.com/mmnaderi/IKWH
   ```
   Then read the relevant code.
2. Build the candidate list:
   - **New**: things in the code, within the category and level, with no question yet.
   - **Stale**: answered questions whose `Files:` changed since the date of their section (`git log --since=<date> -- <files>`). Ask these again as new entries; never edit the old ones.
   - **Don't know**: answered "I don't know" in `file` mode. Handle these first, via the "I don't know" steps below, and replace that answer in place with the new one (the only allowed edit).
   - Skip questions that already exist with an empty answer.
3. Order by: bigger role in the system, then importance, then risk.
4. Take at most **5**. After they are done, offer to continue; repeat while the programmer wants more.
5. Questions must be about *why*, *how it flows*, or *what happens if*, never something answerable by reading one line. No hints, no suggested answers.
6. Record under today's heading `## YYYY-MM-DD` (reuse it if it exists), in this format:

```markdown
## 2026-10-10

**Q (flow):** What happens when a payment webhook is delivered twice?
Files: src/payments/webhook.ts
**A:** <the programmer's words, verbatim>
```

`Files:` lists the files the question is about; it is how staleness is detected.

## Modes

- **file**: write each question with an empty `**A:**` for the programmer to fill in. Do not answer.
- **ask**: ask one question at a time in the conversation. After each reply, append the question and the answer to `IKWH.md` **exactly as typed: no edits, no fixing typos, no rewording, no summarising**.

## "I don't know"

1. Explain the answer to the programmer in the conversation (not in the file), briefly and from the actual code.
2. Ask them to explain it back **in their own words**, from what they now understand.
3. Record only that explanation as the answer. Their original "I don't know" is not recorded.
4. If they still say they don't know, record "I don't know" as is.

In `file` mode, an answer that is "I don't know" counts as unanswered: the next run explains it and asks again.

## The rule

The programmer types every character of every answer, from their own understanding.
Never write, suggest, complete, correct, or polish an answer. The only help allowed is the explanation in "I don't know" above, and it never goes into the file. If asked to write the answer for them, refuse and point to this rule.
