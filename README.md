> **Unmaintained — looking for an owner.** Hermes Agent removes its bundled `holographic` memory provider from core on **October 15, 2026**. This repo is a standalone copy of that provider, kept by Nous Research only so someone can take it over: Nous does not maintain it, publish fixes for it, or list it in the Hermes plugin catalog. Anyone who wants to own it: open an issue here and we'll help with the handoff (repo transfer or fork, plus the catalog entry that lets `hermes update` move existing users onto your plugin). See [HANDOFF.md](HANDOFF.md).

# Holographic Memory Provider

Local SQLite fact store with FTS5 search, trust scoring, entity resolution, and HRR-based compositional retrieval.

## Requirements

None — uses SQLite (always available). NumPy optional for HRR algebra.

## Setup

```bash
hermes memory setup    # select "holographic"
```

Or manually:
```bash
hermes config set memory.provider holographic
```

## Config

Config in `config.yaml` under `plugins.hermes-memory-store`:

| Key | Default | Description |
|-----|---------|-------------|
| `db_path` | `$HERMES_HOME/memory_store.db` | SQLite database path |
| `auto_extract` | `false` | Auto-extract facts at session end |
| `default_trust` | `0.5` | Default trust score for new facts |
| `hrr_dim` | `1024` | HRR vector dimensions |

## Tools

| Tool | Description |
|------|-------------|
| `fact_store` | 9 actions: add, search, probe, related, reason, contradict, update, remove, list |
| `fact_feedback` | Rate facts as helpful/unhelpful (trains trust scores) |
