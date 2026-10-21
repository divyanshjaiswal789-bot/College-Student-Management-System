# College Student Management System

A MERN application for student records, academic marks, fee tracking, staff access, and college documents. The frontend is React + Vite; the API is Express + MongoDB.

## Run locally

Requirements: Node.js 22 or newer and MongoDB Community Server, or a MongoDB Atlas connection string.

1. Install dependencies from the project root with `npm install`, `npm install --prefix server`, and `npm install --prefix client`.
2. Copy `server/.env.example` to `server/.env`. Set `MONGODB_URI`, replace `JWT_SECRET` with a long random value, and change the initial administrator password.
3. Start MongoDB locally, or put an Atlas connection string in `MONGODB_URI`.
4. Run `npm run dev` from the project root. The frontend is at http://localhost:5173 and the API is at http://localhost:4000.
5. Sign in with the `ADMIN_EMAIL` and `ADMIN_PASSWORD` values from `server/.env`. The first API startup creates that administrator if it does not exist.

The frontend can be built with `npm run build --prefix client`. API health is available at `/api/health`.

## Roles

- Administrators manage staff, students, fee records, and deletions.
- Teachers can view student records, add and update marks, generate documents, and record fee payments.
- Permissions are checked by the API, not only hidden in the interface.

## Deploy

GitHub Pages can host the static React frontend, but it cannot run the Express API or MongoDB. Deploy the `server` directory to a Node host and provision MongoDB Atlas first. Configure the API host with `MONGODB_URI`, `JWT_SECRET`, `CLIENT_ORIGIN`, `ADMIN_EMAIL`, and `ADMIN_PASSWORD`. Set `CLIENT_ORIGIN` to the exact Pages origin, such as `https://OWNER.github.io`.

For frontend publishing:

1. Push this project to a GitHub repository on the `main` branch.
2. In the repository settings, set Pages to **GitHub Actions**.
3. In repository **Settings → Secrets and variables → Actions → Variables**, add `VITE_API_URL` with the deployed API's public base URL, without a trailing slash.
4. Push to `main` or start the **Deploy frontend to GitHub Pages** workflow manually. The published site URL is shown in the workflow's deployment environment.

The frontend workflow is in `.github/workflows/pages.yml`. Do not put database credentials or JWT secrets in frontend variables; anything prefixed with `VITE_` is included in the public browser bundle.

## API routes

- `POST /api/auth/login`, `GET /api/users/me`
- `/api/users`: administrator-only staff management
- `/api/students`: list, detail, and administrator CRUD
- `/api/marks`: teacher/admin create and update; administrator delete
- `/api/fees`: role-limited payment updates and administrator management
- `/api/documents`: bonafide, transfer certificate, and marksheet records
- `GET /api/dashboard`: role-authenticated campus summary