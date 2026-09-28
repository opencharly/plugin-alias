# AGENTS.md — plugin-alias

Standalone plugin repo for the host command-alias CLI (`command:alias`). The
plugin is a Go module at `candy/plugin-alias/` (module path
`github.com/opencharly/plugin-alias/candy/plugin-alias`); the root `charly.yml`
only declares `discover: candy` so the repo is a project and its candy is
scanned.

Canonical files:

- `candy/plugin-alias/charly.yml` — the `plugin-alias:` candy entity (`plugin:`
  block, `plan:` check).
- `candy/plugin-alias/plugin.go` + `alias.go` — the provider and the
  `charly alias` command tree/handlers.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-automation:alias` — the host command-alias surface: `charly alias
  add/install/list/remove/uninstall` and the wrapper-script model. Load before
  changing any handler.
- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, placement. Load before touching the
  provider.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-alias/` — compile the plugin module.
- `go test ./...` in `candy/plugin-alias/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The live R10 witness is `check-commands-local` in `opencharly/charly` (the
  `charly alias` end-to-end); the candy `plan:` is a build-context module check.

## Modify this repo

- This is a **command** plugin with no `InputDef` and no CUE schema — do not add
  a schema-less `plugin:` input contract.
- `add` and `install` reach the host over the generic `HostBuild("cli")` reverse
  channel; keep the handlers placement-invisible (the same provider compiles in
  or serves out-of-process).

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
