# IKWH — I Know What's Happening

A single file, `IKWH.md`, where the programmer answers questions about their own codebase.

## Why

AI agents write more and more of our code. The programmer's role moves from writer to architect and reviewer. But often the programmer no longer knows what is actually happening in the codebase.

`IKWH.md` is the place where the programmer sits down and answers questions about the codebase, in their own words.

## The file

`IKWH.md` lives in the repository root. Each section heading is a **date**, and the section holds that day's **programmer notes**: the questions asked and the answers given.

```markdown
# IKWH

## 2026-10-10

**Q:** What happens when a payment webhook is delivered twice?
**A:** (the programmer's answer)

**Q:** Why does the auth module read the session from a cookie, not a header?
**A:** (the programmer's answer)
```

Newest date first or last — pick one and stay consistent.

## The skill (planned)

A skill will let an AI read the codebase and produce the questions. It runs in a chosen **strictness level**, and works in one of two ways:

1. **Write to file** — the AI adds questions to `IKWH.md` under today's date, and the programmer fills in the answers.
2. **Ask in place** — the AI asks the programmer directly in the conversation, then writes the questions and answers into `IKWH.md` **exactly as given, without any change**.

The strictness level controls how many questions are asked and how deep they go.

The skill is not part of this version.

## Name

**IKWH** = **I** **K**now **W**hat's **H**appening. The file is `IKWH.md`.

## Version

v0.1.0 — the idea.
