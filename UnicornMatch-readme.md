UnicornMatch Resume Analysis Tool

Live site: https://www.unicornmatch.app/

UnicornMatch is a resume analysis and job match evaluation tool created to help jobseekers understand how well their resume fits a selected role. It provides structured scoring, breakdowns and improvement suggestions supported by a controlled large language model workflow.

This document outlines the product intent, system design, user experience and technical choices used to build the platform. Redacted examples and non sensitive artifacts are included to demonstrate approach and thinking.

1. Problem Statement

Jobseekers often do not know how well their resume aligns with a job description. Recruiters and hiring managers spend time screening mismatched resumes without a consistent framework.

Key issues observed:

Applicants waste hours rewriting resumes without clear guidance.

Recruiters receive high volume but low relevance submissions.

Most resume tools only provide keyword checks and not structured analysis.

UnicornMatch solves this by providing a fast and objective analysis that reflects how real recruiters screen resumes.

2. Product Goals

Give jobseekers a clear, structured understanding of their resume quality.

Provide practical improvement suggestions they can act on immediately.

Create a scoring and feedback system that simulates recruiter style evaluation.

Reduce guesswork and help users identify gaps in both content and presentation.

Deliver a smooth and friendly user experience without technical friction.

3. System Overview

The platform combines structured scoring layers with controlled LLM logic.

The engine processes:

Resume text

Job description text

Context and user selections

Internal scoring framework

Structured prompts for evaluator consistency

Output includes:

Overall match score

Breakdown across core categories

Strengths

Gaps and opportunities

Targeted rewrite suggestions

A multi layer safety and validation step ensures stability when users upload messy resumes or incomplete job descriptions.

4. Key Features
Structured Scoring Engine

A customised evaluation system that measures:

Relevance to role

Skills and competencies

Experience alignment

Clarity and readability

Role specific indicators

Controlled LLM Evaluation

Rule based prompt design ensures:

Consistent scoring

No hallucination of nonexistent skills

Stable output formats

Defensive checks when inputs are weak

User Friendly Interface

Simple upload and paste workflow

Clear progress state

Clean results page with scannable sections

Messaging written for jobseekers rather than technical users

Brand and Voice

UnicornMatch is designed to feel supportive and encouraging, turning a stressful process into one with clarity and guidance.

5. Architecture Summary (Redacted)
User Upload
   -> Text Extraction
   -> Preprocessing
   -> Scoring Framework
   -> LLM Evaluation
   -> Quality Check
   -> Result Assembly
   -> Frontend Display


Supporting components include:

Input validators

Normalisation utilities

Error handling layer

Prompt control modules

Caching for cost optimisation

6. Sample Artifacts

These are safe for a public repo. You can add real files later.

flow-diagram.png

scoring-framework-overview.md

redacted-prompt-structure.md

ui-mockups.png

processing-pipeline-notes.md

7. Why This Project Matters

UnicornMatch connects multiple strengths:

Product thinking

Human behaviour understanding

AI system design

Structured evaluation systems

Branding and communication

UX for real users under stress

It is both a consumer facing product and a technical demonstration of controlled model based scoring.

8. Access to Full Implementation

The core engine, scoring logic and production prompts are stored in private repos to protect IP. Hirers who wish to review deeper technical details can request temporary access.
