# Shadee Mosaic: Clinical Narrative Intelligence & Semantic Research Platform

Shadee Mosaic is a production-grade clinical intelligence and semantic exploration platform built to analyze authentic lived-experience mental health narratives at scale. Operating over a proprietary vector-embedded data lake of **260,000+ patient and youth narratives**, the platform bridges the gap between raw, unstructured social discourse and rigorous psychiatric research through automated ingestion, clinical risk stratification, semantic vector search, and a recursive thematic saturation audit engine.

I designed and engineered the platform end to end as a solo developer: upstream multi-source ingestion funnels, data lake schema design, vector indexing optimization, PII sanitization pipelines, demographic AI classification, and the real-time research exploration dashboard.

> **Source Code Notice:** The source repository is proprietary and private. This document details the platform's architectural topology, design decisions, data engineering pipelines, and subsystem scale. Proprietary clinical prompts, demographic calibration weights, PII scrubbing regexes, and narrative triage heuristics are deliberately omitted (see Section 7).

[![Live Platform](https://img.shields.io/badge/Platform-shadee.care-rose?style=flat-square)](https://shadee.care)
[![Vector Corpus](https://img.shields.io/badge/Corpus-260k%2B%20Narratives-blue?style=flat-square)](#6-by-the-numbers)
[![Index](https://img.shields.io/badge/Index-IVFFlat%20(pgvector)-teal?style=flat-square)](#4-architecture)
[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-zinc?style=flat-square)](#10-license--intellectual-property)
[![Stack](https://img.shields.io/badge/Python%203.10-FastAPI%20%7C%20Postgres-amber?style=flat-square)](https://fastapi.tiangolo.com)

---

## 1. The Problem

Mental health researchers, clinicians, and youth support organizations struggle to capture the true, unvarnished lived experience of emotional distress. Traditional clinical surveys and academic questionnaires suffer from three fundamental limitations:

*   **Severe Social Desirability & Recall Bias:** In formal clinical settings or self-report surveys, individuals underreport acute distress, suicidal ideation, or stigmatized behaviors. Conversely, anonymous online communities (Reddit, Tumblr, YouTube lived-experience spaces) contain authentic, real-time expressions of distress that traditional instruments miss.
*   **Massive Signal-to-Noise & Colloquial Ambiguity:** Public social discourse is flooded with commercial spam, gaming hyperbole ("*this boss makes me wanna kms*"), sarcasm, and regional cultural slang (e.g., Singaporean/Southeast Asian vernacular: *sibei sian*, *cannot tahan*). Standard keyword scrapers or off-the-shelf sentiment models catastrophically fail at distinguishing benign hyperbole from genuine covert crisis preparation.
*   **Static Search vs. Thematic Saturation:** Standard keyword search retrieves isolated keywords without context. Qualitative clinical research requires **thematic saturation**—verifying that a sample of narratives genuinely captures all dimensions of an emotional stressor rather than cherry-picking an unrepresentative subset.

Shadee Mosaic solves this by continuously ingesting, triaging, and vectorizing community narratives into a high-fidelity semantic data lake, allowing researchers to explore clinical themes with sub-second response times and audit research findings for statistical confidence.

---

## 2. What It Does

*   **Multi-Source Automated Ingestion:** Scheduled daily funnels ingest ~3,800 records/day across Reddit, YouTube comments, Tumblr tags, and Google Trends within strict API rate-limit budgets, automatically deduplicating URLs.
*   **Demographically Calibrated AI Classification:** Enriches every ingested narrative into one of 17 canonical psychological categories, determines demographic cohorts (Youth 12–24 vs. Working Adults / Caregivers 25+), and assigns a 5-tier clinical risk score (`none`, `low`, `moderate`, `high`, `crisis`).
*   **Contrastive Edge-Case Disambiguation:** Disentangles lookalike edge cases—such as separating teenage hyperbole from covert pre-suicide behavior (giving away possessions, farewell notes) and distinguishing authentic dissociation/derealization (DPDR) from general anxiety.
*   **Sub-Second Semantic Retrieval (`< 500ms`):** Powered by an `IVFFlat` vector index over 260,000+ 768-dimensional embeddings, allowing researchers to execute conceptual similarity queries (e.g., *"sandwich-generation caregiver burnout with insomnia"*) across the entire multi-year corpus.
*   **Recursive Thematic Saturation Protocol:** A self-auditing research engine that dynamically samples narratives at incremental sample sizes ($N=100 \rightarrow N=500 \rightarrow N=1,000$), calculating thematic stability to assure researchers that a qualitative finding has reached saturation.
*   **Atmosphere Pulse:** Correlates semantic narrative clusters with quantitative Google Trends search frequency to track societal-level shifts in mental health interest over time.
*   **Sentinel Health & Downtime Watchdog:** An automated alert engine that monitors database connectivity, Gemini API quota/billing health, and scraper execution, dispatching emergency diagnostic emails within seconds of any pipeline degradation.

---

## 3. Product Surface

| Subsystem | What the Researcher / Operator Gets |
|---|---|
| **Semantic Brain Explorer** | Interactive research dashboard with real-time vector search, threshold sliders, demographic cohort filters, and Server-Sent Events (SSE) streaming synthesis |
| **Thematic Saturation Audit** | Recursive verification tab displaying topic stability, centroid distance deltas, and sample adequacy across $N=100 \rightarrow N=1000$ iterations |
| **Atmosphere Pulse (Trends)** | Real-time trend visualization mapping monitored clinical keyword clusters against longitudinal search interest |
| **Clinical Triage Workspace** | Audit view showing raw content, PII-scrubbed text, canonical bucket mappings, clinical risk justifications, and model confidence scores |
| **Session History Manager** | Persistent sidebar storing historical research queries, applied filters, and synthesis reports with one-click state rehydration |
| **Upstream Ingestion Funnels** | Precision scrapers running daily at 06:00 SGT with automated deduplication and zero Google Sheets dependency |
| **Sentinel Alert Watchdog** | Automated alerting engine monitoring Supabase write latency, Gemini API quota exhaustion, and scraper uptime |

---

## 4. Architecture

```
   [ Upstream Sources: Reddit API, YouTube API, Tumblr API, Google Trends ]
                                    |
                                    v
                     +------------------------------+
                     |  Scheduled Ingestion Layer   |
                     |  - Regex Triage & Filtering  |
                     |  - URL-hash Deduplication    |
                     |  - Exponential Backoff       |
                     +------------------------------+
                                    |
                                    v
                     +------------------------------+
                     |  Demographic AI Classifier   |
                     |  - 17 Narrative Buckets      |
                     |  - 5-Tier Clinical Severity  |
                     |  - Contrastive Disambiguator |
                     +------------------------------+
                                    |
                                    v
                     +----------------------------------------------+
                     |  Authoritative PostgreSQL + pgvector Lake   |
                     |  - 260,000+ Clinical Narratives              |
                     |  - 768-Dimensional Embeddings                |
                     |  - IVFFlat Vector Index (385 Lists)          |
                     |  - B-tree Filtered Indexes                   |
                     +----------------------------------------------+
                                    |
                                    |  Sub-second Vector Search
                                    v
                     +------------------------------+
                     |  FastAPI / Next.js Engine    |
                     |  - Singleton DB Architecture |
                     |  - Recursive Saturation Loop |
                     |  - SSE Streaming Synthesis   |
                     +------------------------------+
                                    |
                                    v
                     [ Researcher Workspace & Visual Explorer ]
```

### Key Architectural Decisions

*   **IVFFlat Indexing over HNSW for Massive Dynamic Datasets:** For high-volume ingestion (~3,800 rows/day), HNSW index build times and memory overhead degrade rapidly on constrained instances. Transitioned to an optimized `IVFFlat` index (385 centroids calibrated to $\sqrt{N}$), achieving sub-500ms P95 query times with low build overhead and predictable memory consumption.
*   **Two-Tier Triage Pipeline (Compute Optimization):** To avoid burning millions of LLM tokens on spam, data passes through a lightweight Tier-1 regex triage baseline to discard commercial artifacts and flag clear categories, reserving deep Gemini LLM classification strictly for nuanced, conversational text.
*   **Demographic Polysemy Calibration:** Language that signifies severe clinical crisis in an adult (e.g., financial retrenchment + multi-day insomnia) carries vastly different diagnostic implications when used in adolescent slang. The classification layer conditions its clinical risk evaluation on inferred life stages (`youth` vs. `middle_age`), preventing hyperbole false-positives.
*   **PostgREST Compatibility Layer:** Ingestion and classification communicate via a PostgREST abstraction layer, enabling seamless portability between managed cloud database providers and self-hosted Dockerized PostgreSQL without rewriting client code.
*   **Fail-Safe Sentinel Alerting:** Replaced routine daily "all-clear" emails with an exception-driven alert engine. The pipeline runs completely silent on success, but immediately triggers formatted SMTP alerts with stack traces upon database drops, API quota exhaustion, or scraper authentication errors.

---

## 5. Tech Stack

| Layer | Choice |
|---|---|
| **Core Backend & API** | Python 3.10+, FastAPI (Async, SSE Streaming, Singleton DB connection) |
| **Vector Database & Lake** | PostgreSQL 16 with `pgvector` extension, hosted on Supabase Pro / Docker |
| **Vector Indexing** | `IVFFlat` index with 385 lists, cosine distance operator (`<=>`) |
| **AI Models & Embeddings** | Google Gemini API (`gemini-3.5-flash-lite` for batch classification; `gemini-embedding-001` for 768d vectors; `gemini-2.5` for research synthesis) |
| **Data Pipelines & Scraping** | Python (`praw` for Reddit, `google-api-python-client` for YouTube, `pytumblr`, `pytrends`) |
| **Data Processing & Math** | `pandas`, `numpy`, `tenacity` (retry decorators with exponential jitter) |
| **Alerting & Monitoring** | SMTP / TLS Sentinel Engine with automated HTML diagnostic payloads |
| **Orchestration & CI/CD** | GitHub Actions (Cron schedules: 06:00 SGT ingestion, 07:00 SGT classifier, weekly master cloud) |
| **Visualization & Reporting** | `matplotlib`, `wordcloud`, Streamlit / Next.js exploration UI |

---

## 6. By the Numbers

| Metric | Figure |
|---|---|
| **Total Ingested Narratives** | **260,000+ authentic lived-experience records** |
| **Total Vector Embeddings** | **260,000+ 768-dimensional float arrays** |
| **Daily Ingestion Throughput** | **~3,700 – 3,950 posts/day** across Reddit, YouTube, and Tumblr |
| **Vector Search Latency** | **< 450ms P95** across filtered 260k+ corpus |
| **Active Codebase (Mosaic & Social Listen)** | **~16,000 lines of code** (Python, SQL, TypeScript/HTML, Workflows) |
| **Automated Workflows** | 5 active GitHub Actions cron pipelines |
| **Reliability Record** | **100% consecutive uptime** across scheduled daily runs |
| **Classification Accuracy** | 17 narrative buckets, 5-tier risk severity, demographic tagging |

### Subsystems

*   **Ingestion Funnels:** Automated daily collectors with rate-limit budgets, platform-specific date normalizers, and duplicate filtering.
*   **Classifier & Triage Engine:** 10-post batching harness with rate-limit pacing, 3-attempt backoff retries, and strict foreign-key normalization.
*   **PII Sanitizer & Data Moat:** Regex and semantic scrubbers that strip personal handles, names, phone numbers, and identifying URLs prior to embedding.
*   **Thematic Saturation Engine:** Multi-stage recursive query auditor checking topic stability and semantic convergence across expanding narrative cohorts.
*   **Sentinel Alert Engine:** Instantaneous diagnostic alert dispatcher tracking database write availability, Gemini token quotas, and scraper health.
*   **Master Visualization Engine:** Cloud generation pipeline synthesizing thousands of recent posts directly from the vector lake.

---

## 7. What Is Not In Here, On Purpose

This is a proprietary research platform and commercial data moat. The repository does not expose:

*   The proprietary Singlish, adolescent slang, and clinical distress lexicons used for localized triage.
*   Internal clinical prompt templates, few-shot examples, and demographic calibration weights.
*   The exact semantic convergence formulas and variance thresholds governing the Thematic Saturation Protocol.
*   Production database credentials, service role keys, and SMTP server authentication secrets.
*   Raw patient and author identifiers (the platform strictly adheres to ethical research protocols; raw handles are never stored or exposed).

For engineering leaders, research institutions, or recruiters interested in a live architectural demonstration or deeper technical walkthrough, access can be granted upon request.

---

## 8. What I Would Point You To

*   **Engineering for Fragile External APIs:** How the ingestion pipelines gracefully manage brittle third-party social APIs through jittered backoffs, pagination boundaries, and duplicate filtering without incurring rate-limit bans or data loss.
*   **Cost-Conscious AI Architecture:** Achieving high-accuracy clinical classification across millions of tokens using batched lightweight models (`gemini-3.5-flash-lite`) and multi-stage triage, keeping monthly API bills under double digits.
*   **Production Vector Database Discipline:** Migrating from naive in-memory or high-overhead HNSW structures to a tuned `IVFFlat` index with proper centroid distribution, maintaining sub-second query times over 260k+ rows.
*   **Defensive Reliability Engineering:** Building self-healing pipelines backed by the Sentinel Alert Engine, ensuring silent success when operations are normal but immediate notification with diagnostic traces upon external service degradation.

---

## 9. Links

*   **Platform Overview:** [https://shadee.care](https://shadee.care)
*   **Author:** Rayner Lim ([LinkedIn](https://www.linkedin.com/in/raynerlim/))

---

## 10. License & Intellectual Property

© 2026 Rayner Lim. All rights reserved.

This repository, documentation, architecture diagrams, and associated showcase materials are proprietary and published solely for demonstration and recruitment evaluation purposes. No license is granted to copy, reproduce, fork, redistribute, or build derivative commercial products from this project without explicit written permission from the author.
