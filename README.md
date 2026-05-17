<div align="center">

<img src="./public/logo.png" alt="NumberVerify — Coming Soon" width="420" />

<br />
<br />

**Verify any phone number — instantly. At any scale. At zero cost.**

<br />

[![License: MIT](https://img.shields.io/badge/License-MIT-00d4ff.svg?style=for-the-badge)](LICENSE)
[![Next.js](https://img.shields.io/badge/Next.js-14-ffffff?style=for-the-badge&logo=next.js&logoColor=white&labelColor=000000)](https://nextjs.org)
[![NestJS](https://img.shields.io/badge/NestJS-10-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)](https://nestjs.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Redis](https://img.shields.io/badge/Redis-BullMQ-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-00d4ff?style=for-the-badge&logo=github&logoColor=white)](CONTRIBUTING.md)

<br />

[🚀 Live Demo](#) &nbsp;·&nbsp; [📖 Documentation](#-api-reference) &nbsp;·&nbsp; [🐛 Report a Bug](../../issues) &nbsp;·&nbsp; [💡 Request a Feature](../../issues)

<br />

</div>

---

<br />

## 🔵 What is NumberVerify?

> *"Not all phone numbers are real. NumberVerify tells you which ones aren't — before they cost you."*

**NumberVerify** is an open-source, full-stack phone number verification platform built for developers, marketers, and data teams who need to validate phone numbers at any scale — from a single instant lookup to bulk processing millions of records — without freezing the UI or paying per-lookup API fees.

Whether you're cleaning a CRM, scrubbing a lead list, or verifying user signups at registration, NumberVerify delivers instant, reliable intelligence on every number — powered entirely by Google's `libphonenumber` library at **zero cost**.

<br />

## ✨ Features

### 🟢 Available Now — MVP

| Feature | Description |
|---|---|
| ☎️ **Single Number Check** | Type any number, get instant validation with full E.164 formatting |
| 📂 **Bulk CSV Upload** | Drag and drop a CSV of any size — we handle the rest |
| ⚡ **Queue-Based Processing** | BullMQ workers process numbers in the background — UI never freezes |
| 📡 **Real-Time Progress** | Watch bulk jobs process live via Server-Sent Events (SSE) |
| 🌍 **Country Detection** | Auto-detects country, calling code, and regional format |
| 🏷️ **Number Classification** | `MOBILE` · `FIXED_LINE` · `VOIP` · `TOLL_FREE` · `PREMIUM_RATE` · `PAGER` |
| 📤 **CSV Export** | Download your verified, cleaned list with one click |
| 🆓 **Zero Running Cost** | Built on `libphonenumber` — no external APIs, no per-lookup fees |

### 🔒 Coming Soon — Post-MVP

| Feature | Description |
|---|---|
| 🛰️ **Full Scan** | Live HLR lookup — check if a number is actively reachable right now |
| 📶 **Carrier Intelligence** | Carrier name, MNC/MCC codes via Twilio Lookup |
| 🔁 **Ported Number Detection** | Identify numbers that have moved between carriers |
| 👤 **CNAM Lookup** | Caller name identification on supported regions |
| 🔗 **Webhooks** | HTTP callbacks when a bulk job completes |
| 🔑 **API Key Access** | Programmatic access for your own apps and pipelines |
| 📊 **Analytics Dashboard** | History, trends, and verification stats at a glance |

<br />

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                      Browser  (Next.js 14)                        │
│                                                                  │
│   /check  (single)   ·   /bulk  (upload)   ·   /bulk/[id]        │
│   /full-scan  (coming soon UI)   ·   /dashboard                  │
└────────────────────────────┬─────────────────────────────────────┘
                             │  REST API  +  Server-Sent Events
                             ▼
┌──────────────────────────────────────────────────────────────────┐
│                    API Server  (NestJS / TypeScript)              │
│                                                                  │
│  NumbersModule  →  PhoneService  (libphonenumber — offline/free) │
│  JobsModule     →  BullMQ Producer  ──►  BullMQ Worker           │
│  SseModule      →  EventEmitter2   (real-time progress bridge)   │
│  AuthModule     →  JWT + bcrypt                                  │
└─────────────────────┬──────────────────────────┬─────────────────┘
                      │                          │
            ┌─────────▼───────┐       ┌──────────▼──────┐
            │   PostgreSQL    │       │      Redis       │
            │  (Prisma ORM)   │       │  (BullMQ Queue)  │
            │   Neon.tech     │       │    Upstash       │
            └─────────────────┘       └─────────────────┘
```

<br />

## 🧰 Tech Stack

| Layer | Technology | Why |
|---|---|---|
| **Frontend** | Next.js 14 — App Router | SSR, file-based routing, free on Vercel |
| **Styling** | Tailwind CSS + shadcn/ui | Utility-first, zero design cost |
| **Backend** | NestJS (Node.js) | Modular, TypeScript-native, BullMQ ready |
| **ORM** | Prisma | Type-safe, auto-generated DB client |
| **Database** | PostgreSQL | Jobs, results, users — reliable relational store |
| **Queue** | BullMQ + Redis | Background processing, retry logic, concurrency |
| **Real-time** | Server-Sent Events (SSE) | Live progress with no extra infrastructure |
| **Phone Engine** | google-libphonenumber | 100% free, works fully offline |
| **Auth** | JWT + bcrypt | Stateless, secure, no third-party dependency |
| **Free Hosting** | Vercel + Railway + Neon + Upstash | Entire stack runs free |

<br />

## 🚀 Getting Started

### Prerequisites

- **Node.js** `>= 18.x`
- **Docker + Docker Compose** (for local Postgres & Redis)
- **Git**

### 1 — Clone

```bash
git clone https://github.com/XONight/NumberVerify.git
cd numberverify
```

### 2 — Install

```bash
npm install
```

### 3 — Configure Environment

```bash
cp apps/api/.env.example   apps/api/.env
cp apps/web/.env.example   apps/web/.env.local
```

Fill in the values — see [Environment Variables](#-environment-variables) below.

### 4 — Start Local Services

```bash
docker compose up -d
# → Postgres on :5432  |  Redis on :6379
```

### 5 — Run Database Migrations

```bash
cd apps/api
npx prisma migrate dev --name init
npx prisma generate
```

### 6 — Launch

```bash
# Terminal 1 — API (http://localhost:4000)
cd apps/api && npm run start:dev

# Terminal 2 — Frontend (http://localhost:3000)
cd apps/web && npm run dev
```

Open [http://localhost:3000](http://localhost:3000) — you're live. ✅

<br />

## 📁 Project Structure

```
numberverify/
├── apps/
│   ├── web/                        ← Next.js 14 Frontend
│   │   └── src/
│   │       ├── app/
│   │       │   ├── check/          ← Single number checker page
│   │       │   ├── bulk/           ← CSV upload + live job progress
│   │       │   ├── full-scan/      ← Coming Soon placeholder UI
│   │       │   └── dashboard/      ← Overview & recent job history
│   │       ├── components/         ← Reusable UI components
│   │       ├── hooks/              ← useJobProgress (SSE), useSingleCheck
│   │       └── lib/                ← Typed API client, auth config, utils
│   │
│   └── api/                        ← NestJS Backend
│       └── src/
│           ├── modules/
│           │   ├── numbers/        ← Single check: controller + service
│           │   ├── jobs/           ← Bulk: upload, BullMQ producer + worker
│           │   ├── phone/          ← libphonenumber + Twilio (feature-flagged)
│           │   ├── sse/            ← Server-Sent Events live gateway
│           │   └── auth/           ← Register, login, JWT guards
│           └── prisma/             ← Global Prisma service & module
│
├── prisma/
│   └── schema.prisma               ← Database schema (User, BulkJob, NumberCheck)
│
├── docker-compose.yml              ← Local dev: Postgres + Redis + RedisInsight
└── README.md
```

<br />

## 🔌 API Reference

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/numbers/check` | Validate a single phone number |
| `POST` | `/api/v1/jobs/bulk` | Upload CSV and enqueue bulk verification job |
| `GET` | `/api/v1/jobs/:id` | Poll job status and progress counters |
| `GET` | `/api/v1/jobs/:id/results` | Fetch all verified results for a completed job |
| `GET` | `/api/v1/sse/job/:id` | Subscribe to live SSE progress stream |
| `POST` | `/api/v1/auth/register` | Create a new account |
| `POST` | `/api/v1/auth/login` | Authenticate and receive a JWT |

### Example — Single Check

```bash
curl -X POST http://localhost:4000/api/v1/numbers/check \
  -H "Content-Type: application/json" \
  -d '{"number": "+14155552671"}'
```

```json
{
  "raw": "+14155552671",
  "isValid": true,
  "isPossible": true,
  "e164": "+14155552671",
  "countryCode": "US",
  "numberType": "FIXED_LINE_OR_MOBILE",
  "formatInternational": "+1 415-555-2671",
  "formatNational": "(415) 555-2671"
}
```

### Example — Bulk Upload

```bash
curl -X POST http://localhost:4000/api/v1/jobs/bulk \
  -F "file=@./phone-numbers.csv"
```

```json
{ "jobId": "clx9abc123", "totalCount": 5000 }
```

### Expected CSV Format

```csv
phone,name,email
+14155552671,Alice,alice@example.com
+919876543210,Bob,bob@example.com
+447911123456,Charlie,charlie@example.com
```

> Column name can be `phone`, `number`, `mobile`, or `telephone` — detected automatically.

<br />

## 🌐 Environment Variables

### Backend — `apps/api/.env`

```env
# PostgreSQL — Neon.tech free tier
DATABASE_URL="postgresql://user:pass@ep-xxx.neon.tech/numberverify?sslmode=require"

# Redis — Upstash free tier
REDIS_HOST="your-host.upstash.io"
REDIS_PORT=6379
REDIS_PASSWORD="your-upstash-token"
REDIS_TLS=true

# Application
PORT=4000
FRONTEND_URL=http://localhost:3000
JWT_SECRET=change-this-to-a-long-random-secret

# Twilio — leave blank for MVP, enable post-MVP only
TWILIO_ENABLED=false
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
```

### Frontend — `apps/web/.env.local`

```env
NEXT_PUBLIC_API_URL=http://localhost:4000/api/v1
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=change-this-to-a-long-random-secret
```

<br />

## 🆓 Free Deployment Guide

NumberVerify is engineered to run on a **$0/month infrastructure** using free tiers:

| Layer | Provider | Free Limit |
|---|---|---|
| Frontend | [Vercel](https://vercel.com) | Unlimited hobby projects |
| Backend | [Railway](https://railway.app) | $5 free credit / month |
| Database | [Neon.tech](https://neon.tech) | 0.5 GB storage |
| Redis | [Upstash](https://upstash.com) | 10,000 commands / day |

```bash
# 1 ─ Push to GitHub

# 2 ─ Provision free services
#     neon.tech     → create project → copy DATABASE_URL
#     upstash.com   → create Redis   → copy REDIS_HOST + REDIS_PASSWORD

# 3 ─ Deploy API on Railway
#     Root dir : apps/api
#     Build cmd: npm run build
#     Start cmd: npx prisma migrate deploy && node dist/main
#     Add all env vars in Railway dashboard

# 4 ─ Deploy Frontend on Vercel
#     Root dir : apps/web
#     Set NEXT_PUBLIC_API_URL to your Railway URL
#     Set NEXTAUTH_URL to your Vercel URL
```

<br />

## 🗺️ Roadmap

```
v1.0  MVP                           ✅  Now
  ├── Single number validation
  ├── Bulk CSV upload & queue
  ├── Real-time SSE progress
  ├── CSV export
  └── Basic auth (register / login)

v1.1  Core Polish                   🔄  In Progress
  ├── Job history & dashboard
  ├── Pagination on large result sets
  └── Rate limiting & input sanitization

v2.0  Full Scan                     🔒  Planned
  ├── Twilio Lookup (carrier + line type)
  ├── HLR live reachability check
  └── Ported number detection

v2.5  Platform                      🔒  Future
  ├── API key management
  ├── Webhooks on job complete
  └── Analytics & usage dashboard
```

<br />

## 🧪 Testing

```bash
# Unit tests
cd apps/api && npm run test

# E2E tests
cd apps/api && npm run test:e2e

# Coverage report
cd apps/api && npm run test:cov
```

**Quick smoke test with sample CSV:**

```bash
cat > smoke-test.csv << EOF
phone,name
+14155552671,Alice US
+919876543210,Bob India
+447911123456,Charlie UK
not-a-number,Invalid Entry
EOF

curl -X POST http://localhost:4000/api/v1/jobs/bulk -F "file=@smoke-test.csv"
# → Returns { "jobId": "...", "totalCount": 4 }
```

<br />

## 🤝 Contributing

Contributions make open source great — and any PR you open is genuinely appreciated.

1. Fork the repo
2. Create your branch: `git checkout -b feature/your-feature-name`
3. Commit with convention: `git commit -m 'feat: add your feature'`
4. Push: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting.

<br />

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for full text.

<br />

## 🙏 Acknowledgements

- [google-libphonenumber](https://github.com/googlei18n/libphonenumber) — the free offline engine powering every validation
- [NestJS](https://nestjs.com) — elegant, modular Node.js framework
- [Next.js](https://nextjs.org) — the React framework for production
- [BullMQ](https://docs.bullmq.io) — reliable, Redis-backed job queue
- [Prisma](https://prisma.io) — next-gen TypeScript ORM
- [shadcn/ui](https://ui.shadcn.com) — beautifully designed accessible components
- [Neon](https://neon.tech) — serverless PostgreSQL with a generous free tier
- [Upstash](https://upstash.com) — serverless Redis, perfect for queues

<br />

---

<div align="center">

<img src="./public/logo.png" alt="NumberVerify" width="160" />

<br />

**Built with ❤️ for developers who care about data quality.**

<br />

*If NumberVerify saved you time or money, a ⭐ on GitHub means the world.*

[![GitHub Stars](https://img.shields.io/github/stars/XONight/NumberVerify?style=for-the-badge&logo=github&color=00d4ff&labelColor=0d1117)](../../stargazers)
<br />

</div>