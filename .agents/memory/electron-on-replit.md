---
name: Electron on Replit/NixOS
description: How to run the Electron desktop app in this Replit container (binary override, sandbox, husky/pnpm quirks)
---

# Running Electron in this workspace

**Rule:** Never run the npm-downloaded Electron binary (`node_modules/electron/dist/electron`) — it fails with `libglib-2.0.so.0: cannot open shared object file` on NixOS. Use the Nix `electron` system package and point tooling at it.

**Why:** Prebuilt Electron binaries expect FHS library paths; NixOS has none. Installing the Nix electron package provides a properly linked binary.

**How to apply:**
- electron-vite resolves the binary from `ELECTRON_EXEC_PATH` (checked before `node_modules` resolution). Set it to `$(readlink -f $(which electron))`.
- The plain `electron` npm module instead honors `ELECTRON_OVERRIDE_DIST_PATH` (a directory). electron-vite ignores that one — use `ELECTRON_EXEC_PATH` for `pnpm dev`.
- Set `NO_SANDBOX=1` (the repo's `scripts/dev.cjs` forwards it as `--no-sandbox`).
- Nix electron is v35 vs package.json's v39; dev mode works fine despite the mismatch.
- Workflow output type is `vnc`; verify the window with `DISPLAY=:0 xwininfo -root -tree` and screenshot via `import -window root`.

# Container quirks
- pnpm self-update (packageManager pinning) crashes with `pthread_create: Resource temporarily unavailable`; fixed by `pnpm config set manage-package-manager-versions false --global`.
- `pnpm install` must run with `HUSKY=0`: husky's prepare script writes `.git/config`, which the sandbox blocks as a destructive git operation.
