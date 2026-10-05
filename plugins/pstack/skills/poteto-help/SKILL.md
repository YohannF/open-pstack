---
name: poteto-help
description: Guides users through pstack setup, /poteto-mode, and picking the skill, playbook, or principle for a task. Use for /poteto-help, or when the user asks how to install, set up, or use pstack, or which pstack skill fits. Not for requests to do work, even ones that name pstack.
---

# Poteto help

Answer the user's question about pstack, hand them a prompt they can send, and link the file the answer came from. For a help question, don't start the work. The user asked how, and a pstack run spends real tokens, so let them send the prompt.

A message that asks for work, such as "use pstack to fix this bug", is not a help question. Read [`poteto-mode`](../poteto-mode/SKILL.md) and do the work under it.

This file maps questions to the skills and docs that hold the answers. Those files own the details. Read the file you route to before you quote it, and trust it when it disagrees with this map. The links here point into the installed plugin, which the user may not be able to open, so give the user the file's public copy: `https://github.com/YohannF/open-pstack/blob/main/plugins/pstack/` followed by its path.

## Find out what they need

Infer the need from the message and the conversation. A named situation, such as "which skill reviews a PR?", goes straight to its section. If the need is still unclear, ask one multiple-choice question with these options, then answer only the section they pick:

- Get set up
- Start a task with `/poteto-mode`
- Pick a skill for a situation
- Fix a run that went wrong
- Make pstack my own

Check the state that changes the answer, and mention it only when it does:

