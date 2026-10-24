# Smart Housekeeping Task Manager — team brief

JJCET AI-PDLC Hackathon, 24–25 Oct 2026. Demo: 25 Oct, 10:00, 15 minutes.
A responsive website that ranks hotel housekeeping jobs by a fixed priority score and
alerts free staff on the right floor by browser push.

## Source documents (read before building)

- MRD: https://claude.ai/artifact/V1tpjzqAzMFhQWUePKUXV5
- PRD: https://claude.ai/artifact/8fx4aQb67jp9kaNRaAxR5X
- Backlog (stories, acceptance criteria, Test checklist tab): https://claude.ai/artifact/7f69tH21SbLtYSP52CBBFT

If this file and the PRD disagree, the PRD wins. Do not build anything that is not a story in the Backlog.

## Stack

- Frontend: React (Vite), responsive, service worker for Web Push — `frontend/`
- Backend: Python FastAPI + SQLAlchemy, WebSockets for live screens, `pywebpush` for alerts — `backend/`
- Database: PostgreSQL
- Run: `docker compose up` (Postgres + backend + frontend), seed data loaded on first start
- Demo: desktop Chrome on `http://localhost:5173`. No phones, no tunnel. No AI/LLM at runtime.

## Rules the code must follow

**Priority score** (all values are settings, never hard-coded):

| Factor | Points | Setting |
| --- | --- | --- |
| Maintenance / Cleaning / Extra items / Checkout cleaning | 40 / 30 / 20 / 10 | `SCORE_MAINTENANCE`, `SCORE_CLEANING`, `SCORE_EXTRAS`, `SCORE_CHECKOUT` |
| Arrival due in the room within 120 min | +50 | `ARRIVAL_BOOST`, `ARRIVAL_WINDOW_MIN` |
| VIP guest | +15 | `VIP_BONUS` |
| Waiting | +1 per 5 min, max +20 | `WAIT_STEP_MIN`, `WAIT_MAX` |
| Accept timeout (Could, F-07) | 120 s, demo 20 s | `ACCEPT_TIMEOUT_SECONDS` |

**Routing**

- Floor = room number ÷ 100 (room 214 → floor 2). 5 floors × 40 rooms (x01–x40) = 200 rooms.
- Free staff = on shift, allocated to a floor, holding no accepted job. One active job per person.
- Alert every free staff member on the home floor; first to accept owns the job.
- Nobody free → nearest floor(s) by floor difference that have someone free; a tie alerts both.
- Decline needs a reason: Finishing another room · On break · Needs maintenance skills · Other (typed).
  A staff member who declined is never re-alerted for that job. Only the manager sees reasons.
- Push is the only alert channel. Staff cannot go on shift until notifications are allowed.

**Status flows**

- Request: Pending → Accepted → Done (the guest sees it).
- Room: Dirty → Cleaning → Clean. Only Cleaning requests and Checkout cleaning change room status.
- Guests choose Maintenance, Cleaning or Extra items. Checkout cleaning is created only by reception's checkout.
- Guest finds their room by phone number only; unknown number → "We couldn't find your stay — please contact reception".
- A second open request for the same service and room is blocked ("Already requested — Pending").

## How the team works

Three people, one story at a time, in Backlog order S-01 → S-13 (Must stories S-01–S-09 first).

| Role | Skills | Owns |
| --- | --- | --- |
| UI/UX | `frontend-design` (visual direction, once), `design-taste-frontend` (each screen) | `frontend/src/components/`, `frontend/src/styles/`, screen layouts |
| Dev | `brainstorming` (plan, once), `test-driven-development` (every story) | `backend/`, API calls, wiring screens to data, Docker, seed data |
| QA | `webapp-testing` (each story in a real browser), `systematic-debugging` (when a check fails) | `tests/`, the Test checklist tab, bug notes |

- Work on a branch per role and story, e.g. `dev/s-01-push`, `ui/s-02-guest-screens`. Merge to `main` when the story is Done.
- Pull `main` before starting each story.
- One story per prompt: paste the story and its acceptance criteria as the spec. Never "build the whole app".
- On an error: paste the exact error, the file, and what you expected; ask for the smallest fix, not a rewrite.
- Pass the acceptance criteria first; styling comes last.
- Keep an AI log: note 3–5 prompts that mattered and what the AI got wrong (hand-in item).

## Definition of Done (every story)

1. Every acceptance criterion passes in desktop Chrome on the website (localhost).
2. Code is merged to `main`.
3. QA has marked each criterion Pass in the Backlog's Test checklist tab.
