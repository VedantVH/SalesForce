# ComplyLens

> **Enterprise DPDP Act (2023) Compliance Automation & Cryptographic Governance Engine**  
> *Developed as part of the **Salesforce Compass Program***

[![Next.js](https://img.shields.io/badge/Next.js-15.5-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.1-blue?style=for-the-badge&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)](https://www.prisma.io/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v4-38B2AC?style=for-the-badge&logo=tailwind-css)](https://tailwindcss.com/)
[![Mistral AI](https://img.shields.io/badge/Mistral_AI-Integrated-FF7000?style=for-the-badge&logo=ai)](https://mistral.ai/)
[![Vitest](https://img.shields.io/badge/Vitest-3.2-6E9F18?style=for-the-badge&logo=vitest)](https://vitest.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

---

## 📌 Overview

**ComplyLens** is an enterprise data protection orchestration platform engineered to automate evaluations against India's **Digital Personal Data Protection (DPDP) Act, 2023**, developed as part of the **Salesforce Compass Program**. Built with a strict **zero-trust architecture**, it separates mathematical compliance evaluation from generative intelligence: **the deterministic engine decides, AI explains, and human operators approve**. The platform provides compliance teams, Data Protection Officers (DPOs), and privacy engineers with real-time risk scoring, reproducible rule traces, cryptographic audit integrity, and human-in-the-loop remediation.

---

## ✨ Key Features

- **⚡ Deterministic Rule Engine:** Evaluates contact portfolios against 5 versioned DPDP controls (**Consent, Purpose, Retention, Notice, Minimization**) on a 100-point scale with zero AI hallucination risk in scoring.
- **🔬 Interactive Rule Trace Studio:** Real-time, no-write sandbox enabling DPOs to toggle evidence parameters, inspect per-control trace matrices, and export reproducible scenario fingerprints.
- **🤖 Evidence-Grounded AI Briefings:** Server-isolated **Mistral AI integration** generates executive summaries grounded exclusively in persisted verdict metadata, backed by an instant deterministic fallback engine.
- **🛡️ Tamper-Evident Cryptographic Auditing:** Append-only audit log with serialized **SHA-256 hash chaining**, database transaction locking, **Merkle checkpoint proofs**, and exportable portable JSON proof bundles.
- **🔄 Non-Mutating Remediation Simulation:** Forecasts organizational score improvements and blast radius before applying human-approved fixes to CRM contacts.
- **🚨 Incident Command Cockpit:** Centralized incident tracker with statutory notification timers (72h CERT-In / 144h internal target), evidence logging, and CSV audit exports.

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | **Next.js 15 (App Router)**, **React 19**, **TypeScript (Strict Mode)**, **Tailwind CSS v4**, **Recharts**, **Lucide Icons** |
| **Backend / API** | **Next.js Server Actions & Route Handlers**, **Zod (Runtime Schema Validation)**, **JOSE (JWT Tokens)**, **Bcrypt** |
| **Database & ORM** | **PostgreSQL**, **Prisma ORM (v6.19)**, Connection Pooling, Serialized Transactions |
| **AI / LLM** | **Mistral AI API** (Server-Side Isolation, Zero Direct PII Transmission), Deterministic Fallback Engine |
| **Security & Auth** | Signed HTTP-only Secure Cookies, Dual-Role Authorization (`Admin`, `DPO Reviewer`), Merkle Trees, SHA-256 Chaining |
| **Testing & CI/CD** | **Vitest**, **Playwright Core** (Browser Smoke Suite), **ESLint 9**, **Docker** |

---

## 🏗️ Architecture

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                              INCOMING DATA                                  │
│         (CRM Contacts, Data Subject Requests, Audit Events)                 │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    DETERMINISTIC RULE ENGINE (TypeScript)                    │
│   • Typed Evidence Normalization     • DPDP Control Evaluation              │
│   • Mathematical Deduction Matrix    • Severity Gates & Versioned Rules     │
└──────────────────┬──────────────────────────────────────┬───────────────────┘
                   │ (Persisted Verdict)                  │ (Audit Event)
                   ▼                                      ▼
┌──────────────────────────────────────┐  ┌───────────────────────────────────┐
│       MISTRAL AI BRIEFING LAYER       │  │   CRYPTOGRAPHIC AUDIT LOG ENGINE  │
│  • Reads ONLY Persisted Metadata     │  │  • Serialized Transaction Locks   │
│  • Explains Findings & Next Actions  │  │  • SHA-256 Block Chaining         │
│  • Deterministic Offline Fallback    │  │  • Merkle Checkpoint Inclusion    │
└──────────────────┬───────────────────┘  └───────────────────┬───────────────┘
                   │                                          │
                   └──────────────────┬───────────────────────┘
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                      DPO GOVERNANCE & APPROVAL PORTAL                        │
│          (Rule Trace Studio • Fix Simulator • Remediation Dispatch)         │
└─────────────────────────────────────────────────────────────────────────────┘
```

> **Core System Invariant:** *The rule engine decides, AI explains, and humans approve. AI models have read-only access to persisted verdict metadata and cannot alter compliance scores, mutate records, or execute remediation without explicit human authorization.*

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
   DATABASE_URL="postgresql://postgres:password@localhost:5432/complylens?schema=public"
   JWT_SECRET="your-secure-random-32-character-secret-key"
   MISTRAL_API_KEY="your-mistral-api-key"
   DEMO_ADMIN_PASSWORD="SecureAdminPassword123!"
   DEMO_REVIEWER_PASSWORD="SecureReviewerPassword123!"
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
# Run unit & integration tests
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

- **100% Deterministic Accuracy:** Eliminated subjective compliance interpretations by executing standardized, versioned mathematical rule logic with zero AI hallucination in scoring.
- **Forensic Audit Integrity:** Built append-only SHA-256 hash chaining and Merkle trees to ensure tamper-evident logs verifiable by external regulatory bodies.
- **High-Throughput Performance:** Sub-10ms evaluation latency per contact record using optimized in-memory evaluators and indexed relational storage.
- **Comprehensive Quality Assurance:** Covered by 6 automated test suites encompassing authentication, cryptography, rule evaluation, fix simulation, and browser workflows.

---

## 👥 Contributors

- **[Vedant Vishwanath Honnangi](https://github.com/VedantVH)** — Architecture, Full-Stack Engineering, Rule Engine & AI Orchestration (Salesforce Compass Program)

---

## 🤝 Contributing & License

Contributions, issues, and feature requests are welcome. Feel free to review open items on the [issues page](https://github.com/VedantVH/SalesForce/issues).

Distributed under the **MIT License**. See `LICENSE` for details.

---

## 📬 Contact & Links

- **Author:** Vedant Vishwanath Honnangi
- **GitHub:** [@VedantVH](https://github.com/VedantVH)
- **Repository:** [https://github.com/VedantVH/SalesForce](https://github.com/VedantVH/SalesForce)
