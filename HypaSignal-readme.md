# HypaSignal: Signal-Based B2B Lead Intelligence

HypaSignal is a live, paid SaaS product that finds B2B sales leads by looking for evidence that a company is ready to buy now, instead of filtering a static contact database. It researches companies, scores how strong the buying signal is, finds the right contact, and drafts and sends the outreach, with the user in control at each step.

I designed and built it end to end as a solo developer: product, UX, architecture, backend, billing, email infrastructure, marketing site and operations.

> **Proprietary Notice:** The source is private. This document describes what the system does and how it is shaped. Prompts, scoring logic and data sources are deliberately left out (see section 7).

[![Live Platform](https://img.shields.io/badge/Live-hypasignal.com-indigo?style=flat-square)](https://hypasignal.com)
[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-zinc?style=flat-square)](#10-license--intellectual-property)
[![Stack](https://img.shields.io/badge/React%2019-Vite%206-blue?style=flat-square)](https://react.dev)
[![Backend](https://img.shields.io/badge/Supabase-PostgreSQL-emerald?style=flat-square)](https://supabase.com)

## 1. The Problem

Most outbound sales tools sell a list. Everyone who buys the same list emails the same people with the same pitch, and the people on it were picked by job title and company size, not by whether they have any reason to care this month.

Three problems follow from that:

*   **Timing is invisible.** A filter cannot tell a company that is hiring for the exact problem you solve from one that is not.
*   **AI makes the list problem worse.** Generic AI-written email at volume trains buyers to ignore everything, and models state invented facts about companies with total confidence.
*   **Small teams cannot do the research by hand.** The good version of outbound (read the news, check the hiring page, find the right person, write something specific) does not scale past a handful of leads a day.

## 2. What It Does

*   **Finds companies by signal.** The user describes who they sell to and what they sell. The system searches for companies showing evidence of need, and also supports classic attribute-based targeting for users who want it.
*   **Scores buying intent.** Each lead gets a score and a plain-language explanation of why, so the user can sort by readiness to buy instead of by company size.
*   **Finds the contact.** It identifies decision makers and verifies contact details before anything is shown or charged for.
*   **Drafts outreach that uses the evidence.** Messages are generated per lead and per contact, in the lead's own language where it matters (English, Chinese and Vietnamese are supported across the product).
*   **Runs a hands-free follow-up workflow.** A multi-step contact sequence that sends from the user's own mailbox, pauses when someone replies, and respects unsubscribes and bounces.
*   **Closes the loop.** Opens, clicks and interest signals feed back into the campaign so the user sees which leads are warming up.

## 3. Product Surface

| Area | What the user gets |
|---|---|
| Campaigns | Create, run, re-run and compare lead searches; every run is numbered and auditable |
| Leads | Sortable, filterable table with match tier, intent tier, contact status and starred leads |
| Intent engine | Score plus a written rationale per lead, with live progress while it runs |
| Contact Workflow | Multi-step sequence with scheduling, reply detection and per-lead pause and resume |
| Analytics | Engagement by lead, run and contact step |
| Billing | Credit-based plans, trial, upgrade, cancel and top-ups through Stripe |
| Teams | Organizations, invitations, seats and roles |
| Admin | Superadmin console for health, credits, impersonation and system settings |
| Public site | Landing pages, comparison pages, a lead magnet and a free market-report tool |

## 4. Architecture

```
  Browser (React SPA)
        |
        |  Supabase JS client (RLS-enforced) + Edge Function calls
        v
  +-------------------------------+      +--------------------------+
  |  Supabase Postgres            | <--> |  Edge Functions (Deno)   |
  |  RLS on every table           |      |  ~90 functions           |
  |  SECURITY DEFINER RPCs        |      |  AI, search, email,      |
  |  pg_cron + queue tables       |      |  billing, cron workers   |
  +-------------------------------+      +-----------+--------------+
                                                     |
                      +------------------------------+---------------+
                      |                  |                           |
                   LLM API          Stripe (2 accts)       SMTP / mail providers
```

**Design decisions worth calling out**

*   **Postgres is the source of truth and the queue.** Long-running work (scoring, drafting, sending) is modelled as rows in queue tables and drained by workers, not held in a request. That makes every job resumable, observable and retryable.
*   **Row Level Security everywhere.** The browser talks to the database directly, so authorization lives in the database. Privileged functions are locked down explicitly at creation, backed by CI checks that fail the build on a new unguarded function or table grant.
*   **Idempotent by default.** Webhooks, sends, credit grants and scoring passes can all be delivered twice without double-charging or double-sending.
*   **Failure is a first-class state.** Every long job has a timeout story, a recovery path and a visible failed state, because AI providers and mail servers do fail.
*   **Money is a ledger.** Credits are append-only transactions with a reason, not an editable number.

## 5. Tech Stack

| Layer | Choice |
|---|---|
| Frontend | React 19, TypeScript 5.8, Vite 6, Tailwind CSS 3, React Router 7 |
| Data fetching | TanStack Query with optimistic updates |
| i18n | i18next, 3 locales, lazy-loaded namespaces, CI check for missing keys |
| Backend | Supabase: Postgres, Auth, Row Level Security, Edge Functions on Deno |
| AI | Large language model (LLM), with and without live web grounding, structured output |
| Billing | Stripe Checkout, Customer Portal and webhooks across two Stripe accounts |
| Email | Transactional and campaign email over SMTP, with open, click and reply tracking |
| Scheduling | pg_cron, GitHub Actions and a Cloudflare Worker for heartbeat and watchdog jobs |
| Testing | Vitest (unit), Playwright (end to end), Deno tests for shared backend logic |
| Hosting | Vercel (front end), Supabase (back end), Cloudflare (DNS and edge) |

## 6. By the Numbers

| Metric | Figure |
|---|---|
| TypeScript and TSX, front end | about 107,000 lines |
| TypeScript, Edge Functions | about 41,000 lines |
| SQL, schema and migrations | about 16,000 lines across 390+ migrations |
| Edge Functions | 90+ |
| Routed pages | around 50 |
| Git history | 1,800+ commits since December 2025 |
| CI workflows | 12 (tests, security advisors, grants check, cron health, email health, watchdogs) |
| Languages | English, Chinese, Vietnamese |

### Subsystems

*   **Search and discovery**: two strategies (signal-led and attribute-led), a shared cache so a company researched once is not paid for twice, and wallet-aware limits so a run never spends more than the user holds.
*   **Intent engine**: a two-stage scorer where a fast first pass covers every lead and a deeper pass adds live research, with recovery nets so no lead is silently left unscored.
*   **Contact engine**: a single contact model shared by drafting, sending and analytics, with verification before display.
*   **Drafting**: AI-generated outreach with checks that reject claims it cannot support.
*   **Contact Workflow**: the scheduler, sender and mailbox watcher for multi-step sequences, with a dry-run mode running in CI.
*   **Billing and credits**: plans, trial, cancellation, failed-payment handling and an auditable credit ledger.
*   **Reliability layer**: heartbeats, cron failure alerts, orphan cleanup, a function-deploy verifier and a hash-lock on mission-critical code blocks.
*   **Marketing and growth**: SEO pages, comparison pages, a gated lead magnet and a public market-report pipeline.

## 7. What Is Not In Here, On Purpose

This is a product I am still selling. It is not open source. I have kept out the prompts, the scoring and ranking logic, the data sources and the thresholds, because that is where the difference between this and a thin wrapper around a model lives. If you are a recruiter or an engineer and want a deeper walkthrough, I am happy to do it live.

## 8. What I Would Point You To

*   **Engineering discipline on a solo project**: CI that fails on security regressions, an incident log of real production failures that new work must be checked against, and deploys that are verified rather than assumed.
*   **Production AI**: handling models that are wrong, slow or silently misaligned (for example, a fix this month for a model that mislabelled records when asked to echo long identifiers) rather than assuming they behave.
*   **Billing correctness**: cancellation, downgrade and failed-payment rules written down and enforced in the database.
*   **Product judgement**: the product is deliberately narrow, and a lot of the work was deciding what not to build.

## 9. Links

*   **Live Product:** [https://hypasignal.com](https://hypasignal.com)
*   **Author:** Rayner Lim ([LinkedIn](https://www.linkedin.com/in/raynerlim/))

---

## 10. License & Intellectual Property

© 2026 Rayner Lim. All rights reserved.

This repository, documentation, architecture diagrams, and associated showcase materials are proprietary and published solely for demonstration and recruitment evaluation purposes. No license is granted to copy, reproduce, fork, redistribute, or build derivative commercial products from this project without explicit written permission from the author.
