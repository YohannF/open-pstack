# YohannF fork of open-pstack

Personal fork of [ericlitman/open-pstack](https://github.com/ericlitman/open-pstack). Claude Code installs pstack from this repo through the `open-pstack` marketplace name, so the plugin ID stays `pstack@open-pstack`.

## Differences from upstream

- `/setup-pstack` stores a requested effort per role lane. One family can run at `medium` in one role and `xhigh` in another, and setup probes every distinct pair.

## Versions

Claude Code refreshes an installed plugin only when its version string changes. Every change to this fork bumps the `-yohann.N` suffix in `.claude-plugin/marketplace.json`, `plugins/pstack/.claude-plugin/plugin.json`, and `plugins/pstack/.codex-plugin/plugin.json`, then:

```bash
claude plugin marketplace update open-pstack && claude plugin update pstack@open-pstack
```

## Syncing upstream

```bash
git fetch upstream && git merge upstream/main
```

On a version conflict, keep upstream's number and re-apply the `-yohann.N` suffix.
