# IKWH — I Know What's Happening

A single file, `IKWH.md`, where the programmer answers questions about their own codebase.

## Why

AI agents write more and more of our code. The programmer's role moves from writer to architect and reviewer. But often the programmer no longer knows what is actually happening in the codebase.

`IKWH.md` is the place where the programmer sits down and answers questions about the codebase, in their own words.

## The file

`IKWH.md` lives in the repository root. Each section heading is a **date**, and the section holds that day's **programmer notes**: the questions asked and the answers given.

```markdown
# IKWH - I Know What's Happening

## 2026-10-10

**Q (flow):** What happens when a payment webhook is delivered twice?
Files: src/payments/webhook.ts
**A:** (the programmer's answer)

**Q (security):** Why does the auth module read the session from a cookie, not a header?
Files: src/auth/session.ts
**A:** (the programmer's answer)
```

Newest date first or last — pick one and stay consistent.

## The one rule

**The programmer types every single character of the answers, from their own understanding.**
No AI-written answers, no pasted text, no AI "polishing".

**If you don't know**, say so. The AI explains it to you in the conversation, then you explain it back in your own words. Only your explanation is recorded. If you still don't know, "I don't know" is recorded as is.

## The skill

The skill lives in [`skills/ikwh/SKILL.md`](skills/ikwh/SKILL.md). Usage: `/ikwh [category] [level] [file|ask]`, or `/ikwh-<category>`. It lets an AI read the codebase and produce the questions. It works in one of two ways:

1. **Write to file** — the AI adds questions to `IKWH.md` under today's date, and the programmer fills in the answers.
2. **Ask in place** — the AI asks the programmer directly in the conversation, then writes the questions and answers into `IKWH.md` **exactly as given, without any change**.

How it asks:

- **Only new and stale questions.** It never regenerates everything. A question is *stale* when the code it was about has changed since it was answered.
- **Ordered by** bigger role in the system, then importance, then risk.
- **At most 5 questions per run.** The programmer can ask for more and continue as long as they want.

### Strictness levels

| Level | Meaning |
|---|---|
| `light` | Only the most central parts of the codebase. |
| `normal` | Central parts plus anything important or risky. |
| `strict` | Everything that matters, including edge cases and failure paths. |

The level controls how deep and how wide the questions go. The cap of 5 per run applies to all levels.

### Question categories

Each category is a skill command:

| Command | Asks about |
|---|---|
| `/ikwh-stack` | Languages, frameworks, services, and dependencies: what we use and why. |
| `/ikwh-flow` | How a request or piece of data travels through the system, start to finish. |
| `/ikwh-architecture` | The main parts, how they fit together, and why they are shaped this way. |
| `/ikwh-data` | What data is stored, where, who can change it, and what must always be true. |
| `/ikwh-security` | Who can do what, where secrets live, and what is exposed. |
| `/ikwh-run` | How it is built, configured, deployed, and run. |
| `/ikwh-ops` | What happens when things go wrong: logging, monitoring, alerts, retries, and how you find out and recover. |

Each question lists the files it is about (`Files:`), so the skill can tell when it has become stale.

## Name

**IKWH** = **I** **K**now **W**hat's **H**appening. The file is `IKWH.md`.

## Version

v0.1.0 — the idea. Changes after the tag include the skill.
