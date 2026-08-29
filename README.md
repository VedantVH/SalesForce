Here is the complete `README.md` code block in a single copyable box:

```markdown
# ComplyLens

> **Enterprise DPDP Act (2023) Compliance Automation & Cryptographic Governance Engine**  
> *Developed as part of the **Salesforce Compass Program***

[![Next.js](https://img.shields.io/badge/Next.js-15.5-black?style=for-the-badge&logo=nextdotjs)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.1-blue?style=for-the-badge&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)](https://www.prisma.io/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v4-38B2AC?style=for-the-badge&logo=tailwindcss)](https://tailwindcss.com/)
[![Mistral AI](https://img.shields.io/badge/Mistral_AI-Integrated-FF7000?style=for-the-badge)](https://mistral.ai/)
[![Vitest](https://img.shields.io/badge/Vitest-3.2-6E9F18?style=for-the-badge&logo=vitest)](https://vitest.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

---

## 📌 Overview

**ComplyLens** is an enterprise data protection compliance-operations platform engineered to automate evaluations against India's **Digital Personal Data Protection (DPDP) Act, 2023**, developed as part of the **Salesforce Compass Program**. Built on a strict **zero-trust architecture**, the platform decouples mathematical compliance evaluation from generative intelligence.

### The Product Invariant
🔒 **The Rule Engine Decides, AI Explains, and Humans Approve.**  
*Mistral AI operates strictly on minimized, persisted verdict metadata in read-only mode. It cannot calculate a score, change a status, mutate a compliance result, or execute remediation actions.*

ComplyLens functions as an evidence and decision governance layer above existing CRM, consent-management, security, and ticketing systems—acting as a control plane rather than a replacement system of record.

---

## ✨ Key Features

- **Deterministic Rule Engine:** Evaluates contact records against 5 versioned DPDP controls on a 100-point rubric. Eliminates score hallucination risk using rule-based evaluation logic.
- **Interactive Rule Trace Studio:** Provides a zero-side-effect sandbox for DPOs to test parameter adjustments in real time. Generates per-control trace matrices and exportable scenario fingerprints.
- **Context-Isolated AI Briefings:** Uses a server-isolated Mistral LLM to generate summaries bound strictly to persisted verdict metadata. Includes an immediate deterministic fallback engine for high availability.
- **Cryptographic Audit Log:** Secures event history using append-only SHA-256 hash chaining, transaction locks, and Merkle root verification. Generates portable, tamper-evident JSON proof bundles for compliance audits.
- **Dry-Run Remediation Simulator:** Projects compliance score shifts and system blast radius before executing CRM database updates. Prevents unintended data mutation during contact remediation cycles.
- **Incident Operations Center:** Tracks security events against statutory compliance timers, including 72-hour CERT-In and 144-hour internal SLAs. Centralizes evidence logging and generates structured CSV audit exports.

---

## 👥 Operating Model & Personas

- **DPO / Privacy Operations Lead (Primary):** Reviews organization posture, independently approves remediation, monitors retention pressure, coordinates privacy-specific breach obligations, and exports audit evidence.
- **CRM or Data Steward:** Investigates contact evidence and requests correction. *Cannot approve their own remediation or grant consent for a data principal.*
- **Incident Response Lead:** Records breach scope and operational milestones while the DPO manages notification evidence.

---

## 🛠️ Tech Stack & Specifications

| Layer | Technologies & Specifications |
|---|---|
| **Frontend** | **Next.js 15 (App Router, Turbopack)**, **React 19**, **TypeScript (Strict Mode)**, **Tailwind CSS v4**, **Recharts**, **Lucide Icons** |
| **Backend / API** | **Next.js Server Actions & Route Handlers**, **Zod (Runtime Schema Validation)**, **JOSE (JWT Tokens)**, **Bcrypt** |
| **Database & ORM** | **PostgreSQL (v16)**, **Prisma ORM (v6.19)**, Connection Pooling, Serialized Transaction Locks |
| **AI / LLM** | **Mistral AI API** (Server-Side Isolation, Zero Direct PII Transmission), Deterministic Offline Fallback Engine |
| **Security & Auth** | Signed HTTP-only Secure Cookies, Role-Based Access Control (`admin`, `dpo`, `reviewer`, `analyst`), Merkle Trees, SHA-256 Hash Chaining |
| **Testing & CI/CD** | **Vitest 3.2** (Unit & Integration Suite), **Playwright Core** (Browser Smoke Suite), **ESLint 9**, **Docker** |

---

## 🔌 Rule-Engine Extension Model

Evidence is normalized into five typed inputs, evaluated by deterministic controls, scored from 100 using versioned deductions, checked by a severity gate, and appended to assessment history with the exact rule version and legal mapping. 

To introduce a new policy:
1. Map policy evidence into typed inputs.
2. Implement a typed evaluator/remediation definition.
3. Publish a database-backed rule version.
4. Add boundary tests and activate the rule for future assessments.
5. *Historical verdicts retain their original rule versions permanently.*

---

## 🏗️ Architecture & Workflows

### System Architecture Diagram

```mermaid
flowchart TD
    UI[Next.js 15 Web Application<br>React 19 Dashboard, Trace Studio & Incident Cockpit] --> API[Next.js API Route Handlers<br>Session Guard, RBAC & Zod Validation]
    
    API --> Assess[Assessment Service<br>Batch Orchestration & Transaction Manager]
    API --> Sim[Simulation & Remediation<br>Impact Forecasting & State Machine]
    API --> AI[AI Explainer Service<br>Mistral API with Deterministic Fallback]
    API --> AuditServ[Audit Integrity Service<br>Chain Verification & Merkle Checkpoints]

    Assess <--> Engine[Deterministic Rule Engine<br>DPDP-001 to DPDP-005 Pure Functional Evaluator]
    
    Assess --> DB[(PostgreSQL Database<br>Prisma ORM Indexed Tables)]
    Sim --> DB
    AuditServ --> DB
    
    Assess --> AuditLog[SHA-256 Audit Logger<br>Serialized Hash Chaining]
    AuditLog --> DB
    
    DB -.->|Read-Only Verdict Metadata| AI

