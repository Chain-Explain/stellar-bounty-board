# 💰 Stellar Bounty Board

An open-source Web3 bounty and grant board that allows projects to post development tasks and reward contributors for completed work.

This platform is designed to support open source ecosystems by connecting maintainers and contributors through structured bounty issues and transparent reward tracking.

It is built for Stellar-focused projects but designed to be extensible to other ecosystems.

---

# 🎯 Goals

* Enable maintainers to post bounty tasks
* Allow contributors to submit work proofs
* Track bounty status and rewards
* Support grant campaigns
* Provide contributor reputation signals
* Enable ecosystem-wide open contribution programs

---

# 👥 Who This Is For

* Open source maintainers
* Web3 startup teams
* Grant programs
* Protocol ecosystems
* Hackathon organizers
* Contributor communities

---

# 🧱 Core Features (MVP)

## 📌 Project Campaigns

Projects can create bounty campaigns and attach repositories.

## 🧩 Bounty Issues

Maintainers can post bounty tasks with:

* description
* reward amount
* difficulty
* tags
* deadline

## 🙋 Contributor Submissions

Contributors submit:

* PR link
* commit proof
* description of work

## ✅ Maintainer Review Flow

Maintainers can:

* approve
* reject
* request changes

## 🏆 Reward Tracking

Rewards tracked per:

* contributor
* project
* campaign

(Payout integration comes later.)

---

# ⚙️ Stack (Planned)

Backend:

* NestJS
* PostgreSQL
* TypeORM

Frontend:

* Next.js

Web3:

* Stellar wallet linking (later phase)

---

# 🧭 Architecture (High Level)

```
Projects
  ↓
Campaigns
  ↓
Bounties
  ↓
Submissions
  ↓
Reviews
  ↓
Rewards
```

---

# 🔐 Web3 Extensions (Later Phases)

* Wallet linking
* On-chain reward proofs
* DAO bounty approval voting
* On-chain payout tracking
* Reputation passport integration

---

# 🧪 Contribution Model

The project is modular and issue-driven:

* bounty module
* submission module
* review module
* reward module
* campaign module

Each module can be built independently.

---

# 🗺 Roadmap

## v1 — Core Bounty Board

* project + campaign + bounty CRUD
* submission workflow
* maintainer review
* reward tracking (off-chain)

## v2 — Web3 Integration

* wallet linking
* on-chain proof
* payout hooks

## v3 — DAO Mode

* community review voting
* reputation scoring

---

# 📜 License

MIT — open ecosystem tooling.