- No `pstack-models.md` in the harness config home (see [`codex-tools.md`](../poteto-mode/references/codex-tools.md#harness-config-homes)) means `/setup-pstack` hasn't run for this user, so every role uses its default model.
- No `verify-*` skill or other app harness in the project means agents have no scripted way to drive the app. Mention `/create-verification-skill` when the question is about proving a change works.

## Get set up

1. Install the plugin. The repository README's Install section has the Claude Code and Codex commands.
2. Run [`/setup-pstack`](../setup-pstack/SKILL.md). It maps a model and a requested effort to each role, probes every pair, and writes `pstack-models.md`. Claude Code reads it only when a configured role launches, and Codex mirrors it in a block in `AGENTS.md`.
3. Start a real task with `/poteto-mode`, a goal, and a check that can pass or fail.

The [README](https://github.com/YohannF/open-pstack#readme) has the details. Offer to word their first prompt with them.

If cost is the worry, say where the tokens go and how to spend fewer. pstack spends extra tokens on subagents and review panels. Rerun `/setup-pstack` and pick lower efforts or cheaper models. A role set to `auto` or `inherit-parent` runs on the session's model, which saves tokens when the session runs on a cheaper model. A shorter panel list runs fewer subagents, one for each entry. External lanes also need the Codex and Grok command-line tools, and a role that names neither skips them. Save `/poteto-mode` for work that needs rigor.

## Start a task with `/poteto-mode`

`/poteto-mode` matches the task to a playbook, copies the playbook's steps into the todo list, and runs the other skills as the steps need them. A step it skips stays in the list as `skip: <reason>`. A good prompt states the goal and how to tell it's done. It doesn't list skills, because a hand-written sequence tends to drop or reorder steps the playbook would keep.

Whether `/poteto-mode` runs on its own depends on the harness:

- Claude Code injects a SessionStart instruction that routes every non-trivial engineering task through `pstack:poteto-mode`. User instructions in `CLAUDE.md` or `AGENTS.md` take precedence over it.
- Codex has no plugin hook runtime. Invoke `pstack:poteto-mode` by name for each task, or add a standing instruction to `~/.codex/AGENTS.md`.

Mid-session, "new task" makes the mode match a fresh playbook. `/poteto-mode` already uses `poteto-agent` for the subagents its playbook steps spawn. To get the same style from a subagent of your own on Claude Code, spawn it with `subagent_type: "pstack:poteto-agent"`. On Codex, spawn an agent told to read `poteto-mode` first.

## Pick a skill

The default answer is `/poteto-mode`, which runs most of the others when its steps need them. Name a skill directly when the user wants more or less of something than the playbook gives. Read the skill before you recommend it, and give one example prompt.

| The user wants to | Skill |
|---|---|
| Do any non-trivial task with rigor | [`/poteto-mode`](../poteto-mode/SKILL.md) |
| Know how code works now, or where new code should live | [`/how`](../how/SKILL.md) |
| Know why code is shaped this way, or where a number came from | [`/why`](../why/SKILL.md) |
| Understand a change or subsystem, explained plainly | [`/teach`](../teach/SKILL.md) |
| Catch up on their own recent work on a topic | [`/recall`](../recall/SKILL.md) |
| Know what a small diff could break outside itself | [`/blast-radius`](../blast-radius/SKILL.md) |
| Settle types and module shape before code that crosses a function boundary | [`/architect`](../architect/SKILL.md) |
| Get several attempts at one brief, merged into the best one | [`/arena`](../arena/SKILL.md) |
| Run parallel checks over slices, or race workers | [`/swarm`](../swarm/SKILL.md) |
| Have several models review a diff and try to break it | [`/interrogate`](../interrogate/SKILL.md) |
| Fix a bug test-first when a cheap local test exists | [`/tdd`](../tdd/SKILL.md) |
| Apply TypeScript rules to `.ts` or `.tsx` work | [`/typescript-best-practices`](../typescript-best-practices/SKILL.md) |
| Strip AI slop from a diff before commit | [`/deslop`](../deslop/SKILL.md) |
| Strip comments before review, using a reviewer that didn't write them | [`/no-comments`](../no-comments/SKILL.md) |
| Clean AI tells out of prose | [`/unslop`](../unslop/SKILL.md) |
| Write docs, an RFC, a README, a PR description, or a commit message to a standard | [`/technical-writing`](../technical-writing/SKILL.md) |
| Hear the last reply again in plain words | [`/bro`](../bro/SKILL.md) |
| Drive a PR to green, or fix its CI, conflicts, or review comments | [`/babysit`](../babysit/SKILL.md), [`/fix-ci`](../fix-ci/SKILL.md), [`/fix-merge-conflicts`](../fix-merge-conflicts/SKILL.md), [`/get-pr-comments`](../get-pr-comments/SKILL.md) |
| Give agents a scripted way to drive the app and prove behavior | [`/create-verification-skill`](../create-verification-skill/SKILL.md) |
| Bring a verification skill and its feature map back in line with the app | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.md) |
| Vet a performance number before reporting or acting on it | [`/benchmark-checklist`](../benchmark-checklist/SKILL.md) |
| Run a large or cross-cutting change, or one to review after stepping away | [`/figure-it-out`](../figure-it-out/SKILL.md) |
| Keep a decision log during a run, and review it afterward | [`/show-me-your-work`](../show-me-your-work/SKILL.md) |
| Pick a model and a requested effort for each role | [`/setup-pstack`](../setup-pstack/SKILL.md) |
| Turn their own working habits into a personal mode skill | [`/automate-me`](../automate-me/SKILL.md) |
| Turn what a finished task taught into skill edits | [`/reflect`](../reflect/SKILL.md) |
| Stop agents from repeating the same mistakes in this repo | [`/correct`](../correct/SKILL.md) |
| Find their way around pstack | `/poteto-help` |

If a skill directory next to this one is missing from the table, read its frontmatter and route by its description. The `principle-*` directories are covered under principles below.

Close calls:

- `/how` explains what the code does. `/why` explains the reasons. `/teach` runs one or both and explains the result plainly.
- `/arena` gives every worker the same brief and merges the best parts. `/swarm` splits work into slices or a race and returns one report.
- `/architect` implements right after it settles the design. Add "with checkpoint" to review the design before it writes code.
- `/interrogate` reviews the diff. `/blast-radius` looks for breakage outside the diff and proves the one fact that makes the change safe.
- `/recall` rebuilds context across recent sessions. Resuming one specific session or branch is the Session pickup playbook.
- `/figure-it-out` designs one rigorous run. The Orchestrate playbook runs a program that spans days and many PRs. The Autonomous run playbook drives one task to a finish condition.

Not in pstack:

- Driving a CLI or a UI uses Claude Code's built-in `run` and `verify` skills. On Codex, see [`codex-tools.md`](../poteto-mode/references/codex-tools.md).
- `/loop` is a Claude Code built-in. Skill authoring uses Claude Code's `plugin-dev:skill-development` skill.
- pstack has no `/orchestrate` skill. Orchestrate is a `/poteto-mode` playbook. If the slash menu shows `/orchestrate`, another plugin provides it.

## Playbooks and principles

Playbooks are step lists inside `/poteto-mode`, not skills, so they have no slash command. Inside `/poteto-mode`, describing the task picks one, and these phrases name one directly:

- "babysit this pr" or "check on pr 123" runs Babysit. It drives the PR to merge-ready and stops there. It doesn't merge unless the user asks to merge, land, or ship.
- "land the stack" runs Shipping.
- "take over this branch" runs Session pickup.
- "pause safely" runs Pause safely.
- "full autopilot on this queue" runs Autopilot-full. "stack them, don't ship" runs Autopilot-stack.
- "run the eval playbook" runs Eval.

The Playbooks section of [`poteto-mode`](../poteto-mode/SKILL.md) lists every playbook and when it applies.

pstack has no planning skill. Claude Code's plan mode works alongside it. For work that spans phases or stacked PRs, asking `/poteto-mode` for a plan runs the [Multi-phase plan playbook](../poteto-mode/playbooks/multi-phase-plan.md), which writes the plan and doesn't implement it. For a design question, the Prototype playbook or `/architect` settles it in code first.

Principles are one-rule skills that `/poteto-mode` reads and cites in its replies. They stay out of the slash menu. The user steers with the names instead, as in "apply prove it works. show me the real output." The Principles section of [`poteto-mode`](../poteto-mode/SKILL.md) lists them.

## Fix a run that went wrong

| Symptom | Fix |
|---|---|
| The mode stopped applying after a few turns | Start the next task with `/poteto-mode`. On Codex, add the standing `AGENTS.md` instruction. |
| A question got treated as the next step of the last task | Say "new task", or say the turn doesn't need the mode. |
| A new model choice had no effect | On Codex, the `AGENTS.md` block loads at session start, so start a new session. On Claude Code, the sheet is read once per run, so start a new run. |
| An external lane dropped out | Read its receipt. Rerun `/setup-pstack` to re-probe the CLI and its login. |
| Runs cost more than expected | See the cost paragraph under Get set up. |
| Parallel agents overwrote each other | Give each writer its own worktree. |
| An overnight run moved but finished nothing | `/loop` needs a check that can pass or fail, not a duration. |
| The reply claims success from a green build | Ask for the real command, flow, stored value, or profile. That's the prove-it-works principle. |

## Make pstack my own

- [`/automate-me`](../automate-me/SKILL.md) drafts a personal mode skill from the user's own history, to use alongside `/poteto-mode`.
- [`/reflect`](../reflect/SKILL.md) after a session turns its lessons into skill edits the user approves.
- `/poteto-mode write a skill for <workflow>` runs the authoring playbook. The eval playbook tests a skill change blind.
- Fix a misbehaving skill in its own PR, not inside the feature work where it went wrong.

## Reply

Lead with the answer. Give at most one example prompt in a code block, then the link to that file. Keep it short unless the user asked for the whole map.
