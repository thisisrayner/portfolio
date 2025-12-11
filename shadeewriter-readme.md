Shadee.Care Writer Assistant

The Shadee.Care Writer Assistant is an AI powered content helper designed to support the creation of youth mental health articles for Shadee.Care. It guides writers with research summaries, structure suggestions, tone alignment and safe language practices while preserving Shadee.Care’s voice and mission.

This document outlines the purpose, system design, safety measures and example artifacts included in this public portfolio.

1. Problem Statement

Shadee.Care produces youth centric mental health articles that require sensitivity, accuracy and consistency. Writers often face challenges such as:

Choosing an appropriate structure for different topics

Maintaining a safe and supportive tone for readers aged 13 to 30

Avoiding repetitive formats across articles

Handling sensitive themes responsibly

Ensuring every topic includes pathways to real help when needed

The Writer Assistant was built to help writers create high quality articles faster without losing human judgment or empathy.

2. Product Goals

Give writers a structured starting point based on the chosen topic

Ensure articles are aligned with Shadee.Care’s voice and mental health principles

Provide tone guidance suitable for youth audiences

Suggest relevant examples, metaphors and scenarios

Embed light touch reminders to guide readers toward help when needed

Prevent unsafe or harmful outputs through multi layer safeguards

The Writer Assistant enhances writers rather than replacing them. The goal is supported creativity, not automated publishing.

3. System Overview

The Writer Assistant uses a controlled large language model with a custom instruction set tuned for Shadee.Care’s content needs.

Inputs processed:

Topic or theme

Preferred article style

Audience age range

Sensitivity indicators

Internal tone and safety rules

Outputs include:

A recommended structure

Paragraph level content suggestions

Tone guidelines

Optional analogies or story elements

A concluding message guiding readers toward self help or support channels

The assistant never publishes directly. Writers review, edit and refine the suggestions before any public release.

4. Key Features
Content Structure Selector

Three article structures were designed for variety and depth. The assistant chooses one based on topic suitability rather than generating all three.

Tone and Voice Alignment

The assistant keeps language supportive, relatable and youth friendly.

Light Touch Help Guidance

If the topic touches on sensitive areas, the assistant includes a short reminder paragraph encouraging readers to seek support, without embedding the full hotline list. This ensures the human writer adds context specific details.

Safety and Misuse Prevention

A two layer protection design:

Guardrail logic for detecting prompt attacks or attempts to force unsafe content

Strict compliance instructions that prevent harmful advice or factual errors in mental health contexts

Shadee.Care Context Awareness

The assistant includes:

Youth centric language

Alignment with the five Shadee.Care wellbeing pillars

Mention of Shadee.Care programs and resources at the end of each piece

5. Architecture Summary (Redacted)
User Input
   -> Topic Interpretation
   -> Structure Selector
   -> Tone and Safety Layer
   -> Content Generation
   -> Sensitivity Check
   -> Final Draft Assembly


Support components:

Prompt templates

Safety keyword blocks

Data validation

Output clean up layer

6. Sample Artifacts

These are safe for public GitHub display.

structure-selector-logic.md

tone-guidelines.md

safe-language-framework.md

redacted-main-prompt.md

sample-draft-output.md

writer-workflow-diagram.png

7. Why This Project Matters

The Writer Assistant demonstrates:

How AI can support mental health communication safely

A practical application of LLM prompt engineering for sensitive audiences

Controlled generation that respects organisational values

The combination of product design, behaviour insight and content strategy

Scalable systems for volunteer and youth content teams

This tool reduces writing friction and improves consistency while keeping human oversight central.

8. Access to Full Implementation

The full prompt set, safety layers and production logic exist in private repos because they contain proprietary IP and youth safety controls. Selected examples can be shared with hirers on request.
