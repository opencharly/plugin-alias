# plugin-alias

Host command aliases for OpenCharly — the `charly alias` CLI.

`charly alias add/install/list/remove/uninstall` create and manage
`~/.local/bin` wrapper scripts that shell into a box (each wrapper runs
`charly shell <box> -c "<command> …"`), so a box's tools become reachable as
plain host commands. The plugin is compiled into charly (`command:alias`) and
dispatches in-process; `add` (an image-exists guard) and `install` (reads the
baked `ai.opencharly.alias` label) reach the host over the generic
`HostBuild("cli")` reverse channel. Placement is invisible: the same provider
compiles in or serves out-of-process.

## What it provides

| Capability | Surface |
|---|---|
| `command:alias` | `charly alias add` · `install` · `list` · `remove` · `uninstall` |

## How to use it

The command is compiled in — no candy composition is needed:

```bash
charly alias add <box> <name> <command...>
charly alias install <box>
charly alias list
charly alias remove <name>
charly alias uninstall <box>
```

## Layout

- `candy/plugin-alias/` — the plugin module: `plugin.go` (the provider +
  `NewMeta`), `alias.go` (the command tree + handlers), `cmd/serve/main.go`.
  The capability carries no `InputDef` and ships no CUE schema.
- `candy/plugin-alias/charly.yml` — the `plugin-alias:` candy entity (`plugin:`
  block, `plan:` check).
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-automation:alias` — the host command-alias surface.
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
