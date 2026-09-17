````markdown
# Digital Twin — Architecture

## Overview

This project explores how a professional's experience, reasoning patterns, working philosophy, and concrete career evidence can be represented as a grounded conversational AI system.

The architecture separates **identity** from **evidence**.

> **The Operating System tells the model how to think like Tom.**  
> **The Evidence tells it what actually happened.**

The current implementation is built with Dify, combining a long-context Operating System, retrieval-based Evidence, hybrid search, reranking, and an LLM.

---

## Core Architecture

```text
                         USER
                           │
                           ▼
                  ┌──────────────────┐
                  │   User Question  │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ Knowledge        │
                  │ Retrieval        │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Hybrid Search   │
                  │                  │
                  │ Semantic +       │
                  │ Keyword Search   │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │    Reranking     │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ Relevant Evidence│
                  └────────┬─────────┘
                           │
               ┌───────────┴───────────┐
               │                       │
               ▼                       ▼
      ┌──────────────────┐    ┌──────────────────┐
      │ Operating System │    │  Evidence / RAG  │
      │                  │    │                  │
      │ Who Tom is       │    │ What happened    │
      │ How Tom thinks   │    │ Facts            │
      │ How Tom works    │    │ Stories          │
      │ How Tom speaks   │    │ Metrics          │
      └────────┬─────────┘    │ Projects         │
               │              │ Examples         │
               │              └────────┬─────────┘
               │                       │
               └──────────┬────────────┘
                          ▼
                  ┌──────────────────┐
                  │       LLM        │
                  │                  │
                  │ Synthesis +      │
                  │ Grounded Answer  │
                  └────────┬─────────┘
                           │
                           ▼
                         ANSWER
````

---

# 1. Operating System

## Purpose

The Operating System is the long-context layer of the Digital Twin.

It defines the stable characteristics that should remain available across conversations, including:

* professional identity;
* career arc;
* worldview;
* values;
* decision-making;
* working style;
* leadership philosophy;
* communication style;
* learning approach;
* AI philosophy;
* knowledge boundaries.

The Operating System is intentionally different from a resume.

A resume primarily answers:

> What did Tom do?

The Operating System answers:

> Who is Tom, how does he think, and how should the Digital Twin behave?

---

## 2. Evidence / RAG

The Evidence layer contains concrete, retrievable information.

Examples include:

* career facts;
* career timeline;
* specific achievements;
* market-entry stories;
* sourcing cases;
* recruitment examples;
* metrics;
* AI projects;
* Talent Mapper;
* competitor intelligence;
* interview examples;
* third-party validation.

The purpose of RAG is not to reconstruct Tom's entire identity.

Its purpose is to retrieve the **specific evidence** required to answer a question accurately.

For example:

```text
Question:
"Tell me about Tom's Vietnam market entry experience."

Retrieval:
→ Vietnam Market Build
→ Vietnam Hiring Sequencing
→ Vantage Regional Scaling

LLM:
→ combines retrieved evidence with the Operating System
```

---

# 3. Why Separate Identity and Evidence?

A professional identity is not the same thing as a collection of documents.

Many characteristics of Tom are stable across different experiences:

* Business First;
* Repeatable Systems;
* Efficiency;
* Respect;
* Intellectual Curiosity;
* Business + Systems + People;
* bottom-up systems thinking;
* high context → low control → high autonomy.

These concepts are better represented as persistent context.

Specific evidence, however, is more naturally retrieved.

For example:

```text
Identity:
"I prefer to understand the business before defining the hiring solution."

