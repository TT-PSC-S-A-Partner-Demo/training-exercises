# Exercise 1 - Trim an AGENTS.md, then prove it bites

**Block:** AGENTS.md · **Time:** 8 min · **Format:** everyone at once, no breakouts

## Goal

End with an AGENTS.md that holds *only* lines true on every task, plus one new rule you
added the show-don't-tell way - and see the agent obey it in a fresh chat without you
repeating it.

## You need

- A repo you actually care about, Codex signed in (`/status` green).
- **No repo?** Any folder with a couple of files works, or use `demo-kit-calc/`.

## Steps

1. **Scaffold it.** In the repo, run `/init`. Codex writes an `AGENTS.md` from what it sees.
2. **Read it out loud.** Notice how much is prose a human reads once, not rules the agent needs.
3. **Cut it in half.** Delete every line that is *not* true on **every** task. Aim under ~50 lines.
   Keep: build/test commands, versions, off-limits dirs, one review rule.
   Cut: architecture essays, onboarding, history.
4. **Add one real rule, show-don't-tell.** The rule plus a one-line example, e.g.:
   ```
   Wrap errors with context, no bare throw:
     good -> throw new IOException("reading " + path, e);
   ```
5. **Prove it bites.** Open a **fresh** chat and ask for something the rule covers
   (rule = error wrapping? ask: "add a function that reads a file"). The agent should
   follow the rule **without you repeating it**.
6. **Share.** Paste your one show-don't-tell line in the shared sheet, column one.

## Done when

- AGENTS.md is **under ~50 lines** and holds only always-true rules.
- The agent obeyed your new rule **unprompted** in a fresh chat.
- Your line is in column one.

## Common mistakes

- **Keeping prose.** "This project uses hexagonal architecture..." changes no behavior - cut it.
- **A rule with no example.** "Follow best practices" fires nothing. Show the rule.
- **Not proving it.** The fresh-chat test is the point; skipping it means you don't know it works.

## Why it matters

AGENTS.md is read on **every turn**, so every line is taxed every time. A tight one is a
model upgrade; a bloated one is the cost lever from Session 1 pointed at your own foot.
