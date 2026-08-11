# Sandbox boundary (mandatory)

- **Never** read, write, or copy files to `/Applications/ChatStudio.app/`. That directory is the installed packaged app — a build artifact, not source code.
- **Never** modify files under `web_editor/frontend/build-resources/backend/`. This is a packaging staging area, regenerated from `web_editor/backend/` during `pack-electron.sh`.
- Backend changes go in **`web_editor/backend/`** only. Frontend changes go in **`web_editor/frontend/src/`** only.
- The packaging script copies `web_editor/backend/` → `build-resources/backend/` → packaged `.app`. Any edit outside `web_editor/backend/` will be silently overwritten and may crash the running application.
- To test changes: `cd web_editor/frontend && npm run build` then `./run_pack_electron.command` and open the newly generated `.app` from `web_editor/frontend/release/`.