Evidence:
Vietnam market-entry case
Central Asia market mapping
BFAM candidate case
Competitor intelligence project
```

This separation allows the model to maintain a coherent representation of Tom while still grounding factual claims in evidence.

---

# 4. Knowledge Base Design

The Evidence layer is structured around **Evidence Cards**.

Each card represents a meaningful retrieval unit.

Examples:

```text
E00 — Canonical Career Timeline
E01 — Career Narrative / Current Direction
E02 — Career Arc
E03 — Vantage — Regional Scaling
E04 — Vietnam Market Build
E05 — Vietnam Hiring Sequencing
E06 — Central Asia / Uzbekistan Market Entry
E07 — Xiaohongshu Sourcing Case
E08 — BFAM Secret Candidate
E09 — Agency / Hedge Fund Specialization
E10 — Recruiter Development
E11 — Recruiter Onboarding
E12 — Remote Leadership Operating Model
...
```

Each Evidence Card is designed to contain enough internal context to stand on its own.

The intention is to avoid splitting a meaningful story across unrelated chunks.

---

# 5. Chunking Strategy

## Chunking Method

**General**

## Semantic Boundary

The primary knowledge base uses an explicit delimiter:

```text
===END EVIDENCE CARD===
```

This creates a deliberate boundary between evidence units.

The design principle is:

```text
One Evidence Card
        ↓
One semantic retrieval unit
```

rather than:

```text
Arbitrary paragraph
        ↓
Fixed-size fragment
        ↓
Meaning may be split across chunks
```

## Current Settings

| Setting              | Value                     |
| -------------------- | ------------------------- |
| Chunking method      | General                   |
| Maximum chunk length | 2,000 characters          |
| Chunk overlap        | 0                         |
| Evidence delimiter   | `===END EVIDENCE CARD===` |

The 2,000-character limit is treated as a practical calibration choice rather than a universal optimum.

If evaluation later shows that meaningful evidence is being split, the configuration can be adjusted.

---

# 6. Why General Chunking?

The primary evidence corpus is intentionally pre-structured into self-contained evidence cards.

For example:

```text
E04 — Vietnam Market Build
```

already contains the relevant context needed to understand the case.

Because of this structure, Parent-Child chunking is not required for the current corpus.

Parent-Child retrieval may become useful in the future if the knowledge base becomes significantly larger, more hierarchical, or more dependent on surrounding document context.

---

# 7. Retrieval Strategy

## Hybrid Search

The current retrieval method is **Hybrid Search**.

The knowledge base contains both semantic concepts and exact identifiers.

For example:

### Semantic query

> How does Tom approach entering a new market?

This benefits from semantic retrieval.

### Exact query

> What happened in Uzbekistan?

This may benefit from exact lexical matching for terms such as:

```text
Uzbekistan
Vietnam
ALS
Vantage
BFAM
Talent Mapper
```

Hybrid retrieval therefore combines:

```text
Semantic similarity
+
Keyword / lexical matching
```

---

# 8. Reranking

After the initial retrieval stage, a reranking model is used to reorder the candidate evidence according to relevance to the user's question.

Current reranking configuration:

```text
Rerank Model:
rerank-english-v3.0
```

The purpose of reranking is to improve precision when several Evidence Cards discuss related themes.

For example, a query about:

> market entry

could potentially match:

* Vietnam;
* Central Asia;
* Russia;
* Taiwan;
* general TA methodology.

Reranking helps prioritize the most directly relevant evidence.

---

# 9. Top K

Current setting:

```text
Top K = 5
```

Five is treated as a practical starting point rather than a fixed optimum.

The trade-off is:

```text
Too few results
→ lower recall

Too many results
→ more noise and unnecessary context
```

The value should therefore be calibrated through evaluation.

---

# 10. Score Threshold

Current setting:

```text
Score Threshold = OFF
```

The threshold is initially disabled so that retrieval recall can be observed without aggressively filtering potentially useful evidence.

Once a structured evaluation set exists, a threshold can be introduced and calibrated based on actual retrieval performance.

---

# 11. LLM Context Structure

The final LLM receives three important inputs:

```text
SYSTEM
→ Tom Operating System

CONTEXT
→ Retrieved Evidence

