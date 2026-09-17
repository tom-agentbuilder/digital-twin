# Tom's Digital Twin

A retrieval-grounded AI representation of my professional experience, working philosophy, decision-making style, and approach to talent.

## 💬 Live Demo

**[→ Talk to My Digital Twin](https://udify.app/chat/eYkw4JgXg0lKcfQT)**

Ask me about my career, recruitment experience, market expansion, AI projects, or how I approach talent and business problems.

---

## What I Built

I wanted to explore a simple question:

> Can a professional's experience, reasoning patterns, and working philosophy be represented as a grounded AI system rather than a static CV?

This Digital Twin combines three components:

- a **Long-Context Operating System** that defines how Tom thinks, works, and communicates;
- a **RAG Evidence Layer** containing concrete career facts, stories, metrics, projects, and examples;
- an **LLM** that combines identity context with retrieved evidence to generate grounded answers.

The goal is not to build another generic "AI recruiter".

It is to build a conversational representation of a real professional.

---

## Architecture

### Core design

**Operating System → who Tom is and how Tom thinks**

**Evidence / RAG → what actually happened**

**LLM → combines both to answer the question**

The principle behind the architecture is:

> **The Operating System tells the model how to think like Tom.  
> The Evidence tells it what actually happened.**

### Runtime flow

**User Question → Knowledge Retrieval → Hybrid Search → Reranking → Relevant Evidence → LLM → Answer**

The LLM receives:

- the Tom Operating System as system-level context;
- the user's question;
- retrieved evidence from the knowledge base.

---

## Why Separate Operating System and RAG?

A professional identity contains both stable and dynamic information.

The **Operating System** is intended to remain relatively stable. It defines:

- professional identity
- career arc
- worldview
- values
- decision-making
- working style
- communication style
- AI philosophy
- knowledge boundaries

The **Evidence Layer** contains concrete information that may need to be retrieved selectively:

- career facts
- specific achievements
- market-entry stories
- sourcing cases
- metrics
- projects
- interview examples
- third-party validation

This prevents vector retrieval from having to reconstruct an entire professional identity from isolated chunks.

---

## RAG / Knowledge Architecture

The primary knowledge base is intentionally structured around **Evidence Cards** rather than a raw CV or a large unstructured document.

Each Evidence Card is designed to function as a self-contained retrieval unit.

Examples include:

- Vietnam Market Build
- Central Asia / Uzbekistan Market Entry
- Xiaohongshu Sourcing Case
- BFAM Candidate Story
- Vantage Regional Scaling
- Career Timeline
- AI / Talent Mapper
- Competitor Intelligence
- Third-Party Reference

The current primary knowledge base contains **35 chunks**, with each evidence card preserved as a coherent unit.

---

## Chunking Strategy

### Chunking method

**General**

### Semantic boundary

Each Evidence Card is separated using:

`===END EVIDENCE CARD===`

This allows the retrieval system to preserve the context of a complete evidence unit rather than splitting a story across arbitrary paragraph boundaries.

### Current settings

| Setting | Value |
|---|---|
| Chunking | General |
| Maximum chunk length | 2,000 characters |
| Chunk overlap | 0 |
| Evidence boundary | `===END EVIDENCE CARD===` |

The 2,000-character limit is treated as a practical calibration choice rather than a universal optimum.

---

## Retrieval Strategy

### Hybrid Search

I use **Hybrid Search** because the knowledge base contains both:

- semantic concepts and ideas;
- exact terms such as company names, markets, projects, and metrics.

For example:

A semantic query such as:

> "How does Tom approach entering a new market?"

benefits from semantic retrieval.

A specific query such as:

> "What happened in Uzbekistan?"

or:

> "How many hires did Tom's team deliver?"

also benefits from lexical / keyword matching.

### Reranking

After initial retrieval, a rerank model is used to improve the relevance of the final evidence set.

Current rerank model:

**rerank-english-v3.0**

### Top K

**Top K = 5**

This is a calibration choice intended to balance:

- recall of relevant evidence;
- context quality;
- noise entering the LLM.

### Score Threshold

**Disabled**

During initial calibration, I preferred observing retrieval recall before introducing an aggressive threshold that might filter out useful evidence.

---

## Knowledge Coverage

The current knowledge base covers:

| Domain | Coverage |
|---|---|
| Career history | ✓ |
| Vantage / Regional TA | ✓ |
| Vietnam market entry | ✓ |
| Central Asia / Uzbekistan | ✓ |
| Hedge fund / asset management recruitment | ✓ |
| Recruitment methodology | ✓ |
| Leadership & recruiter development | ✓ |
| Candidate assessment | ✓ |
| AI / Digital Twin | ✓ |
| Talent intelligence | ✓ |
| Competitor intelligence | ✓ |
| Third-party reference | ✓ |

---

## Career Timeline

The Digital Twin uses a resume-grounded career chronology:

**2012–2014** — Telesales / Cold Calling

**Oct 2014–Mar 2015** — Huxley Associates  
Recruitment Consultant — Retail Banking Relationship Managers

**Sep 2015–Jun 2017** — Adecco Personnel  
Consultant — Finance & Accounting

**Jul 2017–Jun 2018** — Spring Professional Hong Kong  
Senior Consultant — Finance & Accounting

**Sep 2018–Sep 2019** — SM Human Capital  
Consultant — Banking & Finance

**Oct 2019–Jul 2022** — Achievers Recruitment  
Senior Consultant — Equity Investment

**Aug 2022–Mar 2024** — ALS International  
Senior Consultant — Financial Services Front Office

**Oct 2024–Present** — Vantage Markets  
Regional Talent Acquisition Lead

The broader career progression can be summarized as:

**Telesales / General Recruitment → Finance & Accounting → Financial Services / Hedge Funds → Regional In-house TA → AI-enabled Talent Intelligence**

---

## Selected Evidence

### Vietnam Market Build

Built the Vietnam hiring operation from an early one-person setup and prioritised revenue-generating sales hiring first, followed by supporting functions as the business matured.

Documented outcome:

- 56 professionals placed in one year
- 26 sales
- 30 non-sales

The hiring sequence later became a reusable model for Central Asia.

### Central Asia / Uzbekistan

Built talent intelligence and market maps for Central Asia, including unconventional sourcing approaches and competitor mapping.

The work supported:

- senior sales hiring;
- market-entry hiring;
- multilingual talent discovery;
- expansion into adjacent talent pools.

### Xiaohongshu Sourcing

For a highly specific Central Asia search involving Chinese-speaking talent, Tom moved beyond conventional recruiting platforms and explored Xiaohongshu as a talent community.

The case illustrates:

- unconventional sourcing;
- community discovery;
- candidate assessment;
- hiring-manager alignment.

### BFAM Candidate Story

For a difficult Financial Services search that had remained open for months, Tom used historical candidate data, market mapping, and updated LinkedIn information to identify a previously inactive candidate with relevant quant / operations experience and Python capability.

### Competitor Intelligence

One project with Global Marketing involved identifying an experienced individual connected to a key competitor and arranging a paid expert consultation.

The purpose was not purely recruitment. The work involved accessing relevant talent-market knowledge and turning a conversation into commercially useful, non-confidential market intelligence.

This reinforced the idea that Talent Acquisition can create value beyond candidate sourcing.

### Third-Party Validation

A written recommendation from a former ALS Managing Director independently described Tom in terms including:

- strong work ethic;
- commitment;
- timely project delivery;
- passion for recruitment;
- deep understanding of his recruitment specialisation;
- teamwork;
- honesty and integrity.

---

## AI Portfolio

The current AI portfolio focuses on two projects.

### 1. Digital Twin

A long-context + RAG system designed to represent:

- professional identity;
- career experience;
- decision-making;
- working philosophy;
- specific evidence and stories.

### 2. Talent Mapper

An AI-assisted recruitment intelligence workflow using:

**OpenCode + Free LLM + Apify**

The current implementation uses **no custom code**.

The purpose is to accelerate:

- talent research;
- competitor mapping;
- market intelligence;
- structured talent discovery.

The value is not in writing software for its own sake.

The value is in identifying a real recruitment problem, selecting the right AI / automation components, connecting them into a repeatable workflow, and evaluating the business usefulness of the output.

---

## Why Not a Generic AI Recruiter?

The purpose of this project is not to automate recruitment end-to-end.

The intended division of labour is:

**AI → information processing, research, parsing, pattern recognition, scalable workflows**

**Human → judgment, relationships, interpretation, persuasion, and business decisions**

This reflects a broader belief that AI should increase the leverage of a recruiter rather than simply reproduce existing recruiting processes.

---

## Grounding & Knowledge Boundaries

The Digital Twin is explicitly instructed not to invent:

- employers
- dates
- metrics
- projects
- achievements
- personal opinions
- technical implementation details

Specific factual claims should be grounded in retrieved evidence whenever available.

When the knowledge base does not contain enough information to support an answer, the intended behaviour is to acknowledge the limitation rather than fabricate a plausible response.

I do not claim zero hallucination.

The objective is:

> **Controlled uncertainty through grounding, explicit boundaries, and evaluation.**

---

## Evaluation

The current evaluation is structured manual testing.

Test categories include:

- exact factual retrieval;
- specific career stories;
- AI / project questions;
- cross-domain questions;
- unknown-answer questions;
- factual boundary tests;
- consistency with the Operating System.

### Example factual-boundary test

A question such as:

> "What was Tom's exact revenue contribution from the Vietnam market?"

should not produce an invented figure when the public knowledge base does not contain a specific number.

This is intentional.

---

## Design Trade-offs

### Why General chunking instead of Parent-Child?

The evidence is deliberately pre-structured into self-contained Evidence Cards.

Because each card already contains its own context, I do not need an additional parent document to reconstruct meaning.

Parent-Child retrieval would become more interesting if the knowledge base became substantially more hierarchical or if important context depended heavily on surrounding sections.

### Why Hybrid Search instead of Vector Search alone?

Because the knowledge base contains both semantic concepts and exact identifiers.

Hybrid retrieval gives the system access to both signals.

### Why Top K = 5?

Top K is treated as a calibration parameter.

Too few results can reduce recall.

Too many results can introduce irrelevant evidence and unnecessary context.

Five is currently a practical middle ground.

### Why no score threshold?

The initial goal was to understand retrieval recall before aggressively filtering results.

A threshold can be introduced later based on evaluation results.

---

## Public-Safe Design

This repository contains a deliberately curated public version of the Digital Twin.

Sensitive information is excluded, including:

- confidential commercial information;
- candidate personal information;
- private contact information;
- credentials and API keys;
- internal information that is not appropriate for public disclosure.

The objective is to make the architecture and reasoning behind the system inspectable without exposing information that should remain private.

---

## Repository Structure

Current repository structure:

- `README.md` — project overview and architecture
- `prompts/tom-operating-system.md` — public Operating System / system prompt
- `knowledge-base/` — public evidence layer
- `architecture/` — system design and retrieval documentation
- `evaluation/` — test cases and evaluation notes

---

## Source Material

The public implementation is based on a combination of:

- resume / documented career facts;
- first-person career stories;
- detailed professional examples;
- interview answers;
- third-party employer reference.

The goal is not to dump every piece of source material into the repository.

Instead, the knowledge is intentionally structured and curated for retrieval quality.

---

## Limitations

The current Digital Twin has several limitations:

- It only knows information represented in its approved knowledge base.
- Retrieval quality can vary between semantically similar experiences.
- The system cannot guarantee zero hallucination.
- The current evaluation is manual rather than a formal benchmark.
- The Digital Twin represents a curated snapshot of my professional experience rather than a complete representation of the person.
- Public knowledge is intentionally narrower than the full private knowledge base.

---

## What I Learned

The main lesson from this project is that a Digital Twin is not simply a chatbot with a large prompt.

The quality of the system depends heavily on knowledge architecture.

The current design can be summarized as:

**Identity → Operating System**

**Facts → Evidence**

**Retrieval → Hybrid Search + Reranking**

**Generation → LLM**

**Reliability → Grounding + Knowledge Boundaries**

**Validation → Evaluation**

---

## Live Demo

### → Talk to My Digital Twin

**https://udify.app/chat/eYkw4JgXg0lKcfQT**

If you have questions about my career, recruiting experience, market-entry work, AI projects, or how I approach business and talent problems, ask the Digital Twin directly.

---

## Current Status

**Working prototype / public demonstration**

The current implementation is intentionally simple:

**Dify Chatflow + Long-Context Operating System + Evidence RAG + Hybrid Retrieval + Reranking + LLM**

The system will continue to evolve as new evidence, projects, and evaluation results are added.
