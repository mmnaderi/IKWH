# IKWH — I Know What's Happening

A single file, `IKWH.md`, where the programmer answers questions about their own codebase.

## Why

AI agents write more and more of our code. The programmer's role moves from writer to architect and reviewer. But often the programmer no longer knows what is actually happening in the codebase.

`IKWH.md` is the place where the programmer sits down and answers questions about the codebase, in their own words.

## When to use it

You don't know the codebase line by line. This happens in two cases:

- **You are building the project with AI agents (agentic coding).** The code grows faster than your understanding. Run `/ikwh` as you go to keep up with what the agents built.
- **You are joining an existing project.** Run `/ikwh` to get familiar with it, from the big picture down to the details.

## The file

`IKWH.md` lives in the repository root. Each section heading is a **date**, and the section holds that day's **programmer notes**: the questions asked and the answers given.

```markdown
# IKWH - I Know What's Happening
> The programmer's own answers about this codebase, typed by hand. What is this file? https://github.com/mmnaderi/IKWH

## 2026-10-10

**Question (flow):** What happens when a payment webhook is delivered twice?
**Answer:** (the programmer's answer)

**Question (security):** Why does the auth module read the session from a cookie, not a header?
**Answer:** (the programmer's answer)
```

The labels (`Question`, `Answer`) are written in the language you chose. For example, in Persian they are `سؤال` and `جواب`.

Newest date first or last — pick one and stay consistent.

## The one rule

**The programmer types every single character of the answers, from their own understanding.**
No AI-written answers, no pasted text, no AI "polishing".

**If you don't know**, say so. The AI explains it to you in the conversation, then you explain it back in your own words. Only your explanation is recorded. If you still don't know, "I don't know" is recorded as is.

## The skill

The skill lives in [`skills/ikwh/SKILL.md`](skills/ikwh/SKILL.md). Usage: `/ikwh [category] [level] [file|ask] [language]`. It lets an AI read the codebase and produce the questions. It works in one of three ways:

1. **Write to file** (`file`) — the AI adds questions to `IKWH.md` under today's date, and the programmer fills in the answers.
2. **Ask in place** (`ask`) — the AI asks the programmer directly in the conversation, then writes the questions and answers into `IKWH.md` **exactly as given, without any change**.
3. **Review** (`review`) — the AI gives feedback in the conversation on the answers already in the file.

How it asks:

- **A teammate, not an examiner.** No scores, no trick questions. "I don't know" is a normal answer.
- **Feedback after every answer**, in the conversation only: what was right, what was missing or inexact (with a pointer into the code), and any important point you missed. You may then rewrite your answer in your own words; only the final version is recorded.
- **Big picture first.** Questions go from the overview to the details, one layer at a time: overview, inventory, roles, then (in `strict`) why and failure cases. Early questions are open and name nothing; you supply the names.
- **Only new and changed questions.** It never regenerates everything. If the code behind an answer changed since you wrote it, the skill offers a refresh.
- **First run help.** If you don't know the project yet, the skill can give you a short map of the high-level design first (it is never written into the file).
- Within a layer: bigger role in the system first, then importance, then risk.
- **At most 5 questions per run.** The programmer can ask for more and continue as long as they want.

### Strictness levels

| Level | Meaning |
|---|---|
| `light` | What the project is and for whom; its main parts. |
| `normal` | The full high-level design: main parts, how they connect, the main data path, where data lives, how it runs. |
| `strict` | Inside each part: edge cases, failures, and smaller design decisions. |

The level is how deep the questions go. The cap of 5 per run applies to all levels.

**Defaults:** level `normal`, mode `ask`, language = the one you write in. Set it with a bare word, e.g. `/ikwh flow fa`.

### Question categories

Each category is an argument of the skill, e.g. `/ikwh flow`. Run `/ikwh` alone (or `/ikwh help`) and it lists the categories, levels, and modes and asks which one you want:

| Usage | Asks about |
|---|---|
| `/ikwh stack` | Languages, frameworks, services, and dependencies: what we use and why. |
| `/ikwh flow` | How a request or piece of data travels through the system, start to finish. |
| `/ikwh architecture` | The main parts, how they fit together, and why they are shaped this way. |
| `/ikwh data` | What data is stored, where, who can change it, and what must always be true. |
| `/ikwh security` | Who can do what, where secrets live, and what is exposed. |
| `/ikwh run` | How it is built, configured, deployed, and run. |
| `/ikwh ops` | What happens when things go wrong: logging, monitoring, alerts, retries, and how you find out and recover. |

## Name

**IKWH** = **I** **K**now **W**hat's **H**appening. The file is `IKWH.md`.

## Version

- **v0.1.5** — Removed the `Files:` line; `Question` / `Answer` labels are full words in the chosen language; added the "When to use it" section.
- **v0.1.4** — Feedback after every answer (right, missing, key points; optional rewrite) and a `review` mode.
- **v0.1.3** — Top-down questions (big picture first), levels by depth (HLD by default), a teammate tone, a first-run map, and a header line in `IKWH.md`.
- **v0.1.2** — Added the `ikwh` skill (categories, levels, modes, language, "I don't know" flow).
- **v0.1.1** — Added the typing rule, strictness levels, question cap and ordering, and question categories.
- **v0.1.0** — The idea: the `IKWH.md` file and this README.
