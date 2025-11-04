## Quick context — Workout Todo (MERN)

Short: monorepo with `frontend/` (React CRA) and `backend/` (Express + Mongoose). Frontend uses Context + custom hooks for state; backend uses JWT auth and protects workout routes with middleware.

Keep edits small and focused. Prefer to update existing files (hooks, controllers) rather than introduce new APIs.

What matters most

- Dev start: from repo root run `npm run dev` (runs frontend + backend concurrently).
- Backend dev: `cd backend && npm run dev` (nodemon server.js). Backend listens on `process.env.PORT` and connects to MongoDB via `process.env.MONGO_URI`.
- Frontend dev: `cd frontend && npm run dev` (react-scripts start). `frontend/package.json` has a proxy to `http://localhost:4000` for API calls.

Auth & data flow (concise)

- Signup/login endpoints: `POST /api/user/signup` and `POST /api/user/login` (see `backend/routes/user.js` → `backend/controllers/userController.js`).
- Controller issues a JWT using `process.env.SECRET`. Token is returned as `{ email, token }` and saved to browser localStorage by the frontend hooks (see `frontend/src/hooks/useLogin.js`).
- Protected workout routes mounted at `/api/workouts` and guarded by `backend/middleware/requireAuth.js` which expects `Authorization: Bearer <token>` header and populates `req.user`.
- Frontend includes the token in requests (examples: `Home.js` and `WorkoutForm.js` use `Authorization: Bearer ${user.token}`).

Key files to inspect when changing behavior

- Backend: `server.js`, `routes/*.js`, `controllers/*Controller.js`, `models/*Model.js`, `middleware/requireAuth.js`.
- Frontend: `src/context/*Context.js`, `src/hooks/*`, `src/pages/*`, `src/components/*`.

Project-specific conventions & pitfalls

- Auth shape: frontend stores `{ email, token }` in localStorage under key `user`. Hooks expect that shape when dispatching `LOGIN` in `AuthContext`.
- Backend `Workout` model requires `user_id` on create; controllers derive `user_id` from `req.user._id` placed by the `requireAuth` middleware.
- Use relative API paths on the frontend (e.g. `/api/workouts`) — rely on the CRA proxy during dev; in production the frontend should call the full backend URL.
- Validation pattern: controllers perform simple validation and return { error, emptyFields } for form feedback (see `createWorkout` in `workoutController.js` and how `WorkoutForm` handles `emptyFields`).

Developer workflows & debugging tips

- Logs: backend prints each request (see `server.js` middleware). Use those console messages to trace incoming requests.
- Common local troubleshooting:
  - If requests return 401: confirm the token exists in localStorage and is sent in `Authorization` header.
  - If DB connection fails: check `backend/.env` for `MONGO_URI` and `PORT`.
  - Ensure `backend/.env` is created from `backend/.env.example` and is NOT committed (gitignore should contain `backend/.env`).

When you make changes

- Update the smallest set of files to achieve the goal.
- Add unit tests only for backend logic if you modify controller/model validation (project currently has no tests).

Examples (search these snippets if unsure)

- Token verify: `backend/middleware/requireAuth.js`
- Login: `backend/controllers/userController.js`
- Frontend auth hook: `frontend/src/hooks/useLogin.js` and context in `frontend/src/context/AuthContext.js`
- Fetch workouts: `frontend/src/pages/Home.js` (calls `/api/workouts`).

If something important is missing

- Ask: which environment (local vs production) and whether you have a running MongoDB URI. I can't access secrets — create `backend/.env` with `PORT`, `MONGO_URI`, `SECRET`.

Done: keep this file concise — if you want, I can add short code-modifying rules (e.g. linting, commit-message patterns, or PR checklist) next.
