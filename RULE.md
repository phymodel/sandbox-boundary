---
name: Sandbox boundary
description: Edit source only — app/backend, app/frontend/src, app/electron; never generated output (app/frontend/dist, app/build/**, /Applications/ChatStudio.app).
always: true
---

# Sandbox boundary (mandatory)

Applies to the **ChatStudio studio repo** (the checkout that holds `run_pack_electron.command`).
In any other project, apply the same split: hand-written source vs. generated output.

## Source — the only places code may be edited

- `app/backend/` — backend (Python)
- `app/frontend/src/` — web UI (TypeScript)
- `app/electron/` — desktop shell (`main.cjs` / `preload.cjs` / `backendProcess.cjs`, its own npm project)

## Generated — never edit (your change is silently overwritten)

- `app/frontend/dist/` — web build output of `npm run web:build`
- `app/build/**` — the packer's staging tree (bundled Python runtime, backend venv + backend copy)
  and the `.app` / `.dmg`; gitignored, and rebuilt from scratch on every pack
- `/Applications/ChatStudio.app` — the installed bundle

## Build & test loop

```bash
cd app && ./pack-electron.sh --fast      # stage runtime + backend, build web UI, package the .app
open app/build/release/ChatStudio.app    # artifacts live under app/build/release/
```

- The packer builds the web UI itself (`app/electron` → `pack:dir` chains
  `npm --prefix ../frontend run web:build`) — do not build the frontend by hand first.
- Web UI only, no packaging: `cd app/frontend && npm run web:build`. There is **no** `npm run build`.
- `./run_pack_electron.command` is the double-click entry point and is interactive (it waits for
  Enter to close), so from a shell or an agent use `cd app && ./pack-electron.sh` instead.
