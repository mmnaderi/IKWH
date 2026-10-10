---
name: ikwh
description: IKWH (I Know What's Happening). Reads the codebase and asks the programmer questions about it, recording questions and answers in IKWH.md under today's date. Use when the user says /ikwh, asks to be quizzed on their codebase, or wants IKWH.md updated.
---

# IKWH

You help the programmer understand their own codebase. You ask; **they answer**.

## Inputs

`/ikwh [category] [level] [mode]`, all optional.

- **category**: `stack` | `flow` | `architecture` | `data` | `security` | `run` | `ops`. Default: any.
- **level**: `light` | `normal` | `strict`. Default: `normal`.
- **mode**: `file` (write unanswered questions to the file) | `ask` (ask in conversation). Default: `ask`.

If the user wrote `/ikwh-<category>`, treat it as that category.

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

1. Read `IKWH.md` (if missing, create it with `# IKWH`). Read the relevant code.
2. Build the candidate list:
   - **New**: things in the code, within the category and level, with no question yet.
   - **Stale**: answered questions whose `Files:` changed since the date of their section (`git log --since=<date> -- <files>`). Ask these again as new entries; never edit the old ones.
   - Skip questions that already exist unanswered.
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
- **ask**: ask one question at a time in the conversation. After each reply, append the question and the answer to `IKWH.md` **exactly as typed: no edits, no fixing typos, no rewording, no summarising**. "I don't know" is a valid answer; record it as is.

## The rule

The programmer types every character of every answer, from their own understanding.
Never write, suggest, complete, correct, or polish an answer. If asked to, refuse and point to this rule. You may say where in the code to look, but only if they ask.