```

### Assessment & Governance Sequence

```mermaid
sequenceDiagram
    autonumber
    participant DPO as DPO / Privacy Officer
    participant API as API Handler (/api/assessments/run)
    participant Service as Assessment Service (lib/assessments)
    participant Engine as Rules Engine (lib/rules-engine)
    participant DB as PostgreSQL Database
    participant Audit as SHA-256 Audit Logger (lib/audit)
    participant AI as Mistral AI (/api/ai/explain)

    DPO->>API: POST /api/assessments/run (contactIds)
    API->>Service: runAssessments(contactIds, actorId)
    Service->>DB: Fetch Contacts, Consents, Purposes & Active Rule Versions
    DB-->>Service: Return Normalized Contact Evidence
    Service->>Engine: assessContact(evidence, ruleVersions)
    Engine-->>Service: Deterministic Scores, Severity Gate & Recommendations
    Service->>DB: Transactional INSERT (Assessment, Results, Recommendations)
    Service->>Audit: writeAudit(action, actorId, sequence)
    Audit->>DB: Compute & Store SHA-256 Chained Hash
    Service-->>API: Return Assessment Results
    API-->>DPO: Render Results on Dashboard
    
    opt Generate AI Briefing
        DPO->>AI: POST /api/ai/explain (verdictMetadata)
        AI->>DB: Read Persisted Verdict (Zero PII)
        AI-->>DPO: Return Structured Briefing & Remediation Playbook
    end

```

---

## 🚀 Local Setup & Development

### 1. Prerequisites

Ensure you have Node.js 18+ and a running PostgreSQL instance (or Docker) available.
For workstation setup, an isolated PostgreSQL container can be bound to `127.0.0.1:55432`:

```bash
docker run --name complylens-dev-db -e POSTGRES_PASSWORD=postgres -p 55432:5432 -d postgres:16

