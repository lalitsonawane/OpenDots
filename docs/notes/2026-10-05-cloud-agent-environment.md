# 2026-10-05 — OpenDots — Cloud Agent environment

Notion: https://app.notion.com/p/3f089f54d2c2815a8aa3da69eecff860

## Outcome

OpenDots has a tested Cloud Agent environment ready to save. The app uses Node.js 24 and npm. `install` puts Node 24 on `PATH` and runs `npm ci`. `start` runs `npm run dev` (Vite on port 5173 and the API on port 4310).

Local checks passed: format, lint, typecheck, 164 tests, production build, and a browser flow that created a Space and saved a page. Draft build `bld-20261005-fb36d97c-de8b-4d82-a558-9aeab118304c` succeeded. A fresh agent booted from that build had Node v24.21.0 and the same 164 passing tests. That boot did not launch `start`, so the dev server was not already listening.

## Key decisions

- Keep the default base image and install Node 24 with nvm, then symlink `node`, `npm`, and `npx` into `/usr/local/bin`. The repository requires Node `>=24`, and the base image had Node 22.
- Do not require Intelligence, OpenAI, Slack, voice, or computer credentials for the dev environment. Spaces and pages persist in local SQLite without those keys.
- Leave Docker computers and the Playwright browser service out of boot. They are optional and documented in `docs/COMPUTERS.md` and `docs/SETUP.md`.

## Artifacts/links

- Notion: https://app.notion.com/p/3f089f54d2c2815a8aa3da69eecff860
- Tested build: `bld-20261005-fb36d97c-de8b-4d82-a558-9aeab118304c`
- Environment public id: `3b4d86c3-c0ee-11f1-bb68-864e54d14197` (dashboard URL was not returned)

## Open follow-ups

- Save the environment in the Cursor Environment panel so later agents use this setup.
- After Save, confirm a new agent writes `/tmp/cursor/start-user/start-user.log` and that `http://127.0.0.1:5173` responds without a manual start. The fresh-agent check of the draft build did not launch `start`.
- Chat, Slack, calls, and Dot computers still need their own credentials and are outside this environment.
