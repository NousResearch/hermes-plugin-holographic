# Maintenance notes — hermes-plugin-holographic

This repository is the `holographic` memory provider that shipped inside `NousResearch/hermes-agent` under
`plugins/memory/holographic/`. Nous Research moved every memory provider out of the core tree; this repo is
now its home, **maintained by Nous Research** and listed in the
[Hermes plugin catalog](https://hermes-agent.nousresearch.com/docs/plugins/holographic) as `holographic` (tier
`official`), pinned to a reviewed release tag. There is no upstream vendor: holographic is a Hermes-native provider and stays with Nous Research.

## Install (as a user)

```
hermes plugins install holographic     # from the catalog, at the reviewed pin
hermes memory setup            # or: set memory.provider: holographic in config.yaml
```

Users who already had `memory.provider: holographic` need do nothing: once core drops its bundled copy,
`hermes update` (and agent start) installs this plugin from the catalog automatically. Config
(`memory.holographic` / plugin sections) and data files are unchanged. Dependencies in `pyproject.toml` are
installed into the Hermes venv automatically and survive `hermes update`.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/holographic` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml` and `pyproject.toml`; the catalog
pin moves only through a hermes-agent PR that bumps `sha:` and `version:` together.

Differences versus the in-tree copy (mechanical only; behaviour is identical):

- Self-imports are relative so the package loads from `~/.hermes/plugins/holographic/` under the loader's
  synthetic namespace.
- `pyproject.toml` is the dependency authority (no `tools.lazy_deps` calls).

## For maintainers

- The in-tree `tools.lazy_deps.ensure("memory.holographic")` calls were removed: they pinned the exact
  (old) version in Hermes' lazy-deps registry and downgraded newer installs (hermes-agent#86992).
  `pyproject.toml` is now the only dependency authority; bump it when you need a newer client.
- `tests/` holds the in-tree tests (`tests/plugins/memory/test_holographic*.py` in hermes-agent) with the
  import path switched to the package `tests/conftest.py` loads from this repo. Run them locally with
  `HERMES_AGENT_REPO=~/.hermes/hermes-agent PYTHONPATH=~/.hermes/hermes-agent python -m pytest -q`;
  CI runs the same on Linux, macOS and Windows against hermes-agent `main`.
- While a Hermes install still carries the bundled `plugins/memory/holographic`, that copy wins on name and
  this plugin is dormant; it takes over once core drops the bundled directory.

Original authors are preserved in hermes-agent's history: `git log -- plugins/memory/holographic`.
