# Flowdesk

> A focused service operations workspace for turning customer requests into clear team action.

Flowdesk is a full-stack capstone application that brings ticket intake, ownership, prioritization, and resolution tracking into one calm operational dashboard. It demonstrates an end-to-end production workflow: a React single-page application, an authenticated Express API, MongoDB persistence, secure configuration, optimized queries, and deployment-ready infrastructure.

## Highlights

- JWT authentication with persistent client sessions
- Axios request and response interceptors
- Protected dashboard routes with automatic session recovery
- Ticket lifecycle management: create, search, filter, update, and delete
- Status flow from `open` to `in progress` to `resolved`
- Priority levels, customer context, ownership, and queue metrics
- Responsive dashboard designed for desktop and mobile screens
- MongoDB Atlas persistence through Mongoose
- Seeded in-memory fallback for local demos without MongoDB
- Helmet security headers, CORS policy, input validation, and request limits
- Production build configuration for Vercel, Netlify, Render, and Railway

## Product Preview

Flowdesk is designed around a simple operational loop:

1. Sign in to a protected workspace.
2. Review queue health and unresolved work.
3. Search or filter tickets by status.
4. Create a ticket with customer, priority, and ownership context.
5. Advance the ticket through its resolution lifecycle.

## Technology Stack

| Layer | Technology |
| --- | --- |
| Frontend | React, React Router, Vite, Axios, Lucide React |
| Backend | Node.js, Express.js, Zod |
| Database | MongoDB Atlas, Mongoose ODM |
| Authentication | JSON Web Tokens (JWT) |
| Security | Helmet, CORS, environment variables, input validation |
| Testing | Node.js test runner, API smoke test |
| Deployment | Vercel or Netlify, Render or Railway |

## Project Structure

```text
flowdesk/
├── client/
│   ├── src/
│   │   ├── main.jsx       # React app, routes, auth, dashboard, CRUD UI
│   │   └── styles.css     # Responsive product interface
│   ├── vercel.json         # SPA rewrite configuration
│   └── vite.config.js      # Vite and API proxy configuration
├── server/
│   ├── src/
│   │   ├── index.js       # Express application and API routes
│   │   └── store.js       # Mongoose model and demo storage adapter
│   ├── test/api.test.js   # API health smoke test
│   └── .env.example       # Server configuration template
├── render.yaml             # Render service definition
├── package.json            # npm workspace scripts
└── README.md
```

## Getting Started

### Prerequisites

- Node.js 20 or later
- npm 10 or later
- MongoDB Atlas account for persistent cloud storage (optional for the demo)

### Install

```bash
npm install
```

### Configure the API

PowerShell:

```powershell
Copy-Item server/.env.example server/.env
```

macOS/Linux:

```bash
cp server/.env.example server/.env
```

For a zero-configuration demo, leave `MONGODB_URI` empty. Flowdesk will use seeded in-memory data. For persistence, set the MongoDB Atlas connection string and replace the JWT secret with a long random value.

### Run the application

```bash
npm run dev
```

The root development command starts the API first and waits for its health endpoint before starting Vite. Open [http://localhost:5173](http://localhost:5173) in a browser.

### Demo credentials

```text
Email:    maya@flowdesk.io
Password: password
```

Any valid email and a password of at least six characters are accepted by the demo login. Use an email containing `admin`, such as `admin@flowdesk.io`, to preview the admin role.

## Environment Variables

Set these values in `server/.env` for local development or in the hosting provider's environment settings for production.

| Variable | Required | Description |
| --- | --- | --- |
| `PORT` | No | API port. Defaults to `4000`. |
| `MONGODB_URI` | Production | MongoDB Atlas connection string. |
| `JWT_SECRET` | Production | Secret used to sign authentication tokens. |
| `CLIENT_ORIGIN` | Production | Allowed frontend origin for CORS. |
| `NODE_ENV` | No | Use `production` to suppress development error details. |

Never commit `.env` files or production secrets. The repository ignores local environment files by default.

## API Reference

All protected routes require an `Authorization` header:

```http
Authorization: Bearer <jwt-token>
```

| Method | Endpoint | Auth | Description |
| --- | --- | --- | --- |
| `GET` | `/api/health` | Public | Returns API and storage status |
| `POST` | `/api/auth/login` | Public | Validates credentials and issues a JWT |
| `GET` | `/api/me` | JWT | Returns the current user session |
| `GET` | `/api/dashboard` | JWT | Returns ticket queue metrics |
| `GET` | `/api/tickets` | JWT | Lists tickets with optional `search` and `status` filters |
| `POST` | `/api/tickets` | JWT | Creates a validated ticket |
| `PATCH` | `/api/tickets/:id` | JWT | Updates ticket fields or status |
| `DELETE` | `/api/tickets/:id` | JWT | Removes a ticket |

Example login request:

```bash
curl -X POST http://localhost:4000/api/auth/login \
	-H "Content-Type: application/json" \
	-d '{"email":"maya@flowdesk.io","password":"password"}'
```

## Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the client and API together |
| `npm run build` | Create the optimized client production build |
| `npm start` | Start the API in production mode |
| `npm test` | Run the server smoke tests |
| `npm run build --workspace client` | Build only the frontend workspace |

## Deployment

### Frontend: Vercel or Netlify

1. Set the project root to `client/`.
2. Use `npm install` as the install command.
3. Use `npm run build` as the build command.
4. Publish the `client/dist` directory.
5. Configure the deployed frontend URL as the API's `CLIENT_ORIGIN`.

The included `client/vercel.json` preserves client-side routes when deployed to Vercel.

### Backend: Render or Railway

1. Set the service root to `server/`.
2. Use `npm install` as the build command.
3. Use `npm start` as the start command.
4. Configure `MONGODB_URI`, `JWT_SECRET`, `CLIENT_ORIGIN`, and `NODE_ENV=production`.
5. Point the frontend's API proxy or production API base URL at the deployed service.

The included `render.yaml` provides a starting Render service definition.

## Security and Performance

- Helmet adds HTTP security headers.
- CORS is restricted to the configured client origin.
- Zod validates login and ticket payloads before controller logic runs.
- JSON request bodies are limited to `100kb`.
- JWTs expire after eight hours.
- MongoDB fields used for filtering and sorting are indexed.
- Production responses omit internal error details.
- Development logging and source-only configuration stay outside the production build.
- Vite minifies and compresses the client bundle during production builds.

## Verification

Run the production build and API tests before deployment:

```bash
npm run build
npm test
```

The current project passes the Vite production build and the API health smoke test.

## Roadmap

- Add role-based controller permissions for admin and agent actions
- Add ticket comments and activity history
- Add pagination for larger queues
- Add automated browser coverage for login and ticket workflows
- Add CI checks for build, tests, and dependency auditing

## License

This project is intended as an educational capstone and portfolio demonstration.
