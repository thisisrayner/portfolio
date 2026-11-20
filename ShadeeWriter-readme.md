# Shadee.Care Editorial Studio: Youth Mental Health AI Editorial Suite & Clinical Research Assistant

Shadee.Care Editorial Studio is a specialized AI-powered editorial workspace and live research pipeline engineered to assist human writers in producing clinically safe, culturally grounded, and emotionally resonant youth mental health content. Operating as an intelligent co-pilot, the platform pairs real-time multi-source web research and automated relevance scoring with a multi-archetype narrative engine, social media adaptation studio, and strict crisis intervention safeguards.

I designed and engineered the platform end to end as a solo builder: prompt topology, two-tier LLM model routing, real-time scraping and evaluation pipelines, Streamlit multi-tab UI architecture, and persistent session authentication.

> **Source Code Notice:** The source repository is proprietary and private. This document details the platform's architectural topology, design decisions, research verification pipeline, and subsystem scale. Proprietary narrative prompts, clinical crisis calibration dictionaries, and few-shot tone examples are deliberately omitted (see Section 7).

[![Live Platform](https://img.shields.io/badge/Platform-shadee.care-rose?style=flat-square)](https://shadee.care)
[![Research Pipeline](https://img.shields.io/badge/Research-Live%20Scraping%20%26%20Scoring-indigo?style=flat-square)](#2-what-it-does)
[![Frameworks](https://img.shields.io/badge/Frameworks-5%20Archetypes-teal?style=flat-square)](#2-what-it-does)
[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-zinc?style=flat-square)](#10-license--intellectual-property)
[![Stack](https://img.shields.io/badge/Python%203.10-Streamlit%20%7C%20Gemini-amber?style=flat-square)](https://streamlit.io)

---

## 1. The Problem

Creating mental health resources for youth and young adults (ages 13–25) requires an exceptional balance between clinical accuracy, emotional safety, and cultural resonance. Editorial teams face three persistent engineering and operational bottlenecks:

*   **Clinical Tone Fragility & Vernacular Distance:** Traditional clinical articles read like dry diagnostic manuals or institutional brochures that alienate young readers. Conversely, unconstrained consumer LLMs produce superficial, overly cheerful advice ("*just practice gratitude!*") or adopt cringeworthy, patronizing slang that destroys credibility.
*   **Hallucination vs. Evidence Grounding:** Youth mental health guidance requires real psychological backing without clinical overclaiming. Off-the-shelf generative models routinely hallucinate non-existent academic papers, quote paywalled studies without verification, or ingest SEO spam and forum rants with zero factual validation.
*   **Crisis Escalation & Ethical Safety Rails:** When addressing sensitive themes (burnout, clinical depression, suicidal ideation, self-harm), automated content tools risk triggering acute distress or generating unsafe descriptions. Guardrails must enforce compassionate, non-intrusive crisis hotline integration and require strict human-in-the-loop sign-off before publication.

Shadee.Care Editorial Studio solves this by combining a real-time web research and citation verification engine with structured psychological narrative frameworks, delivering high-empathy drafts and verified briefings in minutes while preserving human editorial oversight.

---

## 2. What It Does

*   **Two-Tier AI Model Orchestration:** Dynamically partitions editorial tasks between a high-empathy Writer AI (`gemini-3-flash-preview` / `gemini-2.5-pro`) for nuanced prose synthesis and a lightweight Researcher AI (`gemini-3.5-flash-lite`) for rapid candidate filtering and citation analysis.
*   **Live Web Research & 0–5 Relevance Scoring:** Automatically harvests up to 30 candidate sources across live web queries; scrapes each source in real time; and runs AI relevance scoring against mental health research ontologies. Only sources scoring $\ge 3/5$ are admitted into the verified briefing ledger; paywalled, bot-blocked, or low-relevance links are discarded.
*   **Iterative Search Query Refinement:** If initial search passes yield low thematic diversity, the research engine detects knowledge gaps and fires targeted secondary queries to uncover clinical perspectives or lived-experience contexts.
*   **5 Canonical Narrative Frameworks (+ Creative Mode):** Eliminates repetitive formatting by allowing writers (or an automated AI selector) to apply distinct narrative structures:
    1.  *The Classic Reflective:* Older-sibling empathy, personal struggle, and relatable cultural metaphors.
    2.  *The Narrative Journey:* Narrative storytelling, turning points, and overcoming adversity.
    3.  *The Mentor's Guide:* Actionable coping steps, cognitive shifts, and empowerment.
    4.  *The Mythbuster & Reality Check:* Debunking hustle culture, social media perfection, and toxic positivity.
    5.  *The Candid Confessional:* Intimate late-night vulnerability addressing quiet loneliness and shame.
    6.  *The Laureate's Canvas:* Unlocked dynamic mode for evocative, literary essays.
*   **Social Media Adaptation Studio:** In a single generation pass, adapts long-form articles into platform-native micro-content:
    *   *Instagram:* Slide-by-slide carousel descriptions, captions, and curated hashtag clusters.
    *   *TikTok / Reels:* 0–3 second attention hooks, visual/action scene directions, and voiceover scripts.
    *   *Facebook / Community:* Conversational reflection prompts driving peer discussion.
*   **Embedded Clinical Review Checklist:** Directly pairs every generated draft with an interactive editorial checklist (tone resonance, organic keyword integration, helpline verification) that writers must validate before release.
*   **Zero-Flicker 24-Hour Session Auth:** Custom authentication reading native session cookies via `st.context.cookies` with `SameSite=Lax` support, eliminating white-screen flickers on browser refresh.

---

## 3. Product Surface

| Subsystem | What the Writer / Editor Gets |
|---|---|
| **Editorial Draft Reader** | Rich Markdown reader with dynamic archetype indicators, real-time word count, reading time estimates, and markdown copy tools |
| **Research & Citations Ledger** | Verified bibliography displaying extracted quotes, domain authority indicators, 0–5 relevance scores, and direct source links |
| **Social Media Studio** | Tabbed multi-platform creator generating slide-by-slide Instagram carousels, TikTok scripts with visual cues, and Facebook discussion posts |
| **SEO & Internal Linking** | Automated tag generation and contextual recommendation of related articles within the `vibe.shadee.care` ecosystem |
| **Pre-Flight Review Checklist** | Interactive editorial safety audit ensuring suicide helpline presence, youth voice compliance, and clinical boundary validation |
| **Dynamic Framework Selector** | Dropdown controlling narrative archetypes with an automated "Let AI Decide" mode based on emotional topic analysis |
| **Session & Auth Manager** | Secure login portal with persistent 24-hour cookie persistence and role-based editorial access |

---

## 4. Architecture

```
   [ Editorial Topic / Theme Input ]
                  |
                  v
   +---------------------------------------------+
   |   Research Harvester & Live Search Engine   |
   |   - Google Custom Search API Queries        |
   |   - Candidate Extraction (up to 30 URLs)    |
   +---------------------------------------------+
                  |
                  v
   +---------------------------------------------+
   |   Real-Time Scraper & Gatekeeper            |
   |   - Headless Fetch & Text Extraction        |
   |   - Anti-bot / Paywall Detection            |
   +---------------------------------------------+
                  |
                  v
   +---------------------------------------------+
   |   Researcher AI (gemini-3.5-flash-lite)     |
   |   - 0–5 Relevance Scoring Matrix            |
   |   - Quality Gate: Discard if Score < 3/5    |
   |   - Thematic Gap Analysis & Re-query        |
   +---------------------------------------------+
                  |
                  | Verified Citations + Key Insights
                  v
   +---------------------------------------------+
   |   Writer AI (gemini-3-flash-preview / 2.5)  |
   |   - 5 Narrative Framework Prompt Modules    |
   |   - Youth Tone & Singlish/Cultural Rules    |
   |   - Crisis Helpline & Safety Gate Injection |
   +---------------------------------------------+
                  |
         +--------+--------+
         |                 |
         v                 v
   [ 450–650 Word    [ Social Studio:   
     Article Draft ]   IG / TikTok / FB ]
         |                 |
         +--------+--------+
                  |
                  v
   +---------------------------------------------+
   |   Editorial Studio UI (Streamlit Engine)    |
   |   - 4-Tab Interactive Workspace             |
   |   - Human-in-the-Loop Pre-Flight Checklist  |
   |   - One-Click Clipboard / CMS Export        |
   +---------------------------------------------+
```

### Key Architectural Decisions

*   **Live Web Scraping & Scoring over Static RAG:** Mental health discourse evolves alongside digital trends (e.g., algorithmic burnout, cyberbullying patterns, academic pressure spikes). Relying on a static vector database would leave writers with outdated context. Combining live search with real-time scraping and strict AI relevance scoring ensures up-to-the-minute citations without infrastructure bloat.
*   **Two-Tier Model Routing for Cost & Latency:** Evaluating 30 candidate articles using high-parameter frontier models causes unacceptable latency and API expense. By utilizing `gemini-3.5-flash-lite` for citation parsing and relevance scoring, the pipeline processes dozens of sources in under 5 seconds, reserving `gemini-3-flash-preview` exclusively for the final creative synthesis.
*   **Mandatory Human-in-the-Loop Pre-Flight Gate:** Fully autonomous content generation in clinical mental health is fundamentally irresponsible. The architecture deliberately requires human validation: drafts are paired with an interactive pre-flight checklist, ensuring no content reaches publication without human review.
*   **Native Cookie Session Architecture in Streamlit:** Streamlit re-executes its script on every interaction, which frequently causes session loss or UI flicker during state changes. Engineered custom cookie persistence via `st.context.cookies` and secure `SameSite=Lax` tokens, maintaining 24-hour persistent logins without third-party dependencies.

---

## 5. Tech Stack

| Layer | Choice |
|---|---|
| **UI & Workspace Application** | Python 3.10+, Streamlit (Wide-screen responsive layout, custom CSS theming) |
| **Writer AI Engine** | Google Gemini API (`gemini-3-flash-preview` / `gemini-2.5-pro`) |
| **Researcher AI & Scorer** | Google Gemini API (`gemini-3.5-flash-lite`) |
| **Search Engine Client** | Google Custom Search Engine (CSE) API with quota management |
| **Web Scraping & Parsing** | `requests`, `BeautifulSoup4`, regex fallback cleaners |
| **Data Normalization** | `pandas`, `pydantic`, JSON structured schemas |
| **Authentication & Sessions** | Streamlit native request context, secure browser cookie tokens |
| **CMS Publishing Integration**| WordPress REST API client (`vibe.shadee.care`) |

---

## 6. By the Numbers

| Metric | Figure |
|---|---|
| **Active Python Codebase** | **~2,625 lines of code** (Streamlit app, AI helpers, scraping modules) |
| **Candidate Source Harvest** | Up to **30 citations evaluated per topic** |
| **Citation Acceptance Gate** | Strict **$\ge 3/5$ relevance threshold** (auto-rejects paywalls and spam) |
| **Narrative Frameworks** | **5 distinct archetypes** + "Let AI Decide" + Creative Mode |
| **Social Adaptation Formats** | **3 native channels** (Instagram Carousel, TikTok/Reels Script, Facebook) |
| **Draft Generation Latency** | **< 18 seconds end-to-end** (search $\rightarrow$ scrape $\rightarrow$ score $\rightarrow$ draft $\rightarrow$ social) |
| **Session Lifetime** | **24-hour zero-flicker authenticated session** |

### Subsystems Breakdown

*   `app.py`: Streamlit application shell, dark indigo interface styling, tabbed editorial navigation, session state rehydration.
*   `utils/gemini_helper.py`: Research ingestion pipeline, live web scraping, 0–5 relevance evaluation, multi-query expansion logic.
*   `utils/gpt_helper.py`: Writer AI synthesis engine, 5 narrative framework prompt templates, crisis hotline rules, social studio formats.
*   `utils/search_engine.py`: Google CSE API client, query quota management, domain exclusion lists.
*   `utils/scraper.py`: Resilient web scraper with HTTP header rotation, anti-bot detection, and clean text extraction.
*   `utils/wordpress_helper.py`: Automated publishing bridge pushing validated drafts directly to WordPress staging.

---

## 7. What Is Not In Here, On Purpose

This repository is published for architecture and engineering review. It deliberately excludes:

*   Proprietary prompt chains and tone-calibration system instructions for the 5 narrative frameworks.
*   The clinical crisis dictionary and sensitive keyword escalation triggers used for suicide safety rails.
*   Few-shot examples encoding Shadee.Care's proprietary adolescent voice and regional cultural idioms.
*   Production API credentials, Custom Search Engine IDs, and WordPress publishing tokens.

For technical leaders or organizations interested in an end-to-end walkthrough or live demonstration, access can be arranged upon request.

---

## 8. What I Would Point You To

*   **Two-Tier Model Cost Optimization:** How dividing research evaluation from prose drafting slashes inference costs by ~75% while keeping end-to-end response times under 20 seconds.
*   **Resilient Citation Scoring Gate:** The real-time evaluation loop that tests, scrapes, and grades candidate URLs on a 0–5 scale, cleanly filtering out paywalls, bot-blockers, and SEO spam before LLM context injection.
*   **High-Craft Multi-Platform Adaptation:** Synthesizing an entire writer's package (article, Instagram slides, TikTok cues, SEO tags) in a single unified workflow without multi-turn context drift.
*   **Clinical Safety By Design:** The human-in-the-loop pre-flight checklist and non-intrusive crisis intervention rules ensuring ethical AI usage in youth mental health.

---

## 9. Links

*   **Shadee.Care Foundation:** [https://shadee.care](https://shadee.care)
*   **Youth Content Hub:** [https://vibe.shadee.care](https://vibe.shadee.care)
*   **Author:** Rayner Lim ([LinkedIn](https://www.linkedin.com/in/raynerlim/))

---

## 10. License & Intellectual Property

© 2026 Rayner Lim. All rights reserved.

This repository, documentation, architecture diagrams, and associated showcase materials are proprietary and published solely for demonstration and recruitment evaluation purposes. No license is granted to copy, reproduce, fork, redistribute, or build derivative commercial products from this project without explicit written permission from the author.
