# Flowdesk

Flowdesk is a full-stack service operations workspace built for the Week 7-8 capstone. It demonstrates a React SPA connected to an Express API, JWT authentication, MongoDB Atlas persistence, CRUD ticket workflows, production security, and deployment-ready configuration.

## Architecture

- `client/`: Vite + React single-page application with React Router, Axios interceptors, protected routes, form state, responsive dashboard UI, skeleton-safe loading states, and error notices.
- `server/`: Express REST API with Helmet, CORS, Zod validation, JWT auth, Mongoose indexes, and ticket CRUD controllers.
- Storage automatically uses MongoDB when `MONGODB_URI` is provided and a seeded in-memory demo store otherwise.

## Local setup

```bash
npm install
Copy-Item server/.env.example server/.env
npm run dev
```

Open `http://localhost:5173`. The demo accepts any valid email and a password of at least six characters. Use an email containing `admin` to preview the admin role.

## API endpoints

| Method | Endpoint | Auth | Purpose |
| --- | --- | --- | --- |
| GET | `/api/health` | No | Health and storage status |
| POST | `/api/auth/login` | No | Issue a JWT session |
| GET | `/api/me` | JWT | Return current user |
| GET | `/api/dashboard` | JWT | Return queue metrics |
| GET | `/api/tickets` | JWT | Search/filter ticket queue |
| POST | `/api/tickets` | JWT | Create a ticket |
| PATCH | `/api/tickets/:id` | JWT | Update a ticket |
| DELETE | `/api/tickets/:id` | JWT | Delete a ticket |

## Production deployment

Build the frontend with `npm run build --workspace client` and deploy `client/` to Vercel or Netlify. Deploy `server/` to Render or Railway with `MONGODB_URI`, `JWT_SECRET`, `CLIENT_ORIGIN`, and `NODE_ENV=production` environment variables. `render.yaml` and `client/vercel.json` are included as starting points.

Production hardening includes Helmet security headers, strict JSON body limits, Zod input validation, CORS origin configuration, no sensitive secrets in source, indexed `status`, `priority`, and `createdAt` fields, and development-only error detail.
