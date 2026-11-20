# UnicornMatch: AI-Powered Resume Scoring, Gap Analysis & The Council of HRRR

UnicornMatch is a production SaaS platform designed to eliminate the guesswork of job applications by evaluating resumes against target job descriptions using an LLM. Rather than generating surface-level keyword counts, the platform provides deep semantic compatibility scoring, missing competency audits, rewrite-ready feedback, and multi-perspective critique from five distinct AI hiring personas.

I designed and engineered the entire application end to end: React 19 frontend architecture, client-side PDF/DOCX parsing, 14+ serverless Deno edge functions, PostgreSQL RLS security policies, Stripe credit billing, and a proprietary AI-text fingerprint sanitization engine.

> **Source Code Notice:** The source repository is proprietary and private. This document details the platform's architectural topology, design decisions, data flow models, and subsystem scale. Proprietary prompt chains, scoring matrices, and production API credentials are deliberately omitted (see Section 7).

[![Live Platform](https://img.shields.io/badge/Platform-unicornmatch.app-f43f5e?style=flat-square)](https://www.unicornmatch.app/)
[![Database & Auth](https://img.shields.io/badge/Database-PostgreSQL%20%7C%20RLS-3ecf8e?style=flat-square)](https://www.postgresql.org)
[![Scale](https://img.shields.io/badge/Codebase-~20%2C250%20LoC-6366f1?style=flat-square)](#6-by-the-numbers)
[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-zinc?style=flat-square)](#10-license--intellectual-property)
[![Stack](https://img.shields.io/badge/React%2019-Vite%20%7C%20TypeScript-blue?style=flat-square)](https://react.dev)

---

## 1. The Problem

The modern hiring market is characterized by extreme asymmetry between job seekers and Applicant Tracking Systems (ATS). Job applicants face three fundamental structural challenges:

*   **Asymmetric ATS Filtering & Silent Rejections:** Companies receive hundreds of applicants per role and deploy automated ATS filters. Job seekers apply blindly without knowing how their resume matches the job description, frequently being rejected without actionable feedback.
*   **Shallow Keyword Counting vs. Semantic Alignment:** Most commercial "resume checkers" perform naive substring matching (e.g., checking if the exact phrase *"cross-functional leadership"* appears). They fail to evaluate contextual competency, seniority alignment, or evidence of tangible business impact.
*   **The "AI Fingerprint" Penalty:** Candidates using standard ChatGPT prompts often paste text filled with robotic buzzwords, em-dash overuse, and invisible zero-width Unicode artifacts left behind by LLM generators—triggering anti-AI heuristics in hiring pipelines.

UnicornMatch solves this by delivering structured, recruiter-grade semantic scoring, actionable gap analyses, multi-angle HR evaluation, and an automated sanitization engine that produces ATS-ready rewrites free of AI tells.

---

## 2. What It Does

*   **Semantic Resume-vs-JD Compatibility Engine:** Computes an objective Unicorn Match Score by semantically comparing candidate experience against job requirements via an LLM, highlighting matched competencies, missing core skills, and experience red flags.
*   **The Council of HRRR:** Eliminates the flaw of single-number scoring by convening five distinct AI-driven hiring personas (`/council`), each offering unfiltered perspectives:
    1.  *The Hard-Nosed Tech Lead:* Evaluates technical depth, architecture choices, and concrete execution.
    2.  *The Startup Founder:* Assesses speed, versatility, autonomy, and cross-functional ownership.
    3.  *The Corporate Recruiter:* Scans for ATS keyword density, formatting discipline, and career progression.
    4.  *The Culture Champion:* Reviews collaboration language, mentorship, and interpersonal resonance.
    5.  *The Strict Executive:* Focuses strictly on business impact, ROI, metrics, and strategic alignment.
*   **Proprietary AI Fingerprint Sanitizer:** Automatically scrubs invisible zero-width characters, non-standard whitespace, and AI-flavored typography (`unllm` + custom cleaning rules) from all generated rewrite recommendations, making suggestions safe to paste directly into documents without triggering detection algorithms. Public interactive demo hosted at `/ai-fingerprints`.
*   **Section-by-Section Resume Health Workspace:** A persistent `/resume` manager where users break down their resume into modular components (Summary, Experience, Education) for isolated re-scoring and iterative polishing over time.
*   **Client-Side In-Browser Document Extraction:** Extracts clean plain text directly in the browser from PDF and DOCX files (`pdfjs-dist`, `mammoth`) without transmitting unparsed binary blobs to backend servers.
*   **Direct Job Posting URL Scraping:** Extracts and cleans job requirements directly from live posting URLs, stripping surrounding boilerplate, navigation headers, and footer noise.
*   **Stripe-Backed Credit Economy:** Production credit purchasing ("Magic Credits") backed by transactional Stripe Checkout and cryptographic webhook fulfillment with atomic database transactions.

---

## 3. Product Surface

| Subsystem | What the User Gets |
|---|---|
| **Match Analysis Workspace** | Dual input dashboard with PDF/DOCX drag-and-drop, live URL extraction, real-time progress steps, and comprehensive scoring reports |
| **The Council of HRRR** | Dedicated interactive panel featuring 5 distinct recruiter personas delivering independent, unvarnished critiques of the candidate's profile |
| **My Resume (Health Tracker)** | Modular resume builder tracking health scores per section with single-click AI rechecks and persistent versioning |
| **AI Fingerprint Cleaner** | Interactive public diagnostic tool demonstrating invisible Unicode artifact detection and one-click typography normalization |
| **User Dashboard** | Secure user history displaying previous analyses, credit balances, transaction receipts, and stored resumes |
| **Authentication & Profile** | Frictionless sign-in via Google OAuth or passwordless email magic links managed through a cloud authentication service |
| **Billing & Credit Management** | Self-serve Stripe Checkout integration with automated webhook credit balance synchronization |

---

## 4. Architecture

```
   [ User Browser: React 19 + Vite + TypeScript ]
         |
         +--> [ Client-Side Document Parsing: pdfjs-dist / mammoth ]
         |
         v
   +-----------------------------------------------------------+
   |   Serverless Deno Edge Functions Layer                    |
   |   - Bearer Token Authentication & Rate Limiting           |
   |   - Payload Validation & Job Posting Sanitizer            |
   +-----------------------------------------------------------+
         |
         +---------------------------------------+
         |                                       |
         v                                       v
   +---------------------------+   +---------------------------+
   |  LLM API                  |   |  Stripe Billing Engine    |
   |  - Semantic Match Engine  |   |  - Checkout Sessions      |
   |  - 5 Council HR Personas  |   |  - Webhook Event Handlers |
   |  - Section Health Scorer  |   +---------------------------+
   +---------------------------+                 |
         |                                       |
         v                                       v
   +-----------------------------------------------------------+
   |   Text Sanitization Engine (unllm + regex cleaners)       |
   |   - Strips Zero-Width Spaces & Invisible Unicode Artifacts|
   |   - Normalizes AI Typography & Cliché Phrasing            |
   +-----------------------------------------------------------+
         |
         v
   +-----------------------------------------------------------+
   |   PostgreSQL Database (Cloud Managed)                     |
   |   - Strict Row Level Security (RLS) on all user data      |
   |   - Credit Ledger & Atomic Transaction Balance Decrements |
   |   - Saved Resumes & Historical Analysis Snapshots         |
   +-----------------------------------------------------------+
```

### Key Architectural Decisions

*   **Client-Side Document Parsing over Server-Side Workers:** Rather than uploading multi-megabyte binary PDF/DOCX files to serverless edge functions—incurring bandwidth costs and execution timeout risks—parsing occurs entirely in the browser using `pdfjs-dist` and `mammoth`. Edge functions receive clean, pre-sanitized text payloads, keeping function execution under 1 second.
*   **Multi-Agent Personas over Unified Score Averaging:** Standard resume tools average evaluations into a generic 75% score, which obscures critical weaknesses. The Council of HRRR isolates evaluation criteria into independent prompt threads, mirroring how real hiring committees debate candidates with conflicting priorities.
*   **Invisible Character Sanitization as a First-Class Output Layer:** Generated rewrites pass through an automated cleanup pipeline (`unllm` and zero-width character scrubbers) before reaching the user, ensuring suggestions are safe to paste into resumes without triggering automated anti-AI filters.
*   **Database-Centric Credit Ledger with RLS:** Magic credit balances and consumption transactions are enforced directly within PostgreSQL via Row Level Security (RLS) policies and atomic decrements, preventing double-spend exploits or race conditions from concurrent client requests.

---

## 5. Tech Stack

| Layer | Choice |
|---|---|
| **Frontend Framework** | React 19, TypeScript, Vite 5/6 |
| **Routing & State** | React Router v6, React Context (AuthContext single source of truth) |
| **Styling & Animation** | Tailwind CSS, Framer Motion (page transitions and interactive cards) |
| **Data Visualizations** | Recharts (category radar charts, match score gauges) |
| **Icons & Typography** | Lucide React, Plus Jakarta Sans |
| **Backend & Edge Functions**| Serverless Edge Functions (Deno: 14+ microservices) |
| **Database & Security** | PostgreSQL 15+ with strict Row Level Security (RLS) policies |
| **Authentication** | Managed Cloud Auth (Google OAuth 2.0 & passwordless magic links) |
| **AI & NLP Inference** | LLM API |
| **Text Sanitization** | `unllm`, custom Unicode & invisible-character cleaning routines |
| **Document Processing** | `pdfjs-dist` (PDF extraction), `mammoth` (DOCX parsing) |
| **Payments & Billing** | Stripe Checkout API, Stripe Webhooks |

---

## 6. By the Numbers

| Metric | Figure |
|---|---|
| **Active Codebase** | **~20,250 lines of code** across 109 frontend and edge files |
| **Edge Microservices** | **14+ Serverless Deno functions** (`analyze-resume`, `convene-council`, etc.) |
| **Database Migrations** | **26 version-controlled PostgreSQL migrations** |
| **Evaluation Personas** | **5 distinct Council HR perspectives** |
| **Document Parsing** | **100% in-browser client-side extraction** (zero server PDF storage) |
| **Security Architecture**| **Strict Row Level Security (RLS)** covering 100% of user tables |
| **Commercial Billing** | Live Stripe webhook processing with atomic credit fulfillment |

### Subsystems Breakdown

*   `components/`: 34 modular React components covering the Welcome landing page, Analysis Result dashboards, My Resume section manager, Council of HRRR panels, and billing modals.
*   `edge-functions/analyze-resume/`: Core resume-vs-JD evaluation pipeline processing semantic match, keyword categorization, and rewrite drafting.
*   `edge-functions/convene-council/`: Parallel evaluation engine driving the 5 distinct HR persona reactions.
*   `edge-functions/_shared/textSanitizer.ts`: Proprietary cleaning library stripping invisible Unicode markers and normalizing AI typography.
*   `services/`: Frontend API bridge coordinating database client calls, edge function invocations, client-side document parsers, and Stripe checkouts.
*   `db/migrations/`: 26 SQL migration files establishing user credit ledgers, analysis run history, saved resume sections, and strict RLS policies.

---

## 7. What Is Not In Here, On Purpose

This repository is published for technical demonstration and recruitment evaluation. It deliberately omits:

*   Proprietary LLM system prompts and behavioral rubrics for the 5 Council of HRRR personas.
*   The exact internal weights and scoring algorithms governing the Unicorn Match Score.
*   Production database service role secrets, Stripe webhook signing keys, and Google OAuth client secrets.
*   Live production customer records and stored resume analysis data.

Live architectural walkthroughs or staging demonstrations can be scheduled upon request.

---

## 8. What I Would Point You To

*   **Client-Side Document Extraction Architecture:** Zero-overhead text parsing in the browser (`pdfjs-dist`, `mammoth`), completely eliminating server-side file upload bottlenecks.
*   **The Council of HRRR Evaluation Topology:** How independent AI hiring personas provide nuanced, actionable critique without flattening feedback into generic averages.
*   **AI Fingerprint & Invisible Unicode Sanitizer:** The real-time cleanup layer preventing AI watermarks, zero-width spaces, and typographic artifacts from leaking into candidate resumes.
*   **Production Billing & RLS Integrity:** Robust Stripe webhook integration paired with atomic database-level credit decrementing under strict PostgreSQL Row Level Security.

---

## 9. Links

*   **Live SaaS Application:** [https://www.unicornmatch.app/](https://www.unicornmatch.app/)
*   **Author:** Rayner Lim ([LinkedIn](https://www.linkedin.com/in/raynerlim/))

---

## 10. License & Intellectual Property

© 2026 Rayner Lim. All rights reserved.

This repository, documentation, architecture diagrams, and associated showcase materials are proprietary and published solely for demonstration and recruitment evaluation purposes. No license is granted to copy, reproduce, fork, redistribute, or build derivative commercial products from this project without explicit written permission from the author.
