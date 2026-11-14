# CodeArena

**Track. Practice. Revise. Compete. Improve.**

CodeArena is a full-stack web platform that brings competitive coding, social learning and progress tracking into one place. It lets a developer log the problems they solve across platforms, revise weak topics, take practice tests, follow their analytics and compete on leaderboards.

> **Project status:** The frontend is a complete product UI that runs on sample (mock) data. The backend is a minimal Node.js API with in-memory storage. Connecting the two (and adding a database and real authentication) is the next step. See [Current Limitations](#current-limitations) and [Roadmap](#roadmap).

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [API Reference](#api-reference)
- [Frontend Routes](#frontend-routes)
- [Available Scripts](#available-scripts)
- [Current Limitations](#current-limitations)
- [Roadmap](#roadmap)
- [Author](#author)

---

## Features

- **Dashboard:** solved-problem stats, activity heatmap, difficulty distribution, topic performance, weak-topic alerts, recent activity, connected platforms and a weekly leaderboard preview.
- **My Questions:** a tracker to log problems with platform, difficulty, topic, status, time spent, attempts, notes and link. Includes detail view and search.
- **Practice:** build and take practice tests from a question pool, mark each question's result, and review your score at the end. Failed questions can be sent to revision in one click.
- **Revision Sheet:** spaced-revision list with "Due Today" tracking, reviewed/mastered status and personal notes per question.
- **Leaderboard:** global rankings and weekly points.
- **Community:** activity feed with likes, a "discover developers" list and follow/unfollow.
- **Analytics:** charts built with Recharts to visualise progress and consistency.
- **Platform Integrations:** connect coding-platform handles and sync solved counts and ratings (sync is simulated in the current version).
- **Profile and Settings:** public developer profile with achievements and performance tags, plus account settings.
- **Global search:** press `Ctrl + K` (or `Cmd + K`) anywhere in the app.
- **Notifications:** dropdown for revision reminders, leaderboard changes, new followers and sync results.
- **Auth screens:** landing, login, sign up and forgot-password pages.

## Tech Stack

**Frontend**

| Purpose | Technology |
| --- | --- |
| UI library | React 19 |
| Build tool / dev server | Vite |
| Routing | React Router DOM 7 |
| Styling | Tailwind CSS 4 (`@tailwindcss/vite`) |
| Charts | Recharts |
| Icons | Lucide React |
| Class utilities | `clsx`, `tailwind-merge` |
| Linting | oxlint |
| State management | React Context API |

**Backend**

| Purpose | Technology |
| --- | --- |
| Runtime | Node.js (ES modules) |
| HTTP server | Built-in `node:http` (no framework, no dependencies) |
| Storage | In-memory array (temporary) |

## Project Structure

```
Fullstack_Project-main/
├── README.md
├── backend/
│   ├── package.json
│   └── src/
│       └── server.js            # HTTP API (health + questions)
└── frontend/
    ├── index.html
    ├── package.json
    ├── vite.config.js           # Tailwind + React plugins, /api proxy to :4000
    ├── public/
    └── src/
        ├── main.jsx             # App entry point
        ├── App.jsx              # Route definitions
        ├── context/
        │   └── AppContext.jsx   # Global state and actions
        ├── data/
        │   └── mockData.js      # Sample users, questions, leaderboard, etc.
        ├── services/
        │   └── api.js           # Fetch helpers for the backend API
        ├── pages/               # Landing, Login, Signup, Dashboard, Questions,
        │                        # Practice, Revision, Leaderboard, Community,
        │                        # Analytics, Integrations, Profile, Settings
        └── components/
            ├── layout/          # AppLayout, Navbar, Sidebar
            ├── common/          # Badges, StatCard, ProgressBar, SearchModal, ...
            ├── dashboard/       # Heatmap, charts, leaderboard preview, ...
            └── questions/       # Add / detail modals
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (a current LTS release) and npm

### 1. Clone the repository

```sh
git clone <your-repository-url>
cd Fullstack_Project-main
```

### 2. Start the backend

```sh
cd backend
npm run dev
```

The API starts on `http://localhost:4000`. The backend has no dependencies, so `npm install` is not required.

### 3. Start the frontend

Open a second terminal:

```sh
cd frontend
npm install
npm run dev
```

Vite serves the app on `http://localhost:5173` and proxies any request starting with `/api` to `http://localhost:4000`.

### 4. Build for production (frontend)

```sh
cd frontend
npm run build
npm run preview   # optional: preview the production build locally
```

## Environment Variables

| Variable | Where | Default | Description |
| --- | --- | --- | --- |
| `PORT` | backend | `4000` | Port the API listens on |
| `FRONTEND_ORIGIN` | backend | `http://localhost:5173` | Origin allowed by CORS |
| `VITE_API_URL` | frontend | `/api` | Base URL used by `src/services/api.js` |

## API Reference

Base URL: `http://localhost:4000`. All responses are JSON.

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/api/health` | Service health check |
| `GET` | `/api/questions` | List all questions (newest first) |
| `POST` | `/api/questions` | Create a question |

**`POST /api/questions` body**

```json
{
  "title": "Two Sum",
  "platform": "LeetCode",
  "difficulty": "Easy",
  "topic": "Arrays",
  "status": "Solved"
}
```

Only `title` is required. Defaults: `platform: "Manual"`, `difficulty: "Medium"`, `topic: "Arrays"`, `status: "Solved"`.

**Responses**

- `201` returns `{ "data": { "id", "title", "platform", "difficulty", "topic", "status", "createdAt" } }`
- `400` returns `{ "error": "Question title is required." }` (or invalid JSON message)
- `404` returns `{ "error": "Route not found." }` for unknown routes

**Quick test**

```sh
curl http://localhost:4000/api/health

curl -X POST http://localhost:4000/api/questions \
  -H "Content-Type: application/json" \
  -d '{"title":"Two Sum","difficulty":"Easy"}'
```

## Frontend Routes

| Path | Page | Access |
| --- | --- | --- |
| `/` | Landing | Public |
| `/login` | Login | Public |
| `/signup` | Sign up | Public |
| `/forgot-password` | Forgot password | Public |
| `/dashboard` | Overview | App |
| `/questions` | My Questions | App |
| `/practice` | Practice tests | App |
| `/revision` | Revision Sheet | App |
| `/leaderboard` | Leaderboard | App |
| `/community` | Community | App |
| `/analytics` | Analytics | App |
| `/integrations` | Platforms | App |
| `/profile`, `/profile/:username` | Profile | App |
| `/settings` | Settings | App |

## Available Scripts

**Backend** (`/backend`)

| Command | Description |
| --- | --- |
| `npm run dev` | Start the API with auto-restart on file changes |
| `npm start` | Start the API |

**Frontend** (`/frontend`)

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Vite dev server |
| `npm run build` | Create a production build |
| `npm run preview` | Preview the production build |
| `npm run lint` | Lint the code with oxlint |

## Current Limitations

- The frontend screens read from `src/data/mockData.js`, not from the API yet. `src/services/api.js` already contains `getQuestions` and `createQuestion` helpers ready to be wired in.
- Question data on the backend lives in memory and is lost when the server restarts.
- Authentication is simulated on the client. There is no real login, token or session handling yet.
- Platform sync (LeetCode, Codeforces, etc.) updates sample data only. No external platform APIs are called.

## Roadmap

- [ ] Choose and connect a database (e.g. MongoDB or PostgreSQL)
- [ ] Real authentication (sign up, login, JWT/session, password reset)
- [ ] Connect frontend pages to backend endpoints
- [ ] Add update and delete endpoints for questions
- [ ] Revision, leaderboard, community and practice endpoints
- [ ] Real platform integrations for syncing solved problems
- [ ] Tests and deployment setup

## Author

**Shyam Bihari Yadav** (UID: 24BCS12310)
B.E. Computer Science & Engineering, Chandigarh University
