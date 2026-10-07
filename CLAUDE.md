# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Starter Express API (course project) exposing `/health` and `/users`, backed by an in-memory store.

## Commands

- `npm run dev` — start the API with auto-reload (`node --watch server.js`) on `http://localhost:3000`
- `npm test` — run the test suite (Node's built-in `node:test` runner + Supertest)
- `npm run lint` — run ESLint over the whole project
- `npm start` — start the API without auto-reload

There is no single-test-file flag wired up in `package.json`; to run one file directly use `node --test tests/users.test.js`.

## Conventions

- Routes are added via `express.Router()` in `routes/`, one file per resource, and mounted in `server.js` (e.g. `app.use("/users", usersRoutes)`) — don't put route logic directly in `server.js`.
- All data access goes through `db/store.js`; routes never touch the `users` array directly.
- Use `module.exports` / `require`, not ES module `import`/`export` — `.eslintrc.json` sets `sourceType: "script"`.

## Architecture

- `server.js` is the entry point: builds the Express `app`, mounts routers, and only calls `app.listen` when run directly (`require.main === module`), so `tests/*.test.js` can `require("../server")` and drive it with Supertest without opening a real port.
- `db/store.js` is a tiny in-memory data helper (plain array, no persistence — state resets on every restart). It's the seam where a real database would go later.
- Config is env-driven (`PORT`, see `.env.example`); real secrets would live in a git-ignored `.env`, never in code.
