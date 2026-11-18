# KokkaReaperMCP

Kokka & Company fork of `bonfire-systems/reaper-mcp`. CONTRIBUTING.md describes how the
code is organised and tested; follow it.

## Remotes and branches

- `origin` = `Kokka-Company/KokkaReaperMCP` (ours). `upstream` = `bonfire-systems/reaper-mcp`
  (fetch only; its push URL is `no-push`).
- One branch per change: `feat/<name>` or `fix/<name>`, merged into `origin/main` by PR.
- Sync upstream: `git fetch upstream && git merge upstream/main`.
- A fix meant for upstream branches from `upstream/main`, not our `main`, so its PR carries
  no Kokka changes. Upstream wants an issue before any new tool.

## Names

Keep the `reaper_mcp` module, the `reaper-mcp-server` command and the package name, so
upstream merges stay clean. Fork-specific text goes in the README note, not in renames.

## Every change

- A test against `tests/fake_reaper` that fails without the change.
- `uv run pytest` and `uv run ruff check src tests` pass.
- Tool name, description or parameters changed: `UPDATE_TOOL_SCHEMAS=1 uv run pytest tests/test_tool_schemas.py`.
- Where possible, `uv run pytest --live` against a scratch REAPER project (it empties the
  current project).
- The install is editable: reconnect the `reaper` MCP server (`/mcp`) to load code changes.

## Roadmap

Order of work, from building a beat with this server. One branch and PR each.

- **A, bug fixes (also send upstream):** `save_project` expands `~`; `create_bus` turns off
  the sources' master send; `create_track("folder")` closes its folder; `create_drum_pattern`
  takes `note_map`, velocity, `channel` and `swing`.
- **B, ergonomics:** `get_fx_parameters` filter and paging; `set_fx_parameter` by display
  value (move `_set_to_display` out of `mastering_tools.py`); tracks by name or GUID.
- **C, new tools:** `load_sampler` (RS5K `FILE0`/`MODE`), `set_track_record`,
  `add_fx_automation`, `find_plugins`, `delete_items` + bulk `write_midi`, sidechain
  `dest_channel` on `create_send`, analysis by time range or soloed tracks, `scan_samples`.
