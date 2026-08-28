# Exercise 5 (advanced variant of Ex 2) - A "working in this repo" skill for legacy code

**Block:** Skills · **Time:** fits inside Ex 2's 15 min for the confident, or take it home ·
**Format:** breakouts

The highest-value skill most people can write: one that captures a **legacy repo's tribal
knowledge** - how to build and test it, its conventions, its landmines, where things live -
so the agent (and your teammates) stop re-discovering it every time.

## Goal

A skill `working-in-<repo>` that fires when someone asks how to change, test, or navigate a
specific legacy codebase, and answers with the *real* steps - including the gotchas a
newcomer would trip on.

## You need

- A **legacy repo**: your own, or one of these real ones -
  - `cisco/xr-telemetry-m2m-lib` (old Python 2/3, path parser, fussy `distutils` install)
  - `training-gojira` (Go, will not `git clone` cleanly on Windows)

## Steps

1. **Explore read-only.** Ask the agent to map, for this repo:
   - how to build and run the tests (the exact commands, not a guess)
   - the module layout and where the important logic lives
   - naming / style conventions the code actually follows
   - anything that would surprise a newcomer
2. **Extract the Gotchas.** A gotcha is a fact that **contradicts a reasonable assumption** -
   the stuff that costs an afternoon. Examples:
   - "It is Python 2/3, so no f-strings; use `.format`."
   - "`setup.py install` is fussy - import the module directly instead."
   - "On Windows a plain `git clone` fails; sparse-checkout the one folder."
   - "Tests import from `_shared`, not the public API."
3. **Write the skill.** `~/.codex/skills/working-in-<repo>/SKILL.md`:
   ```
   ---
   name: working-in-xrm2m
   description: How to build, test, and navigate the xr-telemetry-m2m-lib repo -
     commands, conventions, and landmines. Use when asked to change, test, run,
     or find something in xrm2m / xr-telemetry-m2m-lib.
   ---
   Build/test: <exact commands>
   Layout: <where the key modules are>
   Conventions: <the real ones>
   Gotchas:
   - <fact that contradicts a reasonable assumption>
   - ...
   Off limits: <generated dirs, vendored code>
   ```
   If the map is large, push it into a `reference/layout.md` and link it - the body stays
   small, the detail loads only when needed (progressive disclosure).
4. **Test it fires.** Fresh chat: *"How do I run the tests in this repo?"* or *"Where does
   path parsing live?"* - it should answer from the skill and surface at least one gotcha,
   without you naming the skill.
5. **Team play.** Commit it to `.codex/skills/` (or `.devin/skills/`) so a teammate inherits
   the whole repo's onboarding by pulling.
6. **(Optional) Audit it.** Run the skill-auditor (see Exercise 2, step 9) on your
   `working-in-<repo>` skill - a repo skill grows fast, so watch the size gate and whether
   the big module map should move to a `reference/` file.

## Done when

The skill fires on a natural "how do I X in this repo" question and returns the real answer
plus at least one gotcha - and it lives in the repo so the team gets it for free.

## In Devin

Same file, `.devin/skills/working-in-<repo>/SKILL.md`. No `/skill-creator` - write it by
hand or prompt Devin to draft it from what it found while exploring.

## Why it matters

Legacy repos cost the most in **re-learning** - every new task re-discovers the same build
quirk and the same landmine. A repo skill is the cheapest onboarding doc your team will ever
ship, and unlike a wiki page the agent actually reads it. Gotchas are the payload; write
those down first.
