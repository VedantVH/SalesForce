# ComplyLens

> **Enterprise DPDP Act (2023) Compliance Automation & Cryptographic Governance Engine**

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

**ComplyLens** is an enterprise data protection orchestration platform engineered to automate evaluations against India's **Digital Personal Data Protection (DPDP) Act, 2023**. Built with a strict **zero-trust architecture**, it separates mathematical compliance evaluation from generative intelligence: **the deterministic engine decides, AI explains, and human operators approve**.

The system features real-time risk scoring, an interactive **Rule Trace Studio**, cryptographic **SHA-256 Merkle audit chains**, non-destructive **remediation simulation**, and an **incident command center** for privacy and security officers.

---

## ✨ Key Features

- **⚡ Deterministic DPDP Compliance Engine:** Evaluates contact portfolios across 5 versioned controls (**Consent, Purpose, Retention, Notice, Minimization**) with mathematical scoring (100-point scale) and zero AI hallucination risk.
- **🔬 Interactive Rule Trace Studio:** Real-time, no-write sandbox enabling Data Protection Officers (DPOs) to toggle evidence parameters, inspect per-control trace matrices, and export reproducible scenario fingerprints.
- **🤖 Evidence-Grounded AI Briefings:** Server-isolated **Mistral AI integration** generates actionable executive summaries grounded exclusively in persisted verdict metadata, backed by an instant deterministic fallback engine.
- **🛡️ Tamper-Evident Cryptographic Auditing:** Append-only audit log with serialized **SHA-256 hash chaining**, database transaction locking, **Merkle checkpoint proofs**, and exportable portable JSON bundles.
- **🔄 Non-Mutating Remediation Simulation:** Forecasts organizational score impact and blast radius before applying human-approved fixes (e.g., consent re-engagement, purpose limitation, data pruning).
- **🚨 Incident & Breach Command Cockpit:** Centralized incident tracker with statutory notification SLAs (72h CERT-In / 144h internal), evidence management, CSV compliance export, and SDF operational review.

---

## 🛠️ Tech Stack

| Layer | Technologies & Tools |
|---|---|
| **Frontend** | **Next.js 15 (App Router)**, **React 19**, **TypeScript (Strict Mode)**, **Tailwind CSS v4**, **Recharts**, **Lucide Icons** |
| **Backend / API** | **Next.js Server Actions & API Routes**, **Zod (Runtime Validation)**, **JOSE (JWT Tokens)**, **Bcrypt** |
| **Database & ORM** | **PostgreSQL**, **Prisma ORM (v6.19)**, Connection Pooling, Serialized Transactions |
| **AI / LLM** | **Mistral AI API** (Server-Side Isolation, Zero Direct PII Transmission), Deterministic Fallback Engine |
| **Security & Auth** | Signed HTTP-only Secure Cookies, Dual-Role Authorization (`Admin`, `DPO Reviewer`), Merkle Trees, SHA-256 Chaining |
| **Testing & CI/CD** | **Vitest**, **Playwright Core** (End-to-End Browser Smoke Suite), **ESLint 9**, **Docker** |

---

## 🏗️ Architecture & Core Invariant

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

> **Core System Invariant:** *AI cannot calculate scores, alter compliance states, mutate records, or execute remediation without explicit human-in-the-loop authorization.*

---

## 🚀 Installation & Setup

### Prerequisites
- **Node.js**: `v20.x` or higher
- **PostgreSQL**: `v15+` (local or hosted like Supabase / Neon / Render)
- **npm** or **pnpm**

### Step-by-Step Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/VedantVH/SalesForce.git
   cd SalesForce/salesforce
   ```

2. **Configure Environment Variables**
   ```bash
   cp .env.example .env.local
   ```
   Configure the following in `.env.local`:
   ```env
   DATABASE_URL="postgresql://user:password@localhost:5432/complylens?schema=public"
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

5. **Run the Development Server**
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 💻 Usage & Verification

### Demo Credentials
| Role | Email | Password |
|---|---|---|
| **System Administrator** | `admin@complylens.demo` | Set via `DEMO_ADMIN_PASSWORD` |
| **DPO Reviewer** | `reviewer@complylens.demo` | Set via `DEMO_REVIEWER_PASSWORD` |

### Running the Test Suite
```bash
# Run unit & integration tests
npm test

# Run type checking & ESLint
npm run typecheck
npm run lint

# Run end-to-end browser smoke test
npm run smoke:browser
```

---

## 📊 Results & Impact

- **100% Deterministic Accuracy:** Replaced error-prone manual audits with automated, repeatable rule evaluations with zero scoring hallucination.
- **Cryptographic Non-Repudiation:** Implemented full SHA-256 hash chaining and Merkle trees to ensure forensic auditability under regulatory scrutiny.
- **Sub-10ms Rule Engine Latency:** High-throughput batch processing of contact portfolios using memory-efficient TypeScript evaluators.
- **Enterprise-Ready Testing:** Backed by 6 comprehensive automated test suites covering cryptographic integrity, authentication, deterministic rules, simulation logic, and browser smoke flows.

---

## 🤝 Contributing & License

Contributions, issues, and feature requests are welcome. Feel free to check the [issues page](https://github.com/VedantVH/SalesForce/issues).

Distributed under the **MIT License**. See `LICENSE` for more information.

---

## 📬 Contact & Links

- **Author:** Vedant Vishwanath Honnangi
- **GitHub:** [@VedantVH](https://github.com/VedantVH)
- **Project Repository:** [VedantVH/SalesForce](https://github.com/VedantVH/SalesForce)
