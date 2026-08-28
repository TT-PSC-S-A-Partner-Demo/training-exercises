# Exercise 4 - Build your own orchestrator (your SDLC as a chain)

**Block:** Orchestrator · **Time:** 25 min · **Format:** breakouts

## Goal

One root agent drives four SDLC stages - **spec -> implement -> test -> review**. The first
three are skills; review is a read-only subagent. One prompt runs the whole thing on a repo
you did not write.

## You need

- The **orchestrator-kit** (repo: `training-orchestrator-kit`) - ready spec/implement/test
  skills + `reviewer.toml` + config snippet.
- `.codex/config.toml` open.
- A **target repo** (see below).

## Full step-by-step

Setup, the driving prompt, and the reference live in the kit:

> **-> `training-orchestrator-kit/PROMPTS.md`**

Short version:
1. Install the three skills into `~/.codex/skills/`.
2. Register `reviewer.toml` in `.codex/config.toml` under `[agents]`.
3. Pick a small change on your target (below).
4. One prompt to the root agent:
   *"Use spec, implement, test in order, then the reviewer subagent. The change: &lt;X&gt;.
   Stop and show me between each stage."*

## Pick a target - by what you're comfortable with

| Target | Language | The change | Repo / kit |
|---|---|---|---|
| Calculator | JS (browser) | divide-by-zero shows a message, not `Infinity` | `training-calculator` |
| dateformat | Python | blank input returns `content must not be blank` | `demo-kit-python` |
| go-jira DateFormat | Go | blank input returns a clear error | `training-gojira` |
| **xr path parser** | **Python (real Cisco repo)** | empty/whitespace path returns a clear error | see below |

### Real-repo target: Cisco xr-telemetry-m2m-lib

For "an agent on code you really didn't write", point the chain at a real Cisco library.

```bash
git clone --depth 1 https://github.com/cisco/xr-telemetry-m2m-lib
cd xr-telemetry-m2m-lib
```
- It is Python 2/3 (`from __future__ import ... print_function`), so it imports on Python 3.
- The target is the path parser in `xrm2m/_shared/_pathstr.py` (`parse()` and helpers).
- Real unguarded edge cases: an **empty string** is not validated, and `_consume_whitespace`
  has **no bounds check**.

The change to spec/implement/test/review:
```
Harden the path parser in xrm2m/_shared/_pathstr.py: an empty or whitespace-only path
should return a clear error (e.g. ValueError "path must not be blank") instead of failing
deep inside parsing. Lock today's behavior with a characterization test first, keep it
green, and add one test proving a blank path returns that message.
```
> Note: the old `distutils` `setup.py install` can be fussy - you do **not** need it. Explore
> read-only and import the module / run the function directly under your characterization
> tests. If the full clone or import fights you on the day, fall back to `demo-kit-python`.

## In Devin

Devin does not use `.codex/agents/*.toml`. Its orchestration is **"Devin manages Devins"**:
one Devin delegates the stages to a team of managed Devins working in parallel. The three
stage skills live in `.devin/skills/`, and you **trigger the chain with a prompt** that
describes the SDLC (spec -> implement -> test -> review) rather than registering agents in a
config file. Same shape, same handoffs; the review stage is a delegated Devin instead of a
`reviewer.toml` subagent.

## Done when

One prompt runs all four stages **in order**, tests end green, the reviewer runs as a
**separate** read-only agent (or a delegated Devin), and you can point to the spec that
drove the code.
