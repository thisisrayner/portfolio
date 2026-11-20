# Brandori: Narrative-Driven Marketing & Product Showcase Platform

Brandori is a live web platform created for marketers, product strategists, and creative directors to showcase their campaigns in a structured, narrative-driven case study format. Inspired by GitHub's documentation depth and Behance's visual craft, Brandori bridges the gap between raw visual assets and strategic rigor—allowing creators to document the insights, hypotheses, execution hurdles, and business outcomes behind their work.

I designed and built it end to end as a solo developer: product strategy, design system, full frontend architecture, custom template engine, interactive physics, backend data layer, and performance optimization.

> **Proprietary Notice:** The source code is private. This document describes the platform's product purpose, architecture, subsystem topology, and engineering design decisions. Proprietary prompts, internal database triggers, and verification algorithms are deliberately omitted (see section 7).

[![Live Platform](https://img.shields.io/badge/Live-brandori.app-indigo?style=flat-square)](https://brandori.app)
[![Live Showcase](https://img.shields.io/badge/Showcase-Rayner%20Lim-emerald?style=flat-square)](https://brandori.app/u/raynerlim)
[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-zinc?style=flat-square)](#10-license--intellectual-property)
[![Stack](https://img.shields.io/badge/React%2019-Vite%206-blue?style=flat-square)](https://react.dev)

---

## 1. The Problem

Marketing and growth portfolios are notoriously fragmented. Most professionals rely on scattered PDFs, drive folders, slide decks, or static image grids.

Three critical problems follow from that:

*   **Context and strategy are lost.** Traditional visual platforms (Dribbble, Behance) treat marketing like graphic design, showcasing final hero assets while completely hiding the strategic hypotheses, audience insights, and performance metrics that determined campaign success.
*   **Recruiters and hirers cannot audit contribution.** Hiring managers evaluating senior marketing or product talent struggle to discern individual contribution from agency output, because static decks rarely document decision-making frameworks.
*   **Case study creation is high-friction.** Writing comprehensive web case studies usually requires custom CMS engineering or inflexible website builders that lack standardized marketing structures (KPI tracking, volume plate numbering, audience segmentation).

Brandori solves this by providing a standardized, elegant medium where marketing professionals present their strategic craft with the same depth that software engineers present code on GitHub.

---

## 2. What It Does

*   **9 Archetypal Portfolio Templates.** Creators choose from distinctive visual archetypes—from high-fashion editorial magazines (*Volume spreads*) and corporate dossiers (*Audit Dossier*) to minimalist Swiss typography (*Monochrome Minimal*) and mobile-first 3D decks (*Swipe Deck*).
*   **8 Specialized Campaign Case Study Formats.** Each project can be framed using a tailored narrative archetype (e.g., Deep Dive Narrative, Lookbook Visual, Data & Metrics Dossier, Startup Launch, Technical Architecture).
*   **Fast-Navigation Viewport Scrubber.** A custom-engineered floating jump pill (`CampaignJumpNav`) tracks active viewport coordinates and allows visitors to fluidly scrub through dozens of case studies with real-time tooltips and dot markers.
*   **Dual-View Mode Toggle.** Every portfolio template seamlessly switches between full-bleed narrative stories and high-density, compact 3–4 column thumbnail catalogues with persistent view synchronization.
*   **Smart URL & Identity Detection.** Social and external links automatically inspect destination hostnames to render native platform iconography (GitHub, Substack, YouTube, Medium, Blog, Figma, Dribbble, etc.) rather than masking everything as generic websites.
*   **Zero-Latency Page Transitions.** Optimistic cache priming instantly seeds case study views from public profile query caches, eliminating loading states and layout jumps on navigation.
*   **Interactive Micro-Interactions.** Tactile physics including draggable profile tokens with pointer-capture and spring dampening, 3D card flick gestures, and real-time color palette customization.

---

## 3. Product Surface

| Area | What the user gets |
|---|---|
| **Public Portfolio** | Curated showcase at `/u/:username` or vanity `/p/:username` with dynamic SEO, OpenGraph metadata, and structured JSON-LD schemas |
| **Case Study View** | Full-depth project breakdowns with cover plates, client metadata, interactive KPI metric pills, gallery sliders, and testimonial quotes |
| **Interactive Scrubber** | Minimalist floating jump scrubber with real-time project title floating tooltips and smooth scroll interpolation |
| **Template Engine** | 9 swappable portfolio layouts and 8 campaign presentation archetypes |
| **View Switcher** | Instant toggle between narrative magazine layouts and high-density 3–4 column thumbnail grids |
| **Live Visual Editor** | Real-time workspace for drafting campaigns, re-ordering projects, configuring accent colors, and toggling visibility |
| **AI Synthesis Assistant** | Integrated AI copy assistant to refine campaign summaries and sharpen value propositions |
| **Social Proof & Metrics** | Client endorsement workflows, verified badges, live view tracking, and like/share engagement metrics |
| **Community Feed** | Public discovery feed showcasing featured campaigns and creative talent across the network |

---

## 4. Architecture & Technical Decisions

```
   Browser (React 19 SPA)
         |
         |  TanStack Query v5 Cache + Supabase JS Client (RLS-enforced)
         v
   +---------------------------------------+      +---------------------------+
   |  Supabase Postgres                    | <--> |  Edge Functions (Deno)    |
   |  - Row Level Security (RLS) policies  |      |  - AI narrative generator |
   |  - Automated timestamps & triggers    |      |  - Email dispatch (Resend)|
   |  - User profiles & campaigns tables   |      |  - Verification workers   |
   +---------------------------------------+      +---------------------------+
                        |
                        +---> Supabase Storage (Campaign assets, responsive avatars)
```

### Architectural Conviction: Why a Cache-Primed SPA over Next.js SSR?

Early prototypes considered server-side rendering (SSR), but deliberate benchmarks revealed critical trade-offs for portfolio platforms:
1. **0ms Perceived Transition Latency:** On portfolio sites, visitors browse between multiple projects by the same creator. Serverless SSR hops incur cold-start latency and full document reconciliations on every project click. Brandori loads the creator's profile payload once and synchronously seeds the TanStack Query campaign cache on card click, enabling instant `0ms` navigation.
2. **Layout Shell Preservation:** Router architecture wraps the view inside persistent layout boundaries, isolating Suspense strictly to dynamic content outlets. The navigation bar, jump scrubber, and global chrome stay mounted without flicker.
3. **Low-Cost Global Edge Deployment:** As a purely static SPA bundle distributed across global CDN edges backed by Supabase Postgres, the platform achieves near-perfect Lighthouse scores without running persistent Node.js server infrastructure.

### Resilience & Edge State Handling

*   **Vanity Route Harmonization:** Robust URL resolution seamlessly rewrites vanity `/p/:username` requests into `/u/:username` and synchronizes target case study parameters without breaking browser history or SEO indexing.
*   **Graceful 404 & Empty State Recovery:** Missing or unpublished profiles trigger structured recovery states with search fallback and home redirection, avoiding white-screen crashes.
*   **Multi-Pointer Event Normalization:** Touch gestures, drag scrubbing, and draggable avatar tokens use native Pointer Events with explicit pointer capture (`setPointerCapture`) and dampening cubic-bezier springs, guaranteeing stutter-free 60fps responsiveness across mobile touchscreens and desktop mice.
*   **Cross-Tab State Synchronization:** Profile settings and template updates broadcast instantaneously across open browser tabs using `BroadcastChannel('brandori-sync')`, ensuring edits reflect in preview tabs without manual page refreshes.
*   **Row Level Security (RLS) as Authorization Source:** Database-level RLS policies strictly control visibility—visitors and recruiters only receive published, public campaigns, while owners can preview private drafts within their management view.

---

## 5. Tech Stack

| Layer | Choice |
|---|---|
| **Frontend Framework** | React 19.2, TypeScript 5.8, Vite 6.2 |
| **Routing** | React Router 7.10 |
| **State & Data Fetching** | TanStack Query v5 (stale-while-revalidate, cache seeding, optimistic updates) |
| **Styling & Design System** | Tailwind CSS 3, custom Charcoal & Cream design tokens, KaTeX typography |
| **Interactive & Gestures** | Native Pointer Capture API, @dnd-kit (drag-and-drop campaign reordering) |
| **Data & Storage** | Supabase Postgres (RLS, stored triggers), Supabase Object Storage |
| **Backend & Serverless** | Supabase Edge Functions (Deno runtime) |
| **AI Integration** | Google Gemini API (context-aware campaign synthesis and copy polish) |
| **Analytics & Charts** | Recharts (portfolio engagement insights) |
| **Testing & CI** | Playwright (multi-tier smoke tests & E2E flows in GitHub Actions) |
| **Hosting & CDN** | Vercel (Edge SPA hosting), Supabase Cloud |

---

## 6. By the Numbers

| Metric | Figure |
|---|---|
| **TypeScript & TSX (Frontend & Shared)** | ~22,100 lines |
| **SQL Migrations & Schema Definitions** | ~1,500 lines across 19 migrations |
| **Portfolio Layout Templates** | 9 flagship archetypes |
| **Campaign Case Study Archetypes** | 8 specialized narrative layouts |
| **Git Commit History** | 330+ structured commits |
| **Automated Test Coverage** | Multi-tier Playwright smoke test suite in CI |
| **Design System** | Custom charcoal/cream tokens, dark/light ambient lighting engines |

### Core Subsystems

*   **Template Rendering Engine:** A modular template architecture decoupling portfolio layouts from underlying data structures, allowing seamless template switching without data loss.
*   **Campaign Jump Navigator:** A viewport-tracking floating scrubber that calculates relative vertical document positions, manages dot marker states, and renders directional tooltips during scroll scrubbing.
*   **Smart Link Metadata Detector:** A regex-driven URL inspector that parses destination domains and subdirectories to assign appropriate platform icons, brand color themes, and human-readable labels.
*   **Tactile Physics Engine:** Custom spring physics and pointer handlers powering the draggable profile badges and mobile Tinder-style campaign card swipe deck.
*   **Client Endorsement Subsystem:** Token-gated recommendation requests allowing collaborators to submit authenticated testimonials and verified project badges.

---

## 7. What Is Not In Here, On Purpose

This is a proprietary production product. The repository does not include:

*   Internal system prompts and temperature parameters used for AI campaign narrative synthesis.
*   Raw database migration triggers governing abuse detection and anti-scraping rate limits.
*   Cryptographic token generation algorithms for client recommendation invites.
*   Private infrastructure environment keys and webhook signature secrets.

For recruiters, engineering leaders, or hiring managers who wish to inspect the complete production repository or request a live architecture walkthrough, access can be granted upon request.

---

## 8. What I Would Point You To

*   **Clean Component Decomposition & Separation of Concerns:** Portfolio templates (`Classic`, `Editorial`, `Showcase`, `Bento`, etc.) operate as interchangeable layout lenses over a unified `PortfolioData` contract.
*   **Attention to Micro-Interactions & Detail:** Subtle optical adjustments—from suppressing scrub tooltips in multi-column grid modes to elastic spring resets on draggable tokens—elevate the application from a generic CRUD tool into a polished consumer product.
*   **Performance Discipline:** Zero layout shifts on route changes, strict pre-fetching on card interaction, and optimized WebP asset loading keep lighthouse performance near 100.
*   **Production Robustness:** Strict TypeScript typings, comprehensive error-boundary fallbacks, and multi-tier Playwright smoke suites running on every pull request.

---

## 9. Links

*   **Live Platform:** [https://brandori.app](https://brandori.app)
*   **Live Profile Showcase:** [https://brandori.app/u/raynerlim](https://brandori.app/u/raynerlim)
*   **Author:** Rayner Lim ([LinkedIn](https://www.linkedin.com/in/raynerlim/))

---

## 10. License & Intellectual Property

© 2026 Rayner Lim. All rights reserved.

This repository, documentation, architecture diagrams, and associated showcase materials are proprietary and published solely for demonstration and recruitment evaluation purposes. No license is granted to copy, reproduce, fork, redistribute, or build derivative commercial products from this project without explicit written permission from the author.
