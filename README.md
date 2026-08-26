# ComplyLens

> **Enterprise DPDP Act (2023) Compliance Automation & Cryptographic Governance Engine**  
> *Developed as part of the **Salesforce Compass Program***

[![Salesforce Compass](https://img.shields.io/badge/Salesforce-Compass%20Program-00A1E0?style=for-the-badge&logo=salesforce)](https://developer.salesforce.com/)
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

**ComplyLens** is an enterprise data protection orchestration platform engineered to automate evaluations against India's **Digital Personal Data Protection (DPDP) Act, 2023**, developed as part of the **Salesforce Compass Program**. Built on a strict **zero-trust architecture**, the system decouples mathematical compliance evaluation from generative intelligence: **the deterministic engine decides, AI explains, and human operators approve**. The platform equips Data Protection Officers (DPOs), compliance teams, and privacy engineers with real-time risk scoring, reproducible rule traces, cryptographic audit integrity, and human-in-the-loop remediation.

---

## ✨ Key Features

- **⚡ Deterministic Rule Engine:** Evaluates contact portfolios against 5 versioned DPDP controls (**Consent, Purpose, Retention, Notice, Minimization**) on a 100-point scale with zero AI hallucination risk in scoring.
- **🔬 Interactive Rule Trace Studio:** Real-time, no-write sandbox enabling DPOs to toggle evidence parameters, inspect per-control trace matrices, and export reproducible scenario fingerprints.
- **🤖 Evidence-Grounded AI Briefings:** Server-isolated **Mistral AI integration** generates structured executive summaries grounded exclusively in persisted verdict metadata, backed by an instant deterministic fallback engine.
- **🛡️ Tamper-Evident Cryptographic Auditing:** Append-only audit trail with serialized **SHA-256 hash chaining**, database transaction locking, **Merkle checkpoint proofs**, and exportable portable JSON proof bundles.
- **🔄 Non-Mutating Remediation Simulation:** Forecasts organizational score impact and blast radius before applying human-approved fixes to CRM contacts.
- **🚨 Incident Command Cockpit:** Centralized incident tracker with statutory notification timers (72h CERT-In / 144h internal target), evidence logging, and CSV audit exports.

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | **Next.js 15 (App Router, Turbopack)**, **React 19**, **TypeScript (Strict Mode)**, **Tailwind CSS v4**, **Recharts**, **Lucide Icons** |
| **Backend / API** | **Next.js Server Actions & Route Handlers**, **Zod (Runtime Schema Validation)**, **JOSE (JWT Tokens)**, **Bcrypt** |
| **Database & ORM** | **PostgreSQL**, **Prisma ORM (v6.19)**, Connection Pooling, Serialized Transaction Locks |
| **AI / LLM** | **Mistral AI API** (Server-Side Isolation, Zero Direct PII Transmission), Deterministic Offline Fallback Engine |
| **Security & Auth** | Signed HTTP-only Secure Cookies, Role-Based Access Control (`admin`, `dpo`, `reviewer`, `analyst`), Merkle Trees, SHA-256 Hash Chaining |
| **Testing & CI/CD** | **Vitest** (Unit & Integration Suite), **Playwright Core** (Browser Smoke Suite), **ESLint 9**, **Docker** |

---

## 🏗️ Architecture

```mermaid
flowchart LR
    %% Black & White Minimalist Architecture
    classDef dark fill:#000000,stroke:#000000,stroke-width:2px,color:#ffffff;
    classDef light fill:#ffffff,stroke:#000000,stroke-width:2px,color:#000000;
    classDef soft fill:#f4f4f5,stroke:#000000,stroke-width:1.5px,color:#000000;

    User([👤 DPO / Analyst]):::dark --> Web[💻 Next.js 15 Portal<br>Dashboard & Trace Studio]:::light
    Web --> Engine[⚙️ Deterministic Rule Engine<br>Evaluates 5 DPDP Controls]:::light
    Engine --> DB[(🗄️ PostgreSQL + SHA-256<br>Audit Ledger & Verdicts)]:::soft
    DB --> AI[🤖 Mistral AI Assistant<br>Read-Only Verdict Briefings]:::light
    AI --> Approval([👮 Human Approval Gate<br>Remediation Sync to CRM]):::dark
```

> **Core Invariant:**  
> 🔒 **The Rule Engine Decides, AI Explains, and Humans Approve.**  
> *AI models operate strictly on persisted metadata in read-only mode and are mathematically isolated from calculating scores, modifying compliance states, or triggering remediation actions.*

---

## 🚀 Installation & Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/VedantVH/SalesForce.git
   cd SalesForce/salesforce
   ```

2. **Configure Environment Variables**
   ```bash
   cp .env.example .env.local
   ```
   Provide the required variables in `.env.local`:
   ```env
   DATABASE_URL="postgresql://USER:PASSWORD@HOST/DB?sslmode=require"
   JWT_SECRET="replace-with-at-least-32-random-characters"
   MISTRAL_API_KEY="replace-with-your-mistral-key"
   DEMO_ADMIN_PASSWORD="replace-with-a-strong-unique-password"
   DEMO_REVIEWER_PASSWORD="replace-with-a-different-strong-password"
   ```

3. **Install Dependencies**
   ```bash
   npm install
   ```

4. **Initialize Database & Seed Data**
   ```bash
   npm run db:migrate -- --name init
   npm run db:seed
   ```

5. **Start Development Server**
   ```bash
   npm run dev
   ```
   Access the application at `http://localhost:3000`.

---

## 💻 Usage & Verification

### Demo Credentials
| Role | Email | Password |
|---|---|---|
| **System Administrator** | `admin@complylens.demo` | Configured via `DEMO_ADMIN_PASSWORD` |
| **DPO Reviewer** | `reviewer@complylens.demo` | Configured via `DEMO_REVIEWER_PASSWORD` |

### Test & Quality Commands
```bash
# Run unit & integration test suites
npm test

# Run ESLint validation
npm run lint

# Run strict TypeScript type checking
npm run typecheck

# Run end-to-end browser smoke test suite
npm run smoke:browser
```

---

## 📊 Results & Impact

- **Deterministic Verdict Integrity:** Enforces pure mathematical rule evaluations across all 5 DPDP controls with zero AI hallucination in scoring.
- **Cryptographic Auditability:** Guarantees append-only tamper detection through serialized SHA-256 hash chaining and exportable Merkle inclusion proofs.
- **Strict Separation of Concerns:** Architecturally isolates LLM reasoning from state mutation, ensuring all remediation actions require human-in-the-loop approval.
- **Comprehensive Test Coverage:** Backed by 6 automated test suites covering authentication security, cryptographic hashing, rule engine boundaries, fix simulations, and browser workflows.

---

## 👥 Contributors

- **[Vedant Vishwanath Honnangi](https://github.com/VedantVH)** — Architecture, Full-Stack Engineering, Rule Engine & AI Orchestration (Salesforce Compass Program)

---

## 🤝 Contributing & License

Contributions, issues, and feature requests are welcome. Feel free to review open items on the [issues page](https://github.com/VedantVH/SalesForce/issues).

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## 📬 Contact & Links

- **Author:** Vedant Vishwanath Honnangi
- **GitHub:** [@VedantVH](https://github.com/VedantVH)
- **Repository:** [https://github.com/VedantVH/SalesForce](https://github.com/VedantVH/SalesForce)
