---
name: Sandbox boundary
description: Backend changes go in app/backend/ only; frontend changes go in app/frontend/src/ only.
always: true
---

# Sandbox boundary (mandatory)

- Backend changes go in **`app/backend/`** only. Frontend changes go in **`app/frontend/src/`** only.
- To test changes: `cd app/frontend && npm run build` then `./run_pack_electron.command` and open the newly generated `.app` from `app/frontend/release/`.