```

### 2. Installation Steps

```bash
# Clone repository
git clone [https://github.com/VedantVH/SalesForce.git](https://github.com/VedantVH/SalesForce.git)
cd SalesForce/salesforce

# Copy environment template
cp .env.example .env.local

# Install dependencies
npm install

# Run migrations & seed demo dataset
npm run db:migrate -- --name init
npm run db:seed

# Start development server
npm run dev

```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🔑 Environment Variables & Demo Access

### Environment Configuration (`.env.local`)

```env
DATABASE_URL="postgresql://USER:PASSWORD@127.0.0.1:55432/complylens?sslmode=disable"
JWT_SECRET="replace-with-at-least-32-random-characters"
MISTRAL_API_KEY="replace-with-your-mistral-api-key"
DEMO_ADMIN_PASSWORD="replace-with-a-strong-admin-password"
DEMO_REVIEWER_PASSWORD="replace-with-a-strong-reviewer-password"

```

### Managed Accounts

| Role | Email | Password |
| --- | --- | --- |
| **Administrator** | `admin@complylens.demo` | Value of `DEMO_ADMIN_PASSWORD` |
| **DPO Reviewer** | `reviewer@complylens.demo` | Value of `DEMO_REVIEWER_PASSWORD` |

*Note: Running `npm run db:seed` refreshes password hashes based on these variables without overwriting operational records.*

---

## 🧪 Testing & Verification Suite

Execute the full suite of automated checks before committing code:

```bash
# Unit and integration tests
npm test

# Linter validation
npm run lint

# Strict TypeScript typechecking
npm run typecheck

# Production build test
npm run build

# End-to-End Headless Browser Smoke Test
npm run smoke:browser

```

> **Smoke Test Scope:** Runs Playwright/Chrome against a production build (`npm start`) verifying auth, bulk assessments, contact investigation, simulation, remediation lifecycle, incident logging, AI fallback, audit proof export, CSV downloads, and layout stability.

---

## ☁️ Deployment Instructions

### Render Deployment (Blueprint)

1. Select **New → Blueprint** in Render and connect this repository.
2. Render will auto-detect `render.yaml`.
3. Provide `MISTRAL_API_KEY`, `DEMO_ADMIN_PASSWORD`, and `DEMO_REVIEWER_PASSWORD` when prompted (*Never use `NEXT_PUBLIC_` prefixes*).
4. Render handles database creation, migrations, seed bootstrapping, and building automatically.
5. Verification URL: `https://complylens.onrender.com/login`

*Manual Build/Start Override for Render Free Tier:*

* **Build Command:** `npm ci && npm run db:deploy && npm run db:seed && npm run build`
* **Start Command:** `npm start`

### Vercel Deployment

1. Provision a managed PostgreSQL database (e.g., Neon, Supabase) and obtain the pooled connection string.
2. Import the GitHub repository into Vercel.
3. Set environment variables: `DATABASE_URL`, `JWT_SECRET`, `MISTRAL_API_KEY`, `DEMO_ADMIN_PASSWORD`, `DEMO_REVIEWER_PASSWORD`.
4. Deploy the application, then execute one-time database setup from your CLI:
```bash
npm run db:deploy
npm run db:seed

```



---

## 🛡️ Security, Compliance Boundaries & Disclaimers

* **Audit Chain Bounds:** `ComplianceAssessment`, `ComplianceResult`, and `AuditLog` records are append-only. Cryptographic hash chains ensure tamper detection against application users, but external root anchoring (Phase 2) is required to restrict direct database administrator overwrites.
* **Session Security:** Session cookies use JOSE-signed tokens, HTTP-only flags, `SameSite=Lax`, 8-hour expiration windows, and strict `Secure` transport in production environments.
* **Zero-PII AI Boundary:** Mistral receives zero direct PII field data. Data residency and cross-border processing review for external LLM calls must be conducted prior to handling production personal data.
* **Regulatory Disclaimer:** Penalty calculations and SLAs (144-hour internal escalation targets) provided in the UI serve as operational aids and illustrative metrics. They do not constitute legal certification or liability predictions.

---

## 🤝 Contributing & License

Contributions and issue tracking are managed through the primary repository.

Distributed under the **MIT License**. See `LICENSE` for details.

```

```
