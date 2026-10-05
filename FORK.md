# YohannF fork of open-pstack

Personal fork of [ericlitman/open-pstack](https://github.com/ericlitman/open-pstack). Claude Code installs pstack from this repo through the `open-pstack` marketplace name, so the plugin ID stays `pstack@open-pstack`.

## Differences from upstream

- Cursor pstack 0.15.10 is synced ahead of upstream open-pstack (see `CHANGES.md` and `UPSTREAM.md`).
- On Claude Code, the model sheet is read on demand when a configured role launches. Setup no longer imports it into `CLAUDE.md`, so sessions that never use pstack never load it. Codex keeps its `AGENTS.md` block.
- `/setup-pstack` stores a requested effort per role lane. One family can run at `medium` in one role and `xhigh` in another, and setup probes every distinct pair.

## Versions

The fork uses Cursor pstack's version number, not open-pstack's. After a Cursor sync, the version is exactly Cursor's (`0.15.10`). A fork-only change between two syncs adds a `-yohann.N` suffix to that number (`0.15.10-yohann.1`, then `-yohann.2`). The next Cursor sync drops the suffix.

Claude Code refreshes an installed plugin only when its version string changes, so every change bumps it. The version lives in `.claude-plugin/marketplace.json`, `plugins/pstack/.claude-plugin/plugin.json`, `plugins/pstack/.codex-plugin/plugin.json`, and the `open-pstack version` row of `UPSTREAM.md`. Then:

```bash
claude plugin marketplace update open-pstack && claude plugin update pstack@open-pstack
```

## Syncing upstream

```bash
git fetch upstream && git merge upstream/main
```

On a version conflict with open-pstack, keep the Cursor version this fork tracks.
