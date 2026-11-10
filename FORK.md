# YohannF fork of open-pstack

Personal fork of [ericlitman/open-pstack](https://github.com/ericlitman/open-pstack). Claude Code installs pstack from this repo through the `open-pstack` marketplace name, so the plugin ID stays `pstack@open-pstack`.

## Differences from upstream

- On Claude Code, the model sheet is read on demand when a configured role launches. Setup no longer imports it into `CLAUDE.md`, so sessions that never use pstack never load it. Codex keeps its `AGENTS.md` block.
- `/setup-pstack` stores a requested effort per role lane. One family can run at `medium` in one role and `xhigh` in another, and setup probes every distinct pair.
- `/statechart` stays model-invocable: the fork drops its `disable-model-invocation: true` line, because generated formal verification skills route to it by name and the flag blocks that route on Claude Code.

## Versions

The fork uses Cursor pstack's version number, not open-pstack's. After a Cursor sync, or a merge of an upstream release that brings one, the version is exactly Cursor's (`0.15.36`). A fork-only change between two syncs adds a `-yohann.N` suffix to that number (`0.15.36-yohann.1`, then `-yohann.2`). The next Cursor sync drops the suffix.

Claude Code refreshes an installed plugin only when its version string changes, so every change bumps it. The version lives in `.claude-plugin/marketplace.json`, `plugins/pstack/.claude-plugin/plugin.json`, `plugins/pstack/.codex-plugin/plugin.json`, and the `open-pstack version` row of `UPSTREAM.md`. Then:

```bash
claude plugin marketplace update open-pstack && claude plugin update pstack@open-pstack
```

## Syncing upstream

```bash
git fetch upstream && git merge upstream/main
```

On a version conflict with open-pstack, keep the Cursor version this fork tracks. When upstream rewrites the setup or dispatch text, re-port the differences above by hand rather than resolving hunk by hunk: `tests/skill-collision-repro.sh` (config-home invariant) and `registry.test.ts` (setup section order) check both.

Last merge: open-pstack 1.17.0 (`bf53ac9`, Cursor pstack 0.15.36) on 2026-11-10. Open-pstack 1.18.0 to 1.21.0 keep that Cursor sync point and are not merged.
