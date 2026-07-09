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
- Workflow "Desktop App" runs the Electron app under VNC output (desktop GUI, not a web preview).
- Command: sets `ELECTRON_EXEC_PATH` to the Nix-provided Electron binary (npm's prebuilt Electron cannot run on NixOS), `NO_SANDBOX=1`, then `cd apps/desktop && pnpm dev`.
- Electron comes from Nix system dependencies (v35.x) while package.json declares v39 — dev works fine with the older runtime.
- `pnpm config manage-package-manager-versions=false` is set globally; pnpm self-updating crashed in this container.
- Install dependencies with: `HUSKY=0 pnpm install` — husky's prepare script writes to `.git/config`, which is blocked here.

## Deployment
Not configured — this is a desktop (Electron) application with no web server to deploy.

## User preferences
(none recorded yet)