USER
→ User Question
```

Conceptually:

```text
┌───────────────────────────────┐
│ SYSTEM                        │
│ Tom Operating System          │
│                               │
│ Identity                      │
│ Philosophy                    │
│ Decision-making               │
│ Working style                 │
│ Communication                 │
│ Knowledge boundaries          │
└───────────────┬───────────────┘
                │
                +
                │
┌───────────────▼───────────────┐
│ CONTEXT                       │
│ Retrieved Evidence            │
│                               │
│ Facts                         │
│ Stories                       │
│ Metrics                       │
│ Projects                      │
│ Examples                      │
└───────────────┬───────────────┘
                │
                +
                │
┌───────────────▼───────────────┐
│ USER                          │
│ Current question              │
└───────────────┬───────────────┘
                │
                ▼
               LLM
                │
                ▼
             Answer
```

---

# 12. Knowledge Boundary

The Digital Twin is explicitly designed not to invent unsupported information.

The system should not fabricate:

* employers;
* dates;
* metrics;
* achievements;
* projects;
* personal opinions;
* technical implementation details.

When the knowledge base does not contain enough information to support an answer, the preferred behaviour is to acknowledge that limitation.

The system is therefore designed around:

> **Controlled uncertainty rather than false confidence.**

The objective is not to claim zero hallucination.

The objective is to reduce unsupported claims through:

1. structured knowledge;
2. evidence retrieval;
3. explicit system instructions;
4. evaluation.

---

# 13. Evidence Hierarchy

Different sources have different evidentiary roles.

## Tier 1 — Resume / Documented Facts

Used primarily for:

* dates;
* formal titles;
* career chronology;
* documented scope;
* documented achievements.

## Tier 2 — First-Person Evidence

Used for:

* career stories;
* reasoning;
* personal experiences;
* professional philosophy;
* lessons learned.

## Tier 3 — Third-Party Evidence

Used for:

* independent validation;
* character;
* work ethic;
* reliability;
* professional conduct.

For example, the ALS Managing Director reference independently describes Tom's work ethic, commitment, recruitment knowledge, teamwork, honesty and integrity.

## Tier 4 — Interview Answer Exemplars

Used to understand how Tom has previously articulated a particular answer.

These are useful for answer style and similar interview questions, but should not automatically override more authoritative factual sources.

---

# 14. Canonical Public Facts

The public Digital Twin intentionally uses disciplined factual wording.

Examples:

### Vantage

> Tom is a Regional Talent Acquisition Lead at Vantage Markets.

### Team Outcome

> Tom led TA across 7 countries, with the team / function delivering 200+ hires.

This should not automatically be rewritten as:

> Tom personally made 200+ placements.

### Current AI Portfolio

The current portfolio is primarily:

```text
Digital Twin
+
Talent Mapper
```

Talent Mapper uses:

```text
OpenCode
+
Free LLM
+
Apify
```

The current implementation uses no custom code.

---

# 15. Current AI Portfolio

## Digital Twin

A professional AI representation combining:

```text
Long-Context Operating System
+
Evidence / RAG
```

The objective is to represent both:

* how Tom thinks;
* what Tom has actually experienced.

## Talent Mapper

A recruitment-intelligence workflow using:

```text
OpenCode
+
Free LLM
+
Apify
```

The workflow is designed to automate and accelerate:

* talent research;
* competitor mapping;
* market intelligence;
* structured talent discovery.

The emphasis is on practical workflow orchestration rather than building custom software for its own sake.

---

# 16. Public-Safe Knowledge Design

The public implementation is intentionally curated.

Sensitive information is excluded, including:

* confidential commercial information;
* candidate personal information;
* private contact details;
* credentials;
* API keys;
* internal information that is not appropriate for public disclosure.

The public repository therefore represents a **public-safe subset** of the broader knowledge system.

---

# 17. Retrieval Testing

The first stage of evaluation is structured manual testing.

The test set should cover several classes.

## Exact Facts

Examples:

```text
How many hires did Tom's team deliver?
Across how many countries?
When did Tom join Vantage?
How long was Tom at ALS?
```

## Specific Stories

Examples:

```text
Tell me about Vietnam.
Tell me about the BFAM candidate.
Tell me about the Xiaohongshu sourcing case.
```

## AI / Project Questions

Examples:

```text
What is Tom's Digital Twin?
What is Talent Mapper?
How does Tom use AI?
```

## Cross-Domain Questions

Examples:

```text
Why is Tom different from a traditional recruiter?
How would Tom approach an HR-tech problem?
What type of company would suit Tom?
```

## Unknown / Boundary Questions

Examples:

```text
What was Tom's exact revenue contribution from Vietnam?
What was Tom's exact billing figure at ALS?
```

The desired behaviour is to avoid inventing answers when the public knowledge base does not support the claim.

---

# 18. Future Routing Layer

The current architecture starts deliberately simple.

A future version can introduce a lightweight **Question Classifier** before retrieval.

For example:

```text
User Question
      │
      ▼
