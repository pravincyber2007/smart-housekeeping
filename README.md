# Smart Housekeeping Task Manager

A responsive website that ranks hotel housekeeping jobs by a fixed priority score and sends a
browser push alert to free staff on the right floor, with nearest-floor fallback.
Built for the JJCET AI-PDLC Hackathon (24–25 Oct 2026).

## Run it

Prerequisite: Docker.

```bash
docker compose up
```

Then open http://localhost:5173 in desktop Chrome and allow notifications.

> The app is being built story by story; `docker compose up` works once story S-01 is merged.

## Documents

- MRD: https://claude.ai/artifact/V1tpjzqAzMFhQWUePKUXV5
- PRD: https://claude.ai/artifact/8fx4aQb67jp9kaNRaAxR5X
- Backlog and test checklist: https://claude.ai/artifact/7f69tH21SbLtYSP52CBBFT

## Folders

| Folder | What | Owner |
| --- | --- | --- |
| `backend/` | FastAPI app, scoring, routing, push, seed data | Dev |
| `frontend/` | React (Vite) screens and service worker | UI/UX + Dev |
| `tests/` | Browser tests for acceptance criteria | QA |
| `docs/` | AI log and notes | Everyone |
| `.claude/skills/` | Team skills, loaded automatically by Claude Code | — |

Team brief for Claude Code sessions: [`CLAUDE.md`](CLAUDE.md).
