# Handoff notes — hermes-plugin-holographic

This repository is a **handoff copy** of the `holographic` memory provider that shipped inside
`NousResearch/hermes-agent` under `plugins/memory/holographic/`. Nous Research is moving every memory
provider out of the core tree; this repo exists so the upstream project
can clone it, take ownership, and submit it to the [Hermes plugin catalog](https://hermes-agent.nousresearch.com/plugins)
under their own org. It is **not** an officially maintained Nous plugin.

What it is: Holographic reduced-representation local memory (SQLite + numpy).

## Install (as a user)

```
hermes plugins install NousResearch/hermes-plugin-holographic
hermes plugins enable holographic
hermes memory setup            # or: set memory.provider: holographic in config.yaml
```

Dependencies in `pyproject.toml` are installed into the Hermes venv automatically and survive
`hermes update`.

## What changed versus the in-tree copy

Mechanical only; behaviour is identical.

- Absolute self-imports (`from plugins.memory.holographic.x import ...`) became relative imports so the
  package loads from `~/.hermes/plugins/holographic/` under the loader's synthetic namespace.
- No shared helpers needed.
- Added `pyproject.toml` declaring: `numpy>=1.26,<3`.

## For the maintainer taking this over

- `config_schema.py` still imports from `plugins.memory.config_schema`: Hermes core loads that file
  directly and the dashboard expects core's `ProviderConfigSchema` type, so it must not be vendored.
- The in-tree `tools.lazy_deps.ensure("memory.holographic")` calls were removed: they pinned the exact
  (old) version in Hermes' lazy-deps registry and downgraded newer installs (hermes-agent#86992).
  `pyproject.toml` is now the only dependency authority; bump it when you need a newer client.
- The setup wizard imports private helpers from `hermes_cli.memory_setup` (`_curses_select`,
  `_prompt`, ...). Those are Hermes internals, not API; expect to own a copy or drop the wizard
  hook if they move.
- Tests were not copied: the in-tree tests import `plugins.memory.holographic` and depend on the
  hermes-agent test harness. See `tests/plugins/memory/` in hermes-agent for the originals.
- While the in-tree copy still exists, a same-named user plugin is shadowed by it
  (bundled providers win on name). It takes effect the moment core drops `plugins/memory/holographic`.

Original authors are preserved in hermes-agent's history: `git log -- plugins/memory/holographic`.
