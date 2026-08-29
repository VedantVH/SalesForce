<div align="center">

<br />

<img src="https://img.shields.io/badge/ComplyLens-DPDP%20Compliance%20Platform-6366f1?style=for-the-badge&logoColor=white" alt="ComplyLens" />

<br /><br />

# ComplyLens

### A Full-Stack Compliance Operations Platform for India's Digital Personal Data Protection (DPDP) Act

**Deterministic rule engine · AI-assisted evidence briefings · Tamper-evident audit chain**

*Designed and built independently, end to end.*

<br />

[![Next.js](https://img.shields.io/badge/Next.js-15-000000?style=flat-square&logo=nextdotjs&logoColor=white)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev)
[![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?style=flat-square&logo=prisma&logoColor=white)](https://prisma.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Mistral AI](https://img.shields.io/badge/Mistral-AI%20Engine-FF7000?style=flat-square)](https://mistral.ai)
[![Vitest](https://img.shields.io/badge/Vitest-Testing-6E9F18?style=flat-square&logo=vitest&logoColor=white)](https://vitest.dev)
[![License](https://img.shields.io/badge/License-Proprietary-dc2626?style=flat-square)](#-license)

<br />

**[Quick Start](#-quick-start) · [Architecture](#-architecture) · [Features](#-features) · [Tech Stack](#-tech-stack) · [Security](#-security) · [Deployment](#-deployment)**

</div>

---

## 🎯 Why This Project Exists

India's DPDP Act imposes real regulatory obligations on any organisation handling personal data — consent tracking, breach notification timelines, retention limits, and audit-ready evidence. Most companies handle this with spreadsheets and manual reviews, which don't scale and don't hold up under audit.

**ComplyLens is a decision-support layer** that sits on top of existing systems (CRM, consent platforms, SIEM, ticketing) and turns raw evidence into rule-based compliance verdicts — with every score reproducible, every action logged, and every AI-generated explanation traceable back to the exact rule that produced it.

> 💡 **The core engineering problem this solves:** how do you let an LLM *explain* a compliance decision to a human without ever letting it *make* that decision? This repo is a working answer to that question.

I designed and built this project myself — from the architecture and deterministic rule engine, to the AI integration boundary, database schema, audit-chain implementation, and test suite.

---

## 🧠 At a Glance — For Recruiters & Reviewers

| | |
|---|---|
| 🏗️ **Type** | Full-stack SaaS platform (frontend, backend, database, AI integration) |
| 🎯 **Domain** | RegTech / Data Privacy Compliance (India's DPDP Act, 2023) |
| 🔐 **Core Challenge Solved** | Strict separation between deterministic decision logic and generative AI explanation |
| 🧩 **Engineering Highlights** | Versioned rule engine · SHA-256 hash-chained audit log with Merkle proofs · Role-based access control · Non-mutating simulation sandbox |
| 🧪 **Quality Bar** | Automated unit/integration tests, full browser smoke suite, strict TypeScript, CI-ready build pipeline |
| ⚙️ **AI Usage Pattern** | Read-only, post-decision, PII-stripped context with deterministic fallback if the AI provider is unavailable |
| 📦 **Deployability** | One-click Render Blueprint deploy + documented Vercel/managed-Postgres path |

This project demonstrates end-to-end product thinking — not just "add an LLM to an app," but designing the *guardrails* around it so the system stays legally defensible, auditable, and trustworthy even when AI is involved.

---

## 🏛 The One Rule This System Never Breaks

```
Rule Engine   ──▶  decides every verdict        deterministic · versioned · no exceptions
Mistral AI    ──▶  explains it afterward         read-only · post-pipeline · no mutations
Human DPO     ──▶  approves all remediation      nothing changes without explicit sign-off
```

This is enforced architecturally, not just by convention — the AI layer has no write path into the scoring pipeline. It can only read a verdict that has already been persisted and generate a plain-language explanation of it.

---

## 🏗 Architecture

<p align="center">
<pre>
┌─────────────────────────────────────────────┐
│              Browser / Client                │
│      Next.js 15 App Router · React 19        │
│           Recharts · TypeScript              │
└──────────────────────┬───────────────────────┘
                        │  HTTPS · HTTP-only signed session cookie
┌───────────────────────▼───────────────────────┐
│              Next.js API Routes                │
│    Auth Middleware · requireSession()          │
│         Zod body + query validation            │
└───────────────────────┬───────────────────────┘
                         │
            ┌────────────▼────────────┐
            │   Comparison available?  │  ◀── decision gate
            └──────┬────────────────┬──┘
                 Yes                No / invalid output
                   │                  │
      ┌────────────▼──────┐  ┌────────▼──────────────┐
      │ Structured        │  │ Deterministic local    │
      │ classifier        │  │ classifier             │
      └────────────┬──────┘  └────────┬───────────────┘
                    └────────┬─────────┘
                             │
   ┌─────────────────────────▼───────────────────┐
   │        Schema validation + normalisation     │
   └─────────────────────────┬───────────────────┘
                             │
   ┌─────────────────────────▼───────────────────┐
   │          Deterministic Rule Engine           │
   │   Evidence → Controls → Score → Severity     │
   │   gate → Persist verdict + legal mapping     │
   │                                               │
   │   ⚠  AI does not participate in this stage   │
   └──────────┬───────────────────┬───────────────┘
              │                   │
   ┌──────────▼──────────┐  ┌─────▼────────────────┐
   │    AI Briefing       │  │    Audit Chain        │
   │    Mistral · server   │  │    SHA-256 chained    │
   │    read-only          │  │    Merkle proofs      │
   │    no PII in context  │  │    JSON export        │
   └──────────┬────────────┘  └─────┬─────────────────┘
              └────────┬──────────────┘
                        │  Prisma ORM
   ┌────────────────────▼─────────────────────────┐
   │                 PostgreSQL                    │
   │   ComplianceAssessment · ComplianceResult      │
   │   AuditLog (append-only) · RuleVersion         │
   │   Contact · Incident · MerkleCheckpoint        │
   └────────────────────┬─────────────────────────┘
                         │  production adapters only
   ┌────────────────────▼─────────────────────────┐
   │            External Connectors                │
   │    CRM · Consent Channel · SIEM · Ticketing    │
   │     (source systems remain system of record)   │
   └─────────────────────────────────────────────┘
</pre>
</p>

<details>
<summary><b>📐 Key architectural decisions (click to expand)</b></summary>
<br />

| Concern | Decision | Why |
|---|---|---|
| Rule evaluation | Deterministic, versioned pipeline | Reproducible verdicts — AI cannot influence any stage |
| AI access | Read persisted verdicts only | Preserves DPDP auditability; cannot mutate state |
| Session auth | HTTP-only JOSE cookie · 8 h TTL · SameSite=Lax | No JS token leakage; CSRF protection |
| Audit chain | SHA-256 chained under serialised DB lock | Detectable tampering; Merkle proofs portable |
| PII in AI calls | Excluded at API layer | Data minimisation — no personal fields reach model |
| Schema writes | Append-only in application code | Immutability by convention; DB hardening is Phase 2 |

</details>

---

## ✨ Features

<details open>
<summary><b>🔢 Deterministic DPDP Assessment Engine</b></summary>
<br />

A five-stage pipeline where Mistral is involved in **zero** of the stages:

```
Raw evidence  (CRM · Consent System · SIEM)
      │
      ├─ 1. Normalise          →  5 typed inputs
      ├─ 2. Evaluate controls  →  per-control pass / fail
      ├─ 3. Score from 100     →  versioned deductions applied
      ├─ 4. Severity gate      →  block if threshold breached
      └─ 5. Persist to history →  rule version + legal mapping locked

            ▲ AI reads the persisted result here — never inside the pipeline
```

Historical verdicts are pinned to their original rule version. Retroactive re-scoring never happens.

</details>

<details>
<summary><b>🔬 Rule Trace Studio</b></summary>
<br />

A non-mutating simulation environment — safe to run against production data.

- Live evidence toggles with per-control execution traces
- Reproducible scenario fingerprint — share exact simulation state with a teammate
- Inactive extension preview — test new rules before activation

</details>

<details>
<summary><b>🛠 Human-Gated Remediation</b></summary>
<br />

- Impact analysis across the full assessed contact portfolio
- One open action per contact per failed control — no duplicate tasks
- Covers: consent · purpose · retention · notice · minimisation
- Consent flow routes the data principal to an external channel — ComplyLens records only after the verified response syncs back

</details>

<details>
<summary><b>🤖 Evidence-Grounded AI Briefings</b></summary>
<br />

- Every insight is rule-cited — no unsupported claims
- Owned actions with measurable success signals
- Deterministic fallback — if Mistral is unavailable, the platform keeps working
- No PII reaches the model — only minimised verdict metadata is sent

</details>

<details>
<summary><b>🚨 Incident Command Cockpit</b></summary>
<br />

- 144-hour internal escalation target visibility *(not a statutory deadline — see [Legal](#%EF%B8%8F-legal))*
- Notification evidence tracking
- Guarded containment and closure — prevents premature status changes
- CSV reporting · SDF operational review · tamper-evident audit verification

</details>

<details>
<summary><b>🔗 Tamper-Evident Audit Chain</b></summary>
<br />

- Every entry is canonically encoded and SHA-256 chained under a serialised DB lock
- Full chain verification in the DPO view
- Merkle checkpoints with inclusion proofs
- Portable JSON proof bundle export — take evidence anywhere

</details>

---

## 👥 User Personas

| Role | Can | Cannot |
|---|---|---|
| 🧑‍⚖️ **DPO / Privacy Lead** | Review posture · approve remediation · export audit evidence | — |
| 🧑‍💼 **CRM / Data Steward** | Investigate contact evidence · request correction | Approve own remediation · grant consent for a data principal |
| 🧑‍🚒 **Incident Response Lead** | Log breach scope · record operational milestones | Oversee notification evidence (DPO only) |

---

## 🚀 Quick Start

### Prerequisites

| Tool | Version |
|---|---|
| Node.js | 20+ |
| PostgreSQL | 14+ (local or Docker) |
| Mistral API key | [Get one →](https://mistral.ai) |

### 1 — Clone and configure

```bash
git clone https://github.com/nischala755/salesforce.git
cd salesforce
cp .env.example .env.local
```

Open `.env.local` and fill in all five values:

```env
DATABASE_URL           = postgresql://user:pass@localhost:55432/complylens
JWT_SECRET              = <random-string-minimum-32-characters>
MISTRAL_API_KEY         = <your-key>
DEMO_ADMIN_PASSWORD     = <strong-password>
DEMO_REVIEWER_PASSWORD  = <strong-password>
```

> ⚠️ Never prefix any of these with `NEXT_PUBLIC_`. Never commit `.env.local`.

### 2 — Install, migrate, seed

```bash
npm install
npm run db:migrate -- --name init
npm run db:seed
```

### 3 — Run

```bash
npm run dev
```

Open **[http://localhost:3000](http://localhost:3000)** and sign in with the [demo accounts](#-demo-accounts).

> A local PostgreSQL container `complylens-dev-db` is bound to `127.0.0.1:55432` for development. Production must supply a managed connection via `DATABASE_URL`.

---

## 🧪 Testing

```bash
npm test               # Vitest — unit + integration
npm run lint            # ESLint
npm run typecheck       # Strict TypeScript check
npm run build           # Production build verification
npm run smoke:browser   # Full browser smoke test (Chrome + running server)
```

<details>
<summary><b>✅ Full smoke test coverage (click to expand)</b></summary>
<br />

| Area | Status |
|---|---|
| Authentication — both roles | ✅ |
| Bulk assessment run | ✅ |
| Contact investigation | ✅ |
| Rule Trace Studio (non-mutating) | ✅ |
| Purpose-remediation: approve → apply → reassess | ✅ |
| Incident tracking | ✅ |
| AI explanation + deterministic fallback | ✅ |
| Integrity checkpoint creation | ✅ |
| Merkle proof export (JSON bundle) | ✅ |
| CSV download | ✅ |
| Console error detection | ✅ |
| Mobile overflow — dashboard · list · detail | ✅ |

</details>

---

## 📦 Deployment

### Environment variables

| Variable | Required | Notes |
|---|---|---|
| `DATABASE_URL` | ✅ | Managed PostgreSQL connection string |
| `JWT_SECRET` | ✅ | Minimum 32 random characters |
| `MISTRAL_API_KEY` | ✅ | Server-side only — never `NEXT_PUBLIC_` |
| `DEMO_ADMIN_PASSWORD` | ✅ | Rotate before handling real personal data |
| `DEMO_REVIEWER_PASSWORD` | ✅ | Rotate before handling real personal data |

<details>
<summary><b>☁️ Deploy to Render — Free Tier (click to expand)</b></summary>
<br />

`render.yaml` is included and auto-generates `JWT_SECRET`. Render's free tier does not support pre-deploy commands, so migrations run as part of the build.

**Steps:**

1. **New → Blueprint** → connect this repository
2. Confirm Render detects `render.yaml`
3. Enter `MISTRAL_API_KEY`, `DEMO_ADMIN_PASSWORD`, `DEMO_REVIEWER_PASSWORD` when prompted
4. Create Blueprint — wait for: migration → bootstrap → build → health check
5. Open `https://complylens.onrender.com/login`

**Manual build command:**
```bash
npm ci && npm run db:deploy && npm run db:seed && npm run build
```

**Start command:**
```bash
npm start
```

> On a paid Render plan, move `npm run db:deploy` to the pre-deploy command.

</details>

<details>
<summary><b>▲ Deploy to Vercel + Managed Postgres (click to expand)</b></summary>
<br />

```bash
# 1. Create a managed PostgreSQL database — copy the pooled connection string
# 2. Import this repo in Vercel as a Next.js project
# 3. Add all 5 env vars to the Production environment

# 4. Run migrations once — never use prisma migrate dev in production
npm run db:deploy
npm run db:seed

# 5. Deploy from main and verify:
#    /login · assessment run · remediation approval · /api/audit/integrity
```

Vercel redeploys automatically on every push to `main`. Always run production migrations as an explicit CI/CD step before promoting schema-changing releases.

</details>

---

## 🔐 Security

<details>
<summary><b>Security controls (click to expand)</b></summary>
<br />

| Control | Status |
|---|---|
| `requireSession()` + middleware outer guard on every route | ✅ Active |
| HTTP-only · SameSite=Lax · JOSE-signed · 8 h TTL · Secure in prod | ✅ Active |
| Zod validation on all incoming bodies and queries | ✅ Active |
| Mistral server-side only — zero API key in browser bundle | ✅ Active |
| Personal fields excluded from all model inputs | ✅ Active |
| SHA-256 audit chain + Merkle checkpoints | ✅ Active |
| DB-level immutability (triggers · restricted roles · WORM) | 🔲 Phase 2 |
| External root anchoring for the audit chain | 🔲 Phase 2 |
| LLM data-residency + cross-border transfer review | 🔲 Phase 2 |

> The hash chain makes tampering **detectable** — it does not prevent a sufficiently privileged database operator from rewriting data and recomputing hashes. External root anchoring is a Phase 2 hardening item.

</details>

---

## 🔑 Demo Accounts

| Role | Email | Password source |
|---|---|---|
| Administrator | `admin@complylens.demo` | `DEMO_ADMIN_PASSWORD` env var |
| DPO Reviewer | `reviewer@complylens.demo` | `DEMO_REVIEWER_PASSWORD` env var |

The seed script refreshes both password hashes from env vars on every run. Operational records are never overwritten on re-seed. **Remove or rotate both accounts before handling real personal data.**

---

## 🗺 Roadmap

**Phase 2 — Hardening**

- [ ] DB-level immutability — triggers, restricted roles, or WORM storage
- [ ] External root anchoring for the SHA-256 audit chain
- [ ] LLM data-residency review — vendor terms, transfer basis, retention config, region selection
- [ ] Production connector adapters — CRM, consent management, SIEM, ticketing

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router) |
| UI | React 19 |
| Language | TypeScript — strict mode |
| ORM | Prisma |
| Database | PostgreSQL |
| AI Engine | Mistral (server-side only) |
| Charts | Recharts |
| Auth | JOSE-signed HTTP-only cookies |
| Validation | Zod |
| Testing | Vitest + browser smoke tests |

---

## ⚖️ Legal

| Item | Clarification |
|---|---|
| Penalty figures | Illustrative operational context only — not a prediction of actual liability |
| SDF mode | An operational setting — does not legally determine SDF classification (depends on government notification) |
| 144-hour timer | An internal escalation target, not a statutory deadline |
| This software | An operational aid, not legal certification or legal advice |

**Have qualified legal counsel validate all rule mappings and deployment controls before using this system with real personal data.**

---

## 📄 License

Proprietary — all rights reserved.

---

<div align="center">
<sub>Built solo with Next.js 15 · React 19 · TypeScript · Prisma · PostgreSQL · Mistral AI · Vitest</sub>
<br />
<sub>⭐ If this project is useful or interesting, consider starring the repo!</sub>
</div>
