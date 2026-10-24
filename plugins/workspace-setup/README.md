One-shot bootstrap for a [LayerFS](https://layerfs.ai) Branch workspace. A fresh workspace has the committed tree; secrets and machine-local files such as `.env` stay in the checkout that created them. This plugin is the generic `.worktreeinclude` plus `setup.sh` for that gap: it copies an include-list of local files into the workspace, installs dependencies, and trusts direnv and mise. It talks to neither a LayerFS daemon nor a particular agent host.

## What it adds

- **`init`** — writes `.layerfs/setup.json` with a starter copy list and direnv/mise trust defaults.
- **`check`** — prints the same plan as a dry-run apply; exits 0 when the config and directories are valid.
- **`apply`** — copies listed files from a source tree, optionally trusts direnv/mise, then runs install commands (or lockfile detection).
- **Path safety** — rejects absolute paths, `..` escapes, and configs whose real path leaves the project (including symlink escapes of `.layerfs` / `setup.json`).

## How it works

From a tree that holds untracked local files:

```bash
export LAYERFS_SETUP_FROM="$PRIMARY_CHECKOUT"
layerfs-workspace-setup check "$WORKSPACE"
layerfs-workspace-setup apply "$WORKSPACE"
```

Config lives at `.layerfs/setup.json`. Default `copy` is `.env`, `.env.local`, `.npmrc`. Omit `install` to pick one frozen-lockfile command from the workspace lockfile (`pnpm`, `bun`, `yarn`, or `npm ci`). Trust runs `direnv allow` / `mise trust` when those configs are present or will be copied.

Copy runs before trust and install. Re-running `apply` leaves existing files unless `--force`. Pass `--skip-trust` / `--skip-install` when a later run should only fill missing files.

## You need

- Node.js 20 or newer (the package has no npm dependencies)
- A LayerFS Branch/workspace directory and a source checkout that holds the local files
- Optional: `direnv` and/or `mise` on `PATH` when trust is enabled

## Limits

- Preview status: CLI and unit tests only; no LayerFS daemon or agent-host integration.
- Install commands run in a non-interactive shell (`sh -c` on macOS and Linux) and do not source `.envrc` themselves.
- Globs in `copy` are not expanded; each entry is one file or a directory copied recursively.
- Symlink-to-directory copy is allowed only when the target stays inside the source tree.
