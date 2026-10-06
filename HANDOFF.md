# Maintenance notes — hermes-plugin-holographic

**Status: unmaintained, looking for an owner.** The bundled copy leaves Hermes core on **October 15, 2026**. Nous Research does not maintain memory providers. This
repository is a standalone copy of the `holographic` memory provider that ships inside `NousResearch/hermes-agent`
under `plugins/memory/holographic/`, prepared so someone else can take it over. It is **not** listed in the
Hermes plugin catalog and Nous publishes no further fixes or releases here. The last sync with core is
tag `v0.1.1`.

## Taking it over

Anyone who wants to maintain this provider: open an issue in this repo. We can
transfer the repository to you, or you can fork it. Once you maintain it, submit a catalog entry to
`NousResearch/hermes-agent` (`plugin-catalog/holographic.yaml`, see the
[plugin catalog guide](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugin-catalog)) under the
name `holographic`. That exact name matters: when core later drops its bundled copy, `hermes update` installs the
catalog plugin of the same name for users who have `memory.provider: holographic`, keeping their config and data.

## Install (as a user)

Until October 15, 2026 Hermes Agent still bundles this provider (`hermes memory setup`, or
`memory.provider: holographic` in config.yaml). After that, unless a new owner has listed it in the catalog, install this
unmaintained copy by hand: `hermes plugins install NousResearch/hermes-plugin-holographic --ref 9ea1cf9b9eab4d35881a5e49343276e9f2eaf015` (tag `v0.1.1`).
Same provider name, config and data, so nothing else changes.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/holographic` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml` and `pyproject.toml`. A future catalog entry would pin a tag by `sha:` and `version:`.

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
