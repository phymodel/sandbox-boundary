# Sandbox Boundary

ChatStudio rule — **edit source, never generated output**.

## Why

The studio repo holds directories that look interchangeable but are not: hand-written source, a
gitignored staging tree that the packer rebuilds from scratch, and the installed `.app`. An edit in
the second or third is silently overwritten by the next pack — and can leave the running app
executing code that matches neither the signed bundle nor the source tree.

## Layout

| Path | Kind |
|---|---|
| `app/backend/` | ✅ source — backend (Python) |
| `app/frontend/src/` | ✅ source — web UI (TypeScript) |
| `app/electron/` | ✅ source — desktop shell (own npm project) |
| `app/frontend/dist/` | ❌ generated — web build output |
| `app/build/**` | ❌ generated — staging tree + `.app` / `.dmg` |
| `/Applications/ChatStudio.app` | ❌ generated — installed bundle |

## Correct loop

```bash
cd app && ./pack-electron.sh --fast   # non-interactive; --fast skips the .dmg
open app/build/release/ChatStudio.app
```

The packer runs the web build for you (`app/electron` → `pack:dir` →
`npm --prefix ../frontend run web:build`). To build the web UI alone:
`cd app/frontend && npm run web:build` — note there is no `npm run build` script.

Humans can double-click `./run_pack_electron.command` instead; it wraps `pack-electron.sh` and
waits for Enter before closing.

## Rule file

[RULE.md](./RULE.md) — the always-on rule ChatStudio injects into every turn.
