# Open CoDesign

## Overview
Open-source AI design tool (Electron desktop app) — turns prompts into interactive prototypes, slide decks, and marketing assets. Multi-model BYOK (Claude, GPT, Gemini, Ollama, etc.) or ChatGPT login. MIT licensed, local-first.

## Project structure
- pnpm monorepo (pnpm 10, workspaces) with turbo
- `apps/desktop` — the Electron app (electron-vite, React 19, Tailwind 4, zustand)
- `packages/*` — core, shared, ui, providers, runtime, exporters, artifacts, i18n, templates
- `website/` — VitePress docs site
- Node >= 22 required; lint/format via biome

## Running in Replit
- Press **Run**. That's the whole first-run procedure: the "Desktop App" workflow is self-healing — it installs dependencies when `node_modules` is missing, then launches the Electron app under **VNC output** (desktop GUI, not a web preview).
- What the workflow does: points `ELECTRON_EXEC_PATH` at the Nix-provided Electron binary (npm's prebuilt Electron cannot run on NixOS), exports `NO_SANDBOX=1` and `npm_config_manage_package_manager_versions=false`, runs `HUSKY=0 pnpm install --frozen-lockfile` if `node_modules` is absent, then `cd apps/desktop && pnpm dev`.
- `NO_SANDBOX=1` disables the Chromium sandbox. That is acceptable inside this already-sandboxed container and required to launch here — do not copy it to real desktop use.
- Manual installs from the Shell tab need the same flags the workflow exports: `npm_config_manage_package_manager_versions=false HUSKY=0 pnpm install`. Without the first, pnpm's self-version management crashes with `pthread_create: Resource temporarily unavailable`; without `HUSKY=0`, husky's prepare script writes to `.git/config`, which is blocked here.
- If Run fails with "Nix electron binary not found": confirm `replit.nix` lists the electron dependency, then reload the container so Nix re-provisions.
- Known caveat: Nix provides Electron 35.x (pinned as `pkgs.electron_35` in `replit.nix` so a channel bump can't silently change the major) while `package.json` declares v39. Dev mode works on the older runtime, but features using post-35 Electron APIs may fail only here — Replit-only misbehavior is not authoritative evidence of an app bug.
- If a first install is interrupted, it can leave a partial `node_modules` that the workflow's existence check then skips. Recovery: `rm -rf node_modules`, then Run again.

## Deployment
Not configured — this is a desktop (Electron) application with no web server to deploy.

## User preferences
(none recorded yet)
