# Contributing to Stellar Bounty Board

First off — thank you for being here. 🙏

Stellar Bounty Board is an open-source project built by and for the Stellar ecosystem. Every contribution — whether it's a bug fix, a new feature, a test, or a documentation improvement — moves this project forward and earns you real USDC rewards through the **Stellar Wave Program on Drips Network**.

This guide will walk you through everything you need to know to make your first (or fiftieth) contribution.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Who Can Contribute?](#who-can-contribute)
- [Drips Wave — How Rewards Work](#drips-wave--how-rewards-work)
- [Finding an Issue to Work On](#finding-an-issue-to-work-on)
- [Setting Up Your Development Environment](#setting-up-your-development-environment)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Commit Message Format](#commit-message-format)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Testing Requirements](#testing-requirements)
- [Documentation Guidelines](#documentation-guidelines)
- [Review Process](#review-process)
- [Getting Help](#getting-help)

---

## Code of Conduct

This project is welcoming to contributors from all backgrounds, experience levels, and locations — particularly from Africa, Southeast Asia, and LATAM where the Stellar ecosystem is growing fast. We expect all participants to be respectful, constructive, and patient with one another.

Harassment, discrimination, or bad-faith behaviour of any kind will not be tolerated and may result in removal from the program.

---

## Who Can Contribute?

Anyone. Seriously.

- You don't need to be a Web3 expert — most of v1 is standard TypeScript backend and frontend work
- You don't need prior Stellar experience for Phase 1 issues
- You don't need to be a senior engineer — Trivial and Medium issues are explicitly designed for early-career contributors
- You do need a GitHub account and basic familiarity with Git

If you can write TypeScript and you care about open-source tooling, you belong here.

---

## Setting Up Your Development Environment

### Prerequisites

| Tool | Version |
|------|---------|
| Node.js | >= 20.x |
| npm | >= 10.x |
| PostgreSQL | >= 15.x |
| Git | latest |

### Fork and Clone

```bash
# 1. Fork the repo on GitHub (top-right "Fork" button)
# 2. Clone your fork locally
git clone https://github.com/YOUR_GITHUB_USERNAME/stellar-bounty-board.git
cd stellar-bounty-board

# 3. Add the upstream remote
git remote add upstream https://github.com/Chain-Explain/stellar-bounty-board.git
```

### Install Dependencies

```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### Configure Environment

```bash
# Backend
cd backend
cp .env.example .env
# Fill in your local PostgreSQL credentials in .env

# Frontend
cd ../frontend
cp .env.local.example .env.local
```

### Set Up the Database

```bash
psql -U postgres -c "CREATE DATABASE stellar_bounty_board;"

cd backend
npm run migration:run
```

### Start the App

```bash
# Terminal 1 — Backend
cd backend
npm run start:dev
# API: http://localhost:3001/api/v1
# Swagger: http://localhost:3001/api/docs

# Terminal 2 — Frontend
cd frontend
npm run dev
# App: http://localhost:3000
```

---

## Development Workflow

Always work from a fresh branch based on the latest `main`:

```bash
# Keep your fork up to date before starting
git checkout main
git pull upstream main

# Create your feature branch
git checkout -b feat/bounty-crud
```

### Branch Naming Convention

```
feat/short-description        # new feature
fix/short-description         # bug fix
docs/short-description        # documentation only
test/short-description        # tests only
chore/short-description       # config, tooling, deps
refactor/short-description    # code restructure, no behaviour change
```

Examples:
```
feat/bounty-status-state-machine
fix/submission-duplicate-check
docs/swagger-bounty-controller
test/review-service-unit
chore/add-eslint-config
```

---

## Coding Standards

This project uses **TypeScript strict mode** everywhere. PRs that disable strict checks or introduce `any` types will be asked to revise.

### Backend (NestJS)

- Always use **dependency injection** — no `new ServiceClass()` outside of modules
- All request bodies must use **DTOs** with `class-validator` decorators
- All endpoints must be protected by the appropriate **guard** (`JwtAuthGuard` etc.)
- Use **TypeORM migrations** for all schema changes — never `synchronize: true`
- Define all entity **relations explicitly** in the TypeORM entity decorators
- Use NestJS **exception filters** for error handling — never return raw DB errors or expose stack traces
- Version all routes under `/api/v1/`
- Update `.env.example` whenever you add a new environment variable

```typescript
// ✅ Correct — DTO with validation
export class CreateBountyDto {
  @IsString()
  @IsNotEmpty()
  @MaxLength(120)
  title: string;

  @IsEnum(BountyDifficulty)
  difficulty: BountyDifficulty;

  @IsNumber()
  @Min(0)
  rewardAmount: number;
}

// ❌ Wrong — no validation, raw type
export class CreateBountyDto {
  title: any;
  difficulty: string;
  rewardAmount: number;
}
```

### Frontend (Next.js)

- Use **App Router** — no Pages Router patterns
- Prefer **server components** unless interactivity requires a client component
- Use **TanStack Query** for all client-side data fetching — no raw `fetch` in components
- No hardcoded API URLs — always use the `NEXT_PUBLIC_API_URL` environment variable
- Keep components small and focused — split early, not late

### General

- No commented-out code in PRs
- No `console.log` left in production code (use NestJS `Logger` in the backend)
- All new files must include TypeScript types — no implicit `any`
- Prettier and ESLint must pass before opening a PR

---

## Commit Message Format

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```

### Types

| Type | When to Use |
|------|------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `chore` | Build process, dependencies, config |
| `refactor` | Code change that is not a fix or feature |
| `style` | Formatting only (no logic change) |

### Scope = the module name

```
feat(bounty): add status transition validation
fix(submission): prevent duplicate submission per bounty
docs(auth): add swagger annotations to auth controller
test(review): add unit tests for ReviewService
chore(config): add eslint rule for no-explicit-any
```

### Rules

- Use lowercase throughout
- Keep the description under 72 characters
- Use the imperative mood: "add" not "added", "fix" not "fixed"
- Reference the issue number in the footer: `Closes #12`

---

## Pull Request Guidelines

### Before Opening a PR

- [ ] Your branch is up to date with `upstream/main`
- [ ] All acceptance criteria from the issue are met
- [ ] ESLint and Prettier pass (`npm run lint`)
- [ ] Tests pass (`npm run test`)
- [ ] You have written tests for any new service logic
- [ ] You have added/updated Swagger annotations if you touched a controller
- [ ] `.env.example` is updated if you added new env variables

### PR Title Format

Same as commit messages:

```
feat(bounty): implement bounty CRUD with status state machine
```

### PR Description Template

When you open a PR, fill in the following:

```markdown
## What does this PR do?
A short description of the change.

## Related Issue
Closes #<issue number>

## Type of change
- [ ] New feature
- [ ] Bug fix
- [ ] Documentation
- [ ] Tests
- [ ] Chore / config

## Acceptance Criteria Checklist
(Copy the checklist from the issue and tick each item)

## Testing
Describe how you tested this change locally.

## Screenshots (if frontend)
```

### PR Rules

- One PR per issue — do not bundle unrelated changes
- Keep PRs focused and small where possible — large PRs take longer to review and may miss the Wave deadline
- Do not force-push to your branch after a review has started
- Respond to review comments within **24 hours** during an active Wave sprint
- If you are blocked or cannot complete within the sprint, comment on the issue immediately so it can be reassigned

---

## Testing Requirements

All service-layer code must have unit tests. PRs that add a new service without tests will not be merged.

### Unit Tests

- Location: `backend/src/<module>/<module>.service.spec.ts`
- Framework: Jest
- Mock all external dependencies (TypeORM repositories, other services)
- Test the happy path and at least one error/edge case per method

```typescript
describe('BountyService', () => {
  describe('create', () => {
    it('should create a bounty and return it', async () => { ... });
    it('should throw if campaign does not exist', async () => { ... });
  });
});
```

### E2E Tests

- Location: `backend/test/<flow>.e2e-spec.ts`
- Required for: submission approval flow, reward creation trigger
- Use a real test database — set `DATABASE_NAME=stellar_bounty_board_test` in test config

### Running Tests

```bash
cd backend

# Unit tests
npm run test

# Unit tests with coverage
npm run test:cov

# E2E tests
npm run test:e2e
```

Coverage threshold: **80% minimum** for any service file you introduce.

---

## Documentation Guidelines

Documentation issues are labelled `Trivial` or `Medium` and are a great entry point for contributors new to the codebase.

### Swagger Annotations

Every controller endpoint must have:

```typescript
@ApiOperation({ summary: 'Create a new bounty' })
@ApiResponse({ status: 201, description: 'Bounty created successfully', type: BountyResponseDto })
@ApiResponse({ status: 400, description: 'Validation error' })
@ApiResponse({ status: 401, description: 'Unauthorized' })
@ApiBearerAuth()
```

### JSDoc on Services

All public service methods should have a JSDoc comment:

```typescript
/**
 * Creates a new bounty under the given campaign.
 * Throws NotFoundException if the campaign does not exist.
 */
async create(dto: CreateBountyDto, ownerId: string): Promise<Bounty> { ... }
```

### README Updates

If your PR changes how the project is set up, run, or configured — update the relevant section of `README.md` in the same PR. Do not leave the README out of sync with the code.

---

## Review Process

The maintainer (**TomikeDS**) reviews all PRs. Here is what to expect:

| Stage | Typical Turnaround |
|-------|--------------------|
| First review after PR opened | Within 24–48 hours |
| Follow-up review after changes | Within 24 hours |
| Merge after approval | Same day |

During an active **Wave sprint**, reviews are prioritised and turnaround is faster. If you have not received a review within 48 hours during a sprint, ping in the issue comments.

### What reviewers look for

- Does the code meet the acceptance criteria exactly?
- Does it follow the coding standards in this guide?
- Are there tests, and do they cover meaningful cases?
- Is the PR scoped appropriately (no unrelated changes)?
- Does the Swagger documentation reflect the implementation?

Reviews are constructive. If changes are requested, they come with clear explanations. The goal is a merged PR, not a failed one.

---

## Getting Help

- **Question about an issue?** Comment directly on the GitHub issue
- **Stuck on setup?** Open a [GitHub Discussion](https://github.com/Chain-Explain/stellar-bounty-board/discussions)
- **Found a bug not covered by an issue?** Open a new issue using the bug report template
- **General questions about the Wave Program?** See the [Drips Wave docs](https://docs.drips.network/wave)

---

<div align="center">

Built with ❤️ by <a href="https://github.com/TomikeDS">TomikeDS</a> · <a href="https://github.com/Chain-Explain">Chain-Explain</a> · MIT Licensed

</div>
