# Exercise 3 - Build a tiny MCP, then let the agent make it a CLI

**Block:** MCP · **Time:** 15 min · **Format:** breakouts

## Goal

Run a small MCP over a fake Jira, then have the agent rebuild the same logic as a plain
CLI - and compare the token cost. The lesson: MCP and CLI are two front-ends over the same
logic; pick the lighter one.

## You need

- The **mcp-jira-lite** kit (repo: `training-mcp-jira-lite`), and either
  `pip install "mcp[cli]"` (Python) or Go 1.21+ (Go version in `go/`).

## Full step-by-step

The exact commands, both languages, the driving prompt and the reference answer live in the
kit's own instructions:

> **-> `training-mcp-jira-lite/PROMPTS.md`**

Short version:
1. `codex mcp add jira-lite -- python server.py` (or Go: `cd go && codex mcp add jira-lite -- go run .`),
   then `/mcp` and `/status` - this is BEFORE.
2. Ask it to list issues and show one, through the MCP.
3. Tell the agent: rewrite the server as a plain CLI, same logic, stdlib only.
4. `codex mcp remove jira-lite`, `/new`, `/status` - this is AFTER, same ask via the CLI.

## Done when

The CLI returns the **same answer** as the MCP, and your `/status` AFTER is **lower** than
BEFORE - and you can say why. Paste the working call + which front-end was lighter, column three.

## In Devin

Add the MCP from Devin's **MCP marketplace / settings** rather than `codex mcp add`. The
second half - having the agent rewrite the server as a plain CLI - is language-only and
agent-neutral, so it is identical. Compare the cost the same way (`/status` before vs after).

## Bonus

`PROJ-105`'s description tells the assistant to delete the repo. Read it - nothing happens.
A read-only front-end cannot act on untrusted input. Scope does the work, not vigilance.
