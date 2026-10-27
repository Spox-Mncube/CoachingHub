# CoachingHub

**Making coaching innovative.**

CoachingHub is a web portal for **Joburg Junior Squash**. It brings players, coaches, and parents together in one place, so everyone can book sessions, follow progress, and stay up to date.

> **Project status:** Phase 1 (scaffold). The app runs locally in VS Code. The backend API is not hosted yet.

---

## Table of Contents

1. [What Problem Does It Solve?](#what-problem-does-it-solve)
2. [Who Uses It?](#who-uses-it)
3. [Features by Role](#features-by-role)
4. [Tech Stack](#tech-stack)
5. [Project Structure](#project-structure)
6. [API Endpoints](#api-endpoints)
7. [Development Roadmap](#development-roadmap)
8. [Security Plan](#security-plan)
9. [Getting Started](#getting-started)

---

## What Problem Does It Solve?

Right now, a junior squash club has to juggle bookings, progress notes, and updates across phone calls, messages, and paper. CoachingHub puts it all in one website:

- **Players** can book sessions and see how they are improving.
- **Coaches** can manage their schedule and keep records of every session.
- **Parents** can follow their child's progress and book on their behalf.

---

## Who Uses It?

| Role | Who they are |
|------|--------------|
| **Player** | A junior squash player |
| **Coach** | A certified squash coach |
| **Parent** | A guardian of a junior player |

---

## Features by Role

### Player
- Book coaching sessions
- Track progress and set goals
- View the Joburg Junior rankings
- Receive updates from coaches
- Edit their own profile

### Coach
- Approve or reject booking requests
- Set their availability
- Log what happened in each session
- Post updates for the community
- View all Joburg Junior players, and the players who chose them
- Edit their own profile

### Parent
- Browse coach profiles
- Monitor their child's progress
- Book sessions for their child
- Receive updates
- Edit their own profile

---

## Tech Stack

| Technology | What we use it for |
|------------|--------------------|
| **HTML5** | The structure of each page |
| **CSS3** | Styling, using plain CSS with variables (no framework) |
| **Vanilla JavaScript (ES6+)** | Page behaviour, with no build tools needed |
| **REST API** | How the website talks to the backend, using `fetch()` |

---

## Project Structure

Each role (player, coach, parent) has its own folder. Inside each folder, every page has its own HTML, CSS, and JavaScript file.

**Why?** Changes stay safe and contained. Editing the coach booking page will never break the player booking page.

Shared code (colours, fonts, login checks, API helpers) lives in the root `css/` and `js/` folders.

```
squash-coaching-portal/
│
├── index.html                  # Landing / login page
├── README.md
├── docs/
│   ├── wireframes.md           # Page layout sketches
│   ├── user-flow.md            # How users move through the app
│   └── security-measures.md    # Security checklist
│
├── assets/
│   ├── images/
│   ├── icons/
│   └── fonts/
│
├── css/
│   └── global.css              # Shared styles (variables, reset, typography)
│
├── js/
│   ├── global.js               # Shared utilities (API helper, auth guard, nav)
│   └── api.js                  # REST API wrapper (fetch calls)
│
├── player/
│   ├── dashboard.html
│   ├── booking.html
│   ├── progress.html
│   ├── goals.html
│   ├── ranking.html
│   ├── updates.html
│   ├── profile.html
│   ├── css/                    # One CSS file per page
│   └── js/                     # One JS file per page
│
├── coach/
│   ├── dashboard.html          # Overview + lists
│   ├── bookings.html           # Approve / reject
│   ├── availability.html
│   ├── session-log.html
│   ├── updates.html
│   ├── players.html            # All Joburg Junior players
│   ├── my-players.html         # Players who chose this coach
│   ├── profile.html
│   ├── css/                    # One CSS file per page
│   └── js/                     # One JS file per page
│
└── parent/
    ├── dashboard.html
    ├── coaches.html            # View coach profiles
    ├── progress.html           # Monitor kids
    ├── booking.html            # Book for kids
    ├── updates.html
    ├── profile.html
    ├── css/                    # One CSS file per page
    └── js/                     # One JS file per page
```

> The `css/` and `js/` folders inside each role contain one file per page, named the same as the page (for example, `player/js/booking.js` powers `player/booking.html`).

---

## API Endpoints

An **API endpoint** is a web address the frontend calls to ask the backend for data or to send it something. This table is the agreed "contract" the backend must follow.

| Method | Endpoint | Used by | Purpose |
|--------|----------|---------|---------|
| POST | `/auth/login` | All | Log in |
| POST | `/auth/register` | All | Create an account |
| GET | `/players/:id` | Player, Parent, Coach | View a player profile |
| PUT | `/players/:id` | Player | Update own profile |
| GET | `/players/:id/progress` | Player, Parent, Coach | View progress |
| GET | `/players/:id/goals` | Player | View goals |
| POST | `/players/:id/goals` | Player | Add a goal |
| GET | `/rankings/joburg-junior` | Player, Parent | View rankings |
| GET | `/coaches` | Parent | List coaches |
| GET | `/coaches/:id` | Parent | View a coach profile |
| PUT | `/coaches/:id` | Coach | Update own profile |
| GET | `/coaches/:id/availability` | Coach, Parent | View availability |
| PUT | `/coaches/:id/availability` | Coach | Update availability |
| GET | `/coaches/:id/players` | Coach | View assigned players |
| GET | `/bookings?playerId=` | Player, Parent, Coach | View bookings |
| POST | `/bookings` | Player, Parent | Create a booking |
| PATCH | `/bookings/:id` | Coach | Approve or reject a booking |
| POST | `/sessions` | Coach | Log a session |
| GET | `/updates` | All | Read community updates |
| POST | `/updates` | Coach | Post an update |

**Quick guide to the methods:** `GET` reads data, `POST` creates data, `PUT` replaces data, and `PATCH` changes part of existing data.

---

## Development Roadmap

|               Phase                 |                           Goal                           |               Status               |
|-------------------------------------|----------------------------------------------------------|------------------------------------|
|                1                    | **Scaffold:** folder structure, boilerplate, docs        |             In progress            |
|                2                    | **Auth and profiles** for all three roles                |               Planned              |
|                3                    | **Booking flow** (player requests, coach approves)       |               Planned              |
|                4                    | **Progress, goals, and session logs**                    |               Planned              |
|                5                    | **Rankings and community updates**                       |               Planned              |
|                6                    | **Parent dashboards**                                    |               Planned              |
|                7                    | **Security hardening**                                   |               Planned              |
|                8                    | **Deploy**                                               |               Planned              |

---

## Security Plan

CoachingHub stores information about children, so security matters. The full checklist is in [`docs/security-measures.md`](docs/security-measures.md) and will be completed **after the functional build, before going live**. The main areas are:

1. **Authentication and authorization**
   - Never store raw passwords; hash them with bcrypt (cost 12 or higher) or Argon2.
   - Use short-lived login tokens (for example, 15-minute access token, 7-day refresh token).
   - Check the user's role on every endpoint (a player must not be able to change a coach's availability).
   - Check ownership (a coach edits only their own availability; a parent books only for their linked children).
   - On logout, clear the token on the client and invalidate the refresh token on the server.
   - Force a new login after a password change.

2. **Input validation and output encoding**
   - Validate all input on the server, never trusting the client.
   - Sanitise text fields (`notes`, `bio`, `title`, `body`) to prevent XSS attacks.
   - Display user text with `textContent`, not `innerHTML`.
   - Only accept allowed values (for example, booking status must be `pending`, `approved`, or `rejected`).
   - Reject unknown fields.

3. **Frontend security**
   - Prefer `HttpOnly` + `Secure` cookies over `localStorage` for tokens (this needs CSRF protection).
   - Store as little personal data as possible in the browser.
   - Remove sensitive `console.log` output before release.
   - Add a Content Security Policy (CSP) meta tag.
   - Add `rel="noopener noreferrer"` to external links.
   - Guard every protected page with `requireAuth(role)` in `js/global.js`.
   - Wrap `fetch()` calls in `try/catch` and redirect to login on a 401 error.
   - Never rely on client-side role checks alone; the server must verify too.

4. **API and network**
   - Use HTTPS only in production.
   - Restrict CORS to the exact frontend origin (not `*`).
   - Add rate limiting (for example, login 5 per minute, general 100 per minute).
   - Limit request body size (for example, 50 KB).
   - Lock out accounts after repeated failed logins.
   - Use CSRF tokens if using cookie-based auth.
   - Return generic error messages (do not reveal "user not found" vs "wrong password").

5. **Data protection (POPIA, South African law)**
   - Because the app handles minors' data, POPIA compliance is essential.
   - Build a parental consent flow for players under 18.
   - Collect only the minimum personal data needed.

---

## Getting Started

The backend is not hosted yet, so the project runs locally.

1. Clone this repository.
2. Open the project folder in **VS Code**.
3. Open `index.html` in your browser (or use the Live Server extension).
4. Point `js/api.js` at your local backend once it is running.

---

## License

To be decided.
