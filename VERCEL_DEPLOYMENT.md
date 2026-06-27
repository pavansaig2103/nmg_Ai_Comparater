# Vercel Deployment Guide

This repository is configured to deploy both the Vite frontend and Express API on Vercel.

## Required Services

- Vercel project connected to this repository.
- A hosted PostgreSQL database, such as Vercel Postgres, Neon, Supabase, or Railway.

SQLite is not supported for this deployment because Vercel serverless functions do not provide durable writable local storage.

## Vercel Environment Variables

Set these in the Vercel project settings before deploying:

```env
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE?sslmode=require
JWT_SECRET=use-a-long-random-secret
OPENAI_API_KEY=optional-openai-key
GEMINI_API_KEY=optional-gemini-key
```

Optional:

```env
CORS_ORIGIN=https://your-custom-domain.com
```

Do not set `VITE_API_URL` for the normal single-project Vercel deployment. The frontend calls the same deployment at `/api/...`.

## First Database Setup

After setting `DATABASE_URL`, create the tables and seed starter data:

```bash
npm install --prefix backend
npm run db:push
npm run db:seed
```

The seed creates:

```text
Email: admin@nethigupta.com
Password: AdminPass123!
```

## Vercel Build Settings

The root `vercel.json` already defines:

- Build command: `npm run vercel-build`
- Output directory: `frontend/dist`
- API function: `api/index.js`
- SPA fallback: `/index.html`

No separate Render backend is required.

## Local Development

Create `backend/.env` from `backend/.env.example`, then run:

```bash
npm run install-all
npm run db:push
npm run db:seed
npm run dev
```

The frontend runs on Vite and the backend runs on Express locally.