Question Classifier
      │
      ├── Identity / Philosophy
      │       ↓
      │   Operating System
      │
      ├── Career / Facts
      │       ↓
      │   Evidence Retrieval
      │
      ├── Stories / Achievements
      │       ↓
      │   Evidence Retrieval
      │
      ├── AI / Projects
      │       ↓
      │   Evidence Retrieval
      │
      ├── Third-Party Validation
      │       ↓
      │   Reference Evidence
      │
      └── Cross-Domain
              ↓
          Broad Retrieval
```

The classifier is intended as a **routing layer**, not as an additional intelligence layer.

The first implementation prioritises validating retrieval quality before adding routing complexity.

---

# 19. Why This Architecture?

The architecture follows one basic principle:

```text
Stable identity
→ Long Context

Specific evidence
→ Retrieval

Routing
→ Lightweight classifier when needed

Generation
→ LLM

Reliability
→ Grounding + explicit boundaries + evaluation
```

The design deliberately avoids making the vector database responsible for representing the entire person.

---

# 20. Current Implementation

```text
Dify Chatflow
    │
    ├── User Input
    │
    ├── Knowledge Retrieval
    │      ├── Hybrid Search
    │      ├── Reranking
    │      └── Top K = 5
    │
    ├── LLM
    │      ├── Tom Operating System
    │      ├── Retrieved Context
    │      └── User Question
    │
    └── Answer
```

Current production candidate:

```text
LLM:
DeepSeek V4 Flash

Primary RAG:
Tom Primary Evidence

Chunking:
General

Maximum chunk length:
2,000 characters

Chunk overlap:
0

Retrieval:
Hybrid Search

Reranking:
rerank-english-v3.0

Top K:
5

Score Threshold:
Off
```

---

# 21. Design Philosophy

The system follows several practical principles:

### Business First

The architecture should solve the actual problem rather than add technology for its own sake.

### Simplicity Before Complexity

Validate retrieval and answer quality before introducing additional routing or orchestration.

### Evidence Over Claims

Specific claims should be grounded in retrievable evidence.

### Human Judgment Remains Important

The Digital Twin represents a curated professional system; it is not intended to make unsupported decisions or replace human judgment.

### Iterative Evaluation

Parameters such as chunk size, Top K, reranking and routing should be tuned based on actual evaluation results rather than assumed to be universally optimal.

---

# 22. Summary

The Digital Twin is deliberately built as two connected layers:

```text
                    DIGITAL TWIN
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
      OPERATING SYSTEM             EVIDENCE
             │                         │
      "How Tom thinks"          "What happened"
             │                         │
             └────────────┬────────────┘
                          │
                          ▼
                         LLM
                          │
                          ▼
                       ANSWER
```

The key idea is simple:

> **RAG retrieves Tom's evidence.
> Long context preserves Tom's identity.**

This separation makes the system easier to reason about, evaluate, maintain, and extend over time.

```
```
