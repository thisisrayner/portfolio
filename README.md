# Engineering Portfolio & Systems Architecture

**Author:** Rayner Lim  
**Role:** Solo Full-Stack Software Architect & Product Builder  
**Connect:** [LinkedIn](https://www.linkedin.com/in/raynerlim/) · [Website](https://hypasignal.com)

Welcome. This repository serves as the public technical portfolio and architectural showcase of platforms I have designed, engineered, and shipped end to end as a solo builder.

Each dossier provides a deep dive into problem space framing, data contracts, distributed subsystems, database schema topology, and operational reliability benchmarks.

---

## 🚀 Flagship Systems Showcase

| System | Domain & Focus | Core Tech Stack | Scale & Moat | Technical Dossier |
| :--- | :--- | :--- | :--- | :---: |
| **EmpathAIc** | **Sovereign Clinical Safety & Triage Engine**<br/>On-premise air-gapped clinical intelligence platform resolving demographic polysemy, covert suicide preparation, and zero-hallucination crisis escalation. | Next.js 15, React 19, TypeScript, Python 3.10+, Supabase Postgres, Tailwind, Docker, NDJSON Streaming | **~35,000 LoC**<br/>100% on-premise air-gap (zero WAN egress), Decoupled Dual-Head architecture, C-SSRS Level P Sentinel, Sympathetic Dreaming Realm (SDR) | [**Read Case Study**](./EmpathAIc-readme.md) |
| **HypaSignal** | **B2B Intent Intelligence SaaS**<br/>Live commercial SaaS discovering high-intent leads via real-world buying signals rather than static contact databases. | React 19, TypeScript, PostgreSQL (RLS), 90+ Deno Edge Functions, Stripe, SMTP, pg_cron | **~164,000 LoC**<br/>390+ migrations, 50 pages, 3 locales, 12 CI health watchdogs | [**Read Case Study**](./HypaSignal-readme.md) |
| **Brandori** | **Narrative Showcase Web Platform**<br/>Case study portfolio network bridging high-craft visual design with strategic metrics and interactive physics. | React 19 SPA, Vite 6, TypeScript, TanStack Query v5, Tailwind, Pointer Capture API, PostgreSQL (RLS) | **~23,600 LoC**<br/>9 swappable layouts, 8 case study archetypes, 0ms cache-primed route transitions | [**Read Case Study**](./Brandori-readme.md) |
| **Shadee Mosaic** | **Clinical Narrative Intelligence Platform**<br/>Semantic research data lake and psychiatric narrative exploration engine *(incorporates D.O.R.A.)*. | Python 3.10+, FastAPI, PostgreSQL 16 + `pgvector`, LLM API, GitHub Actions, Docker | **~16,000 LoC**<br/>**260,000+ vector embeddings**, ~3,800 posts/day ingestion, `< 450ms` IVFFlat search | [**Read Case Study**](./ShadeeMosaic-readme.md) |
| **Shadee.Care Writer** | **Clinical Editorial Suite & Research Co-Pilot**<br/>Specialized workspace pairing live web scraping and 0–5 relevance verification with 5 narrative archetypes and youth safety rails. | Python 3.10+, Streamlit, LLM API, Google CSE, BeautifulSoup4, WordPress REST | **~2,625 LoC**<br/>Up to 30 citations evaluated/topic, $\ge 3/5$ relevance gate, 24-hr session auth | [**Read Case Study**](./ShadeeWriter-readme.md) |
| **UnicornMatch** | **Resume Analysis & Recruiter Evaluation SaaS**<br/>Live commercial SaaS evaluating resumes against JDs using an LLM, featuring The Council of HRRR and AI fingerprint sanitization. | React 19, TypeScript, Vite, Tailwind, PostgreSQL (RLS), 14+ Deno Edge Functions, Stripe | **~20,250 LoC**<br/>26 migrations, 5 Council HR personas, in-browser PDF/DOCX parsing, atomic credit billing | [**Read Case Study**](./UnicornMatch-readme.md) |

---

## 🏛️ Engineering Disciplines & Architectural Principles

Across all projects, my engineering is guided by five foundational tenets:

### 1. Database-Centric Integrity & Row Level Security (RLS)
PostgreSQL is treated as the single source of truth, queue, and security perimeter. Rather than letting business logic sprawl across uncoordinated microservices:
* **Authorization lives in the database:** Tables are guarded by explicit RLS policies, ensuring client-side queries cannot leak cross-tenant records.
* **Queues as state machines:** Long-running asynchronous tasks (lead scoring, PII vectorization, email sequences) are modelled as database queue tables with status transitions, making jobs atomic, retryable, and observable.

### 2. High-Fidelity, Cost-Conscious AI Engineering
Rejecting thin, brittle API wrappers in favor of grounded, production-hardened AI topologies:
* **Demographic & Contextual Polysemy:** Calibrating prompts to evaluate language based on inferred cohorts (e.g., distinguishing adolescent colloquial hyperbole from genuine adult crisis).
* **Multi-Stage Tiered Triage:** Filtering noisy internet data through high-throughput regex and heuristic gates before triggering LLM calls, keeping monthly inference budgets in the low double digits.
* **Failure as a First-Class State:** Designing explicit fallbacks for model timeouts, rate limits, and JSON hallucination.

### 3. Production Vector Storage & Retrieval at Scale
Real-world vector search requires balancing build times, memory constraints, and query latency:
* Handled over **260,000+ 768-dimensional float embeddings** using tuned `IVFFlat` vector indexes ($\sqrt{N}$ centroid distribution), maintaining sub-500ms P95 query times without exhausting server RAM.

### 4. Defensive Reliability & Exception-Driven Alerting
* Ingestion pipelines feature exponential backoffs with random jitter to protect against brittle upstream API changes.
* Swapped out noisy daily "all-clear" emails for automated **Sentinel Watchdogs** that stay silent when healthy, but dispatch diagnostic HTML emails with stack traces within seconds of database drops or API quota exhaustion.

---

## 🔒 Intellectual Property & Commercial Boundary

The source code repositories for these products are proprietary and commercially active. 

To balance technical transparency for recruiters and engineering leaders with intellectual property protection:
* **Included in dossiers:** End-to-end architecture diagrams, design decisions, data flow models, database schemas, and scale metrics.
* **Deliberately omitted:** Proprietary prompt chains, clinical few-shot examples, private scoring algorithms, and production database credentials.

*Live code walkthroughs, architecture reviews, or staging demonstrations can be scheduled upon request.*

---

## 📬 Contact & Links

* **LinkedIn:** [https://www.linkedin.com/in/raynerlim/](https://www.linkedin.com/in/raynerlim/)
* **Products:**
  * EmpathAIc: [https://empathaic.com](https://empathaic.com)
  * HypaSignal: [https://hypasignal.com](https://hypasignal.com)
  * Brandori: [https://brandori.app](https://brandori.app)
  * Shadee.Care: [https://shadee.care](https://shadee.care)
  * UnicornMatch: [https://www.unicornmatch.app/](https://www.unicornmatch.app/)
