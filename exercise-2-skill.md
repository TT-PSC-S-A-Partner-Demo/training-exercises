# Exercise 2 - Build your own skill

**Block:** Skills · **Time:** 15 min · **Format:** breakouts of 3-4

## Goal

Leave with one working skill built from a task you actually repeat - and see it fire from
a natural request, not just when you call it by name.

## You need

- A repeated task you brought, and a writable `~/.codex/skills/`.
- **Not a coder?** Pick a writing task you repeat: a standup update, a PR description, a release note.

## Steps

1. **Take the repetitive task you brought.**
2. **Create the skill.** By hand:
   ```bash
   mkdir -p ~/.codex/skills/<name>
   $EDITOR ~/.codex/skills/<name>/SKILL.md
   ```
   or let Codex do it: `$skill-creator`.
3. **Write the frontmatter.** `name` + a description shaped **"Does X. Use when Y"**, third person,
   with the trigger words a person would actually type:
   ```
   ---
   name: go-pr-review
   description: Reviews a Go diff for %w error wrapping, naked returns, table tests.
     Use when asked to review a PR, check staged changes, or before pushing.
   ---
   Flag any error not wrapped with %w. Require a table test for new funcs. Ignore vendor/.
   ```
4. **Confirm it loaded.** `/skills` should list it.
5. **Test the trigger.** Take **3 real requests** from your own week, phrased naturally.
   It should fire on all three **without** you typing `$<name>`.
6. **If it misses, fix the DESCRIPTION, not the steps.** Re-test. Almost every miss is a
   description problem.
7. **(Optional) Write the checks down.** Add an `evals.json` next to the skill so anyone can
   re-run them:
   ```json
   { "evals": [
     { "query": "review my staged changes",
       "expected_behavior": ["fires without naming the skill", "flags unwrapped errors"] } ] }
   ```
8. **Share.** Paste the skill name + description in the shared sheet, column two.

## Done when

- `/skills` lists it **and** it fires from a natural request, not just `$name`.
- Pasted in column two.

## Advanced variant - a skill for a legacy repo

Finished early, or want something meatier? Write a **"working in this repo" skill** that
captures a legacy codebase's build steps, conventions and landmines, so the agent stops
re-discovering them. Full instructions: [exercise-5-legacy-skill.md](exercise-5-legacy-skill.md).

## Common mistakes

- **Description written for a human**, not a matcher: "helps with code quality" fires on nothing.
- **The god-skill:** one skill for "all our standards" matches no specific request. One skill, one job.
- **Fixing the steps when the trigger misses.** The description is the trigger - fix that first.

## In Devin

Same `SKILL.md`, different folder: `.devin/skills/<name>/SKILL.md` (repo, committed) or
`~/.config/devin/skills/<name>/SKILL.md` (global). There is no `$skill-creator` - write the
file by hand, or **prompt it**: *"Create a skill that does X, triggered when I ask Y."* Call
it explicitly with `$<name>`; the skills/plugins settings show what is loaded. The trigger
test (3 real requests, fix the description if it misses) is identical.

## Why it matters

The description is always in context (~100 tokens); the body loads only when it fires. So a
big skill is cheap until used - but a bad description is pure waste, because it never fires.
