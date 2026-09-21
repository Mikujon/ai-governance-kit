# Working with an AI coding assistant — setup, usage, and safe limits

**Purpose:** this file is the practical companion to the rest of the kit. The other files govern *what* you build; this one covers *how you drive the tool itself* — installing Claude Code or Codex CLI, which mode to run it in, how to keep token/cost usage down, what good day-to-day use looks like, and — most importantly — what it must never be given access to. Read this once before your first real project, and point new hires at it directly.

This file describes two tools by name — **Claude Code** (Anthropic) and **Codex CLI** (OpenAI) — because those are the two the team is standardizing on. The advice generalizes to any similar agentic coding assistant; if the team adopts another one later, re-check its own docs against the same headings below.

---

## 1. Installing the tools

### Claude Code

| Platform | Command |
|---|---|
| macOS / Linux / WSL | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows (PowerShell) | `irm https://claude.ai/install.ps1 \| iex` |
| Windows (CMD) | `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd` |
| macOS (Homebrew) | `brew install --cask claude-code` |
| Windows (WinGet) | `winget install Anthropic.ClaudeCode` |

Confirm with `claude --version`, then run `claude` inside a project folder and log in when prompted (a Claude Pro/Max/Team/Enterprise account, or a Console/API key). On native Windows, install [Git for Windows](https://git-scm.com/downloads/win) first so Claude Code has a real Bash tool to work with — without it, Claude Code falls back to PowerShell only.

### Codex CLI

```bash
npm install -g @openai/codex
```

Confirm with `codex --version`, then run `codex` inside a project folder and sign in with your ChatGPT or API account.

### Either tool — first thing to do in a new project

Run the init command (`/init` in both tools) to generate a starter `CLAUDE.md` (Claude Code) or `AGENTS.md` (Codex CLI) — the memory file the assistant reads at the start of every session. Put this kit's relevant starter file (`PROJECT_STARTER_T*.md`) and, for anything Tier 2+, `AI_PROJECT_STRUCTURE.md` on the assistant's reading list from day one — either by referencing them in that memory file, or by handing them over at the start of the session as `AI_INTAKE_ASSESSMENT.md` already instructs. Keep that memory file under ~200 lines: it loads into every session whether needed or not, and a bloated one is the single biggest avoidable source of wasted tokens (see Section 3).

---

## 2. Picking a mode — how much you supervise

Both tools offer a spectrum from "ask me before everything" to "run unattended." The right point on that spectrum depends on the project's **tier** from `AI_PROJECT_GUIDELINES.md` — not on how much of a hurry you're in, and not on what the tool starts you in by default (see the note below the table).

| Mode | Config value | Claude Code | Codex CLI | Use it when |
|---|---|---|---|---|
| Manual — review everything | `default` (alias `manual`) | `claude --permission-mode default` | `--ask-for-approval on-request` (the default) | Tier 2–3 work, unfamiliar code, anything touching real data |
| Plan — explore without changing anything | `plan` | `--permission-mode plan`, or `Shift+Tab` | `/plan` | The start of any non-trivial task, regardless of tier — see what the assistant intends before it touches a single file |
| Accept edits — fewer prompts on file changes | `acceptEdits` | `--permission-mode acceptEdits`, or Manual mode + sandbox auto-allow (`/sandbox`) | `--sandbox workspace-write` | Tier 0–1 iteration you're reviewing via `git diff` afterward |
| Auto — a classifier reviews instead of you | `auto` | `--permission-mode auto` | `--ask-for-approval never` (still sandboxed) | Routine, well-scoped Tier 0–1 work only, per this kit — see note below |
| Don't ask — pre-approved tools only | `dontAsk` | `--permission-mode dontAsk` | n/a (use a narrow `--sandbox`/`--ask-for-approval` combination) | Locked-down CI or scripts with an exact allowlist, never an interactive session |
| Fully unattended — no checks at all | `bypassPermissions` | `--dangerously-skip-permissions` | `--dangerously-bypass-approvals-and-sandbox` | **Inside a container/VM only** — see Section 4. Never on a laptop with real credentials or customer data reachable |

**Auto mode is not a low-stakes convenience anymore — read this before treating it as optional.** On Claude Code Pro, Max, and Team plans, **auto mode is the tool's own built-in starting mode** for interactive sessions — Claude Code does not wait for you to opt into it. Its classifier does block a real, specific list by default: secrets or credentials leaving the repository, force-push, production deploys and migrations, IAM/permission changes, disabling CI checks, destroying infrastructure, and more (Anthropic updates this list; `claude auto-mode defaults` prints the current one). That's a meaningful safety net — but it's a second AI model judging the first one's actions, not a human, and it can still misjudge something specific to your codebase or infrastructure that it has no way to know is sensitive.

**Default recommendation for this kit:** treat the tool's starting mode as something to actively check, not trust — the first thing in any session on Tier 2+ work is confirming (or switching to, `Shift+Tab`) plan or manual mode, regardless of what the session actually opened in. Start every non-trivial session in plan mode so the assistant states its approach before changing anything, then drop into manual or accept-edits for the actual implementation. Reserve auto mode for routine Tier 0–1 iteration you're watching, and unattended/bypass modes strictly for Tier 0 personal scripts or CI jobs already running inside an isolated, disposable environment (Section 4).

---

## 3. Using less — the practical cost/token guide

Token usage (and therefore cost) scales almost entirely with **how much context the assistant is carrying**, not with how hard the task is. In order of impact:

1. **Clear between unrelated tasks.** `/clear` (Claude Code) starts a fresh session for free; a long-running session re-sends its whole history on every turn, including turns that have nothing to do with what you're asking now. Rename (`/rename`) before clearing if you'll want to find the session again.
2. **Match the model to the task.** Use the lighter/default model (e.g. Sonnet) for ordinary coding; reserve the heaviest reasoning model for genuinely hard architectural decisions. Both tools let you switch per-session (`/model` in Claude Code; the reasoning-effort flag in Codex CLI).
3. **Keep the memory file short.** Move detailed, occasional-use instructions (a migration runbook, a release checklist) into a skill or a separate doc that's loaded on demand, not into `CLAUDE.md`/`AGENTS.md`, which loads on every single session whether you need it that turn or not.
4. **Prefer CLI tools over chatty integrations.** `gh`, `aws`, `gcloud` and similar CLIs are more context-efficient than an equivalent MCP integration, and disable any MCP server you're not actively using in a given project (`/mcp`).
5. **Delegate noisy output.** Test runs, log files, and long fetches burn context fast. Let a subagent or a hook filter that output down to just the failures/errors before it reaches the main conversation instead of dumping the whole thing in.
6. **Write specific prompts.** "Add input validation to the login handler in `auth.ts`" costs far less than "improve this codebase," which triggers broad, expensive exploration.
7. **Use plan mode on anything non-trivial, not just for safety.** Catching a wrong approach before code is written is cheaper than re-work after.

If the organization is running several seats, set per-user or per-team spend limits and check the usage dashboard periodically (both vendors provide one) rather than discovering a cost problem a month later.

---

## 4. What never gets access — no exceptions

This section is not optional guidance; treat every line as a hard rule, and treat a violation the same way you'd treat a real security incident, because functionally it is one.

- **No production credentials, ever, in any form the assistant can read** — not in an env var it can `cat`, not pasted into a prompt "just this once," not in a config file it has read access to. If a task genuinely requires a production secret, a human runs that one step manually; the assistant does not see the value.
- **No direct network reach to production servers, internal admin panels, or customer-data systems.** Point the assistant at a local dev/staging environment or a sandboxed copy of the data. If the task is "fix something in production," a human executes the fix the assistant proposes — the assistant does not get a live connection to do it itself.
- **No unattended/bypass mode (`--dangerously-skip-permissions`, `--dangerously-bypass-approvals-and-sandbox`) outside a disposable container or VM.** These modes remove the safety net entirely; the only acceptable place to remove a safety net is somewhere a mistake can't reach anything that matters — a throwaway container, not a laptop that also has your SSH keys, cloud CLI sessions, and password manager unlocked.
- **No real customer, financial, or health data as working material**, even for a "quick test" — use synthetic or anonymized data. This is the same rule as `AI_PROJECT_GUIDELINES.md`'s data-classification requirement; an AI assistant session is not an exception to it.
- **No auto-approval of destructive or irreversible actions** — force-pushes, dropping/altering production schemas, deleting resources the assistant didn't create in the session, disabling a test or security check to make it pass. Both tools already block several of these by default in their safer modes; don't override that with a broad allow rule to save a few prompts.
- **No repointing of remotes, API base URLs, or webhook endpoints** to a host you didn't name — a classic sign of the assistant following an injected instruction from something it read (a webpage, an issue, a file) rather than from you.
- **No browser automation that can carry your cookies/session/credentials off-site**, unless you're watching it do it.

If a task seems to require breaking one of these rules, that's a sign the task needs a human to do that specific step, not a sign to loosen the rule.

## 5. Recommended configuration

A reasonable default `settings.json` (Claude Code) or `config.toml` (Codex CLI) for a Tier 1–2 project on this kit:

- **Starting mode:** manual or plan for anyone unfamiliar with the codebase; sandboxed auto-allow for day-to-day iteration by an owner who already knows it.
- **Deny rules for anything secret**, regardless of mode:
  ```json
  {
    "permissions": {
      "deny": ["Read(./.env)", "Read(**/secrets/**)", "Read(**/*.pem)", "Read(**/*credentials*)"]
    }
  }
  ```
  A deny rule stops the exact invocation form Claude usually produces, not every possible way to reach the same file — it isn't a security boundary on its own. Pair it with sandboxing (below) for anything that actually needs enforcing.
- **Network restricted to what the task needs** — Codex CLI: `network_access = false` under `[sandbox_workspace_write]`, enabled per-domain only when required; Claude Code: deny raw `curl`/`wget` via Bash and use `WebFetch(domain:…)` allow rules instead for the domains actually needed.
- **Ask (not allow) rules for anything that pushes, deploys, or deletes** — e.g. `Bash(git push *)`, `Bash(terraform apply *)` — so these always get a human look regardless of what mode the session is in.
- **Checked into version control** where the tool supports it (Claude Code project settings), so the whole team gets the same floor and can review changes to it like any other config.
- **Tier 3 projects:** never run above manual/plan mode, and never inside a shared or long-lived environment — an ephemeral, single-purpose container per session, torn down afterward, in line with `AI_PROJECT_STRUCTURE.md`'s security requirements.

## 6. Good day-to-day habits

- **Explore, then plan, then implement, then commit.** Let the assistant read and explain before it changes anything on a codebase you don't know yet ("what does this do", "where's the entry point"), then have it write a plan in plan mode before touching files, then implement and verify against that plan, then commit. Skip planning only when you could describe the resulting diff in one sentence (a typo fix, a log line, a rename).
- **Give it something to verify against, and ask for the evidence, not just the claim.** A failing test to make pass, an expected output, a screenshot to compare against a design — so the assistant checks its own work instead of you finding the mistake later. Have it show the test output or the screenshot rather than just asserting "done."
- **Use a fresh subagent for an adversarial review before calling something finished**, especially after a long or unattended run — a reviewer with only the diff and your criteria, not the reasoning that produced the change, catches gaps the implementing session is blind to. Point it at what to check (a plan, a spec, "correctness only, not style") so it doesn't just invent nitpicks.
- Break multi-step work into an explicit numbered plan rather than one big vague ask; reference files and paste screenshots directly instead of describing where something lives.
- Test/commit incrementally rather than accepting one huge diff at the end.
- Course-correct immediately (`Esc` to stop, `Esc Esc` or `/rewind` to roll back) the moment it heads the wrong way. After two failed corrections on the same issue in one session, `/clear` and restart with a sharper prompt instead of continuing to correct — a cluttered session rarely recovers on its own.
- Keep any checked-in memory file (`CLAUDE.md` / `AGENTS.md`) ruthlessly short: for every line, ask "would removing this cause a mistake?" — if not, cut it or turn it into a hook. A bloated memory file is why an assistant starts ignoring instructions that used to work.
- Review AI-authored changes the same way you'd review a colleague's pull request — this kit's per-tier requirements (a second reviewer at T2+, a documented fallback at T3) apply exactly the same whether the diff was written by a person or an assistant.

---

## Where this fits in the kit

This file doesn't change any tier or requirement — it's how you operate the tool while meeting the requirements the rest of the kit already sets. Section 4 above is itself one concrete way the Security & data handling rules in `AI_PROJECT_GUIDELINES.md` §4 and `AI_PROJECT_STRUCTURE.md` §3 get applied at the tool level, and belongs in any Tier 3 security review as evidence of how developer tooling is configured, not just how the application itself is.

Sources consulted for this file, last checked 2026-09-21 (check them directly if something here seems to have changed — both vendors update fast): [Claude Code quickstart](https://code.claude.com/docs/en/quickstart), [Claude Code best practices](https://code.claude.com/docs/en/best-practices), [permission modes](https://code.claude.com/docs/en/permission-modes), [permissions reference](https://code.claude.com/docs/en/permissions), [managing costs](https://code.claude.com/docs/en/costs), [Codex agent approvals & security](https://learn.chatgpt.com/docs/agent-approvals-security), [Codex best practices](https://learn.chatgpt.com/guides/best-practices).
