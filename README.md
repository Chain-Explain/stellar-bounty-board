# 🌟 Stellar Bounty Board

> An open-source Web3 bounty and grant board connecting project maintainers with contributors through structured bounty issues and transparent reward tracking — built for the Stellar ecosystem.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Stellar Wave](https://img.shields.io/badge/Stellar%20Wave-Program-blue)](https://www.drips.network/wave/stellar)
[![NestJS](https://img.shields.io/badge/NestJS-v10-red)](https://nestjs.com/)
[![Next.js](https://img.shields.io/badge/Next.js-v14-black)](https://nextjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-v15-blue)](https://www.postgresql.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue)](https://www.typescriptlang.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Why Stellar Bounty Board?](#-why-stellar-bounty-board)
- [Live Demo](#-live-demo)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Data Model](#-data-model)
- [Features](#-features)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Running the App](#running-the-app)
- [API Documentation](#-api-documentation)
- [Project Structure](#-project-structure)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Drips Wave Program](#-drips-wave-program)
- [License](#-license)
- [Maintainer](#-maintainer)

---

## 🌐 Overview

**Stellar Bounty Board** is an open-source platform that brings structured, transparent bounty workflows to the Stellar ecosystem. It allows open-source project maintainers to post funded bounties on their issues, and enables contributors worldwide to discover, apply for, and get rewarded for meaningful work.

The platform mirrors the issue lifecycle of platforms like GitHub — but adds a reward and reputation layer on top, with deep integration with the [Stellar blockchain](https://stellar.org) for on-chain payouts (Phase 2) and Soroban smart contracts for verifiable proof-of-work (Phase 3).

This project is part of the **Stellar Wave Program on Drips Network** — a monthly sprint-based funding cycle where contributors earn USDC rewards for merged pull requests.

---

## 💡 Why Stellar Bounty Board?

The open-source ecosystem has a well-known "maintenance gap": projects are under-resourced, maintainers are overwhelmed, and new contributors lack clear entry points.

Stellar Bounty Board addresses this by:

- Giving **maintainers** a structured way to scope, fund, and track bounty work without administrative overhead
- Giving **contributors** a transparent board to discover paid tasks with clear acceptance criteria and difficulty levels
- Giving **ecosystems** (like Stellar) a reusable, extensible coordination layer for contributor programs
- Building toward **on-chain accountability** — where every reward payout is verifiable on the Stellar network

---

## 🚀 Live Demo

> Coming soon — first deployment will be announced via the project's GitHub Discussions.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Next.js Frontend                         │
│              (App Router · Server Components · TanStack Query)  │
└──────────────────────────┬──────────────────────────────────────┘
                           │ HTTP / REST
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                       NestJS Backend                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐ │
│  │  Project │ │ Campaign │ │  Bounty  │ │    Submission      │ │
│  │  Module  │ │  Module  │ │  Module  │ │      Module        │ │
│  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐ │
│  │  Review  │ │  Reward  │ │   Auth   │ │  User/Reputation   │ │
│  │  Module  │ │  Module  │ │  Module  │ │      Module        │ │
│  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘ │
└──────────────────────────┬──────────────────────────────────────┘
                           │ TypeORM
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                       PostgreSQL Database                        │
└─────────────────────────────────────────────────────────────────┘

                  Phase 2 & 3 (Upcoming)
┌─────────────────────────────────────────────────────────────────┐
│              Stellar Network / Soroban Smart Contracts           │
│         (Wallet Auth · On-chain Proof · USDC Payouts)           │
└─────────────────────────────────────────────────────────────────┘
```

**Domain hierarchy:**

```
Project
  └── Campaign (funding round / sprint)
        └── Bounty (individual task with reward)
              └── Submission (contributor PR / work proof)
                    └── Review (maintainer decision)
                          └── Reward (payout record)
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend Framework** | [NestJS](https://nestjs.com/) v10 (TypeScript, strict mode) |
| **Frontend Framework** | [Next.js](https://nextjs.org/) v14 (App Router) |
| **Database** | [PostgreSQL](https://www.postgresql.org/) v15 |
| **ORM** | [TypeORM](https://typeorm.io/) with migrations |
| **Authentication** | JWT (Phase 1) · Stellar SEP-10 wallet signature (Phase 2) |
| **Client Data Fetching** | [TanStack Query](https://tanstack.com/query) |
| **API Style** | RESTful — versioned under `/api/v1/` |
| **API Docs** | OpenAPI 3.0 / Swagger UI |
| **Validation** | `class-validator` + `class-transformer` DTOs |
| **Testing** | Jest (unit) · Supertest (e2e) |
| **Web3 (Phase 2+)** | [Stellar SDK](https://stellar.github.io/js-stellar-sdk/) · [Soroban](https://soroban.stellar.org/) |

---

## 📐 Data Model

```
┌─────────────┐       ┌──────────────┐       ┌──────────────┐
│   User      │       │   Project    │       │   Campaign   │
│─────────────│       │──────────────│       │──────────────│
│ id          │◄──┐   │ id           │◄──┐   │ id           │
│ githubHandle│   │   │ name         │   │   │ title        │
│ walletAddr  │   │   │ description  │   │   │ budget       │
│ reputation  │   │   │ repoUrl      │   │   │ startDate    │
│ createdAt   │   │   │ logoUrl      │   │   │ endDate      │
└─────────────┘   │   │ owner FK     │───┘   │ project FK   │───┐
                  │   └──────────────┘       └──────────────┘   │
                  │                                               │
                  │   ┌──────────────┐                           │
                  │   │   Bounty     │◄──────────────────────────┘
                  │   │──────────────│
                  │   │ id           │
                  │   │ title        │
                  │   │ description  │
                  │   │ rewardAmount │
                  │   │ difficulty   │  (trivial/medium/high)
                  │   │ tags         │
                  │   │ deadline     │
                  │   │ status       │  (open/assigned/completed/cancelled)
                  │   │ campaign FK  │
                  │   └──────┬───────┘
                  │          │
                  │   ┌──────▼───────┐       ┌──────────────┐
                  │   │  Submission  │       │   Review     │
                  │   │──────────────│       │──────────────│
                  └───│ contributor  │       │ id           │
                      │ prLink       │◄──┐   │ decision     │
                      │ commitProof  │   │   │ feedback     │
                      │ description  │   │   │ reviewer FK  │
                      │ status       │   └───│ submission FK│
                      │ bounty FK    │       └──────────────┘
                      └──────┬───────┘
                             │
                      ┌──────▼───────┐
                      │   Reward     │
                      │──────────────│
                      │ id           │
                      │ amount       │
                      │ currency     │
                      │ status       │  (pending/paid)
                      │ contributor  │
                      │ bounty FK    │
                      └──────────────┘
```

### Entity Descriptions

| Entity | Description |
|--------|-------------|
| `Project` | A maintainer's open-source project. Has a name, description, GitHub repo URL, and logo. |
| `Campaign` | A funding round or sprint within a project. Groups multiple bounties under a budget and time window. |
| `Bounty` | A single task with a defined reward, difficulty, deadline, and lifecycle status. |
| `Submission` | A contributor's work submission for a bounty — includes PR link and commit proof. |
| `Review` | A maintainer's decision on a submission: approved, rejected, or changes requested. |
| `Reward` | The payout record tied to an approved submission. Tracks currency, amount, and payment status. |
| `User` | Any platform participant. Stores GitHub handle, optional Stellar wallet address, and reputation score. |

---

## ✨ Features

### Phase 1 — Core Bounty Board (Current)

- **Project Management** — Create and manage open-source projects with metadata
- **Campaign System** — Group bounties into funded campaigns with budgets and timelines
- **Bounty CRUD** — Post, update, and cancel bounties with difficulty labels and deadlines
- **Submission Workflow** — Contributors submit PR links and commit proofs for review
- **Review System** — Maintainers approve, reject, or request changes on submissions
- **Reward Tracking** — Off-chain tracking of reward amounts and payout status
- **JWT Authentication** — Secure API access for maintainers and contributors
- **Reputation Scoring** — Basic contributor reputation tracking based on approved submissions
- **OpenAPI / Swagger Docs** — Full API documentation at `/api/docs`

### Phase 2 — Stellar Integration (Upcoming)

- Stellar wallet linking via SEP-10 signature authentication
- USDC reward payouts directly to contributor Stellar wallets
- On-chain proof of contribution via Soroban contracts
- Payout hooks triggered on review approval

### Phase 3 — DAO & Reputation Passport (Future)

- DAO voting on bounty disputes
- Reputation passport — portable contributor profile
- Soroban contract-based escrow for bounty funds
- Deep integration with Drips Wave on-chain reward distribution

---

## 🚦 Getting Started

### Prerequisites

Ensure you have the following installed:

| Tool | Version |
|------|---------|
| Node.js | >= 20.x |
| npm | >= 10.x |
| PostgreSQL | >= 15.x |
| Git | latest |

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/YOUR_ORG/stellar-bounty-board.git
cd stellar-bounty-board
```

**2. Install backend dependencies**

```bash
cd backend
npm install
```

**3. Install frontend dependencies**

```bash
cd ../frontend
npm install
```

### Environment Variables

**Backend** — copy and fill in `.env.example`:

```bash
cd backend
cp .env.example .env
```

```env
# ── Database ──────────────────────────────────────────────
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=stellar_bounty_board
DATABASE_USER=postgres
DATABASE_PASSWORD=yourpassword

# ── Auth ──────────────────────────────────────────────────
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRES_IN=7d

# ── App ───────────────────────────────────────────────────
PORT=3001
NODE_ENV=development
API_PREFIX=api/v1

# ── Stellar (Phase 2) ─────────────────────────────────────
# STELLAR_NETWORK=testnet
# STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org
```

**Frontend** — copy and fill in `.env.local.example`:

```bash
cd frontend
cp .env.local.example .env.local
```

```env
NEXT_PUBLIC_API_URL=http://localhost:3001/api/v1
```

### Running the App

**1. Create the database**

```bash
psql -U postgres -c "CREATE DATABASE stellar_bounty_board;"
```

**2. Run database migrations**

```bash
cd backend
npm run migration:run
```

**3. Start the backend**

```bash
# Development (with hot reload)
npm run start:dev

# Production
npm run build && npm run start:prod
```

The API will be available at `http://localhost:3001/api/v1`
Swagger docs at `http://localhost:3001/api/docs`

**4. Start the frontend**

```bash
cd frontend
npm run dev
```

The app will be available at `http://localhost:3000`

---

**Run everything with Docker (optional)**

```bash
# From project root
docker-compose up --build
```

---

## 📚 API Documentation

Full interactive API documentation is available via Swagger UI at:

```
http://localhost:3001/api/docs
```

### Core Endpoints (v1)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/auth/register` | Register a new user |
| `POST` | `/api/v1/auth/login` | Login and receive JWT |
| `GET` | `/api/v1/projects` | List all projects |
| `POST` | `/api/v1/projects` | Create a project |
| `GET` | `/api/v1/projects/:id` | Get project details |
| `GET` | `/api/v1/projects/:id/campaigns` | List campaigns for a project |
| `POST` | `/api/v1/campaigns` | Create a campaign |
| `GET` | `/api/v1/bounties` | List all open bounties |
| `POST` | `/api/v1/bounties` | Create a bounty |
| `GET` | `/api/v1/bounties/:id` | Get bounty details |
| `PATCH` | `/api/v1/bounties/:id` | Update a bounty |
| `POST` | `/api/v1/submissions` | Submit work for a bounty |
| `GET` | `/api/v1/submissions/:id` | Get submission details |
| `POST` | `/api/v1/reviews` | Submit a review decision |
| `GET` | `/api/v1/rewards` | List rewards |
| `GET` | `/api/v1/users/:id` | Get user profile & reputation |

All protected endpoints require the `Authorization: Bearer <token>` header.

---

## 📁 Project Structure

```
stellar-bounty-board/
├── backend/                        # NestJS API
│   ├── src/
│   │   ├── auth/                   # JWT auth module
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.module.ts
│   │   │   ├── guards/
│   │   │   │   └── jwt-auth.guard.ts
│   │   │   └── strategies/
│   │   │       └── jwt.strategy.ts
│   │   ├── bounty/                 # Bounty module
│   │   │   ├── bounty.controller.ts
│   │   │   ├── bounty.service.ts
│   │   │   ├── bounty.module.ts
│   │   │   ├── entities/
│   │   │   │   └── bounty.entity.ts
│   │   │   └── dto/
│   │   │       ├── create-bounty.dto.ts
│   │   │       └── update-bounty.dto.ts
│   │   ├── campaign/               # Campaign module
│   │   ├── project/                # Project module
│   │   ├── submission/             # Submission module
│   │   ├── review/                 # Review module
│   │   ├── reward/                 # Reward module
│   │   ├── user/                   # User + reputation module
│   │   ├── common/
│   │   │   ├── filters/            # Global exception filters
│   │   │   ├── interceptors/       # Response shaping
│   │   │   ├── decorators/         # Custom decorators
│   │   │   └── pipes/              # Validation pipes
│   │   ├── config/                 # @nestjs/config setup
│   │   ├── database/
│   │   │   └── migrations/         # TypeORM migrations
│   │   └── main.ts
│   ├── test/                       # e2e tests
│   ├── .env.example
│   ├── tsconfig.json
│   └── package.json
│
├── frontend/                       # Next.js App
│   ├── app/
│   │   ├── (auth)/                 # Auth routes
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── dashboard/              # Maintainer dashboard
│   │   ├── bounties/               # Public bounty board
│   │   │   ├── page.tsx            # Bounty listing
│   │   │   └── [id]/
│   │   │       └── page.tsx        # Bounty detail
│   │   ├── projects/               # Project pages
│   │   ├── submissions/            # Submission pages
│   │   └── layout.tsx
│   ├── components/                 # Shared UI components
│   │   ├── bounty/
│   │   ├── campaign/
│   │   ├── project/
│   │   └── ui/                     # Base UI primitives
│   ├── lib/
│   │   ├── api/                    # API client functions
│   │   ├── hooks/                  # TanStack Query hooks
│   │   └── utils/
│   ├── public/
│   ├── .env.local.example
│   └── package.json
│
├── docker-compose.yml
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bounty-trivial.md
│   │   ├── bounty-medium.md
│   │   └── bounty-high.md
│   └── workflows/
│       └── ci.yml
├── CONTRIBUTING.md
└── README.md
```

---

## 🗺️ Roadmap

### v1 — Core Bounty Board *(In Progress)*

- [x] Project scaffolding (NestJS + Next.js + PostgreSQL)
- [ ] Auth module — JWT registration, login, guards
- [ ] User module — profile, reputation score
- [ ] Project module — CRUD, ownership
- [ ] Campaign module — CRUD, budget tracking
- [ ] Bounty module — CRUD, status state machine, difficulty labels
- [ ] Submission module — workflow, status transitions
- [ ] Review module — maintainer decision flow
- [ ] Reward module — off-chain tracking, payment status
- [ ] OpenAPI / Swagger documentation
- [ ] Frontend — bounty board listing page
- [ ] Frontend — bounty detail + submission form
- [ ] Frontend — maintainer dashboard
- [ ] Unit tests (services)
- [ ] e2e tests (critical flows)
- [ ] Docker Compose setup
- [ ] CI/CD via GitHub Actions

### v2 — Stellar Integration *(Planned)*

- [ ] Stellar SEP-10 wallet signature authentication
- [ ] Wallet linking on user profile
- [ ] USDC reward payout via Stellar Horizon
- [ ] Soroban on-chain proof of contribution
- [ ] Payout hooks on review approval

### v3 — DAO & Ecosystem *(Future)*

- [ ] DAO voting on bounty disputes
- [ ] Reputation passport (portable contributor profile)
- [ ] Soroban escrow contracts for bounty funds
- [ ] Deep Drips Wave integration for on-chain cycle rewards
- [ ] Multi-project dashboard for ecosystem managers

---

## 🤝 Contributing

We welcome contributors of all experience levels. Stellar Bounty Board is built in the open, and contributions are rewarded through the **Stellar Wave Program on Drips Network**.

### How to Contribute

1. **Browse open bounty issues** — filter by `Trivial`, `Medium`, or `High` labels
2. **Comment on the issue** to express interest (the maintainer will assign you)
3. **Fork the repo**, create a feature branch from `main`
4. **Write your code** following the coding standards below
5. **Open a Pull Request** — reference the issue number in your PR description
6. **Respond to review feedback** promptly (Wave cycles are one week!)

### Coding Standards

- TypeScript strict mode everywhere — no `any` types
- NestJS: always use dependency injection, DTOs with `class-validator`
- TypeORM: use migrations only — never `synchronize: true` in production
- Next.js: App Router preferred, server components where possible
- Always write unit tests for service methods
- Follow RESTful conventions under `/api/v1/`
- Use NestJS exception filters — never return raw database errors
- Update `.env.example` when adding new environment variables

### Branch Naming

```
feat/bounty-crud
fix/submission-status-transition
docs/contributing-guide
chore/add-jest-config
```

### Commit Message Format

```
feat(bounty): add difficulty label validation
fix(submission): correct status transition guard
docs(readme): update installation steps
```

For full contribution guidelines, see [CONTRIBUTING.md](./CONTRIBUTING.md).

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](./LICENSE) file for details.

---

## 👤 Maintain
---

<div align="center">

Built with ❤️ for the Stellar ecosystem · Powered by [Drips Wave](https://www.drips.network/wave/stellar)

</div>
