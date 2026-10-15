---
name: Sandbox boundary
description: Studio repo — the packer's output (app/frontend/dist, app/build/**, the installed .app) is generated and must never be edited; everything hand-written is source.
always: true
---

# Edit source, never generated output

In the **ChatStudio studio repo** the generated set is closed — anything hand-written outside it is
source and may be edited.

- ❌ `app/frontend/dist/` — web build output
- ❌ `app/build/**` — packer staging tree + `.app` / `.dmg`; gitignored, rebuilt from scratch
- ❌ the installed bundle (`/Applications/ChatStudio.app` on macOS)

Editing there is silently overwritten, and can leave the app running code that matches neither the
source tree nor the signed bundle. Verify a path with `git check-ignore -v <path>`. The same split —
hand-written vs. generated — applies to any other project.

```bash
cd app && ./pack-electron.sh --fast      # stages runtime + backend, builds the web UI, packages the .app
open app/build/release/ChatStudio.app    # artifacts live under app/build/release/
```

- The packer builds the web UI itself (`app/electron` → `pack:dir` →
  `npm --prefix ../frontend run web:build`) — don't build the frontend first.
- Web UI only: `cd app/frontend && npm run web:build`. There is **no** `npm run build`.
- `./run_pack_electron.command` is the interactive double-click entry; from a shell or an agent use
  `cd app && ./pack-electron.sh`.
