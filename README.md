# llm_triangle_eval_skill
> A triangular multi-agent evaluation skill for Large Language Models, where a Worker, Leader, and Auditor collaboratively assess reasoning quality, factual correctness, and execution reliability through adversarial verification.


# Introduction
Triangular Evaluation Framework

> A systematic explanation of the design principles, practical methodology, and optimization path of the Triangular Evaluation Framework, providing a deployable solution for AI response quality assurance.


------------------------------------------------------------------------

## 1. Why Was This Method Proposed? Origin of the Idea

### 1.1 Background: Limitations of Single-Point Evaluation

When AI assistants (such as Qoder / Claude Code / OpenClaw) respond to
users, traditional **single-point evaluation** suffers from fundamental
weaknesses:

| Risk Type                  | Manifestation                                    | Consequence                                            | Frequency |
| -------------------------- | ------------------------------------------------ | ------------------------------------------------------ | --------- |
| **Self-evaluation bias**   | AI tends to overestimate its own quality         | Problems remain unnoticed, low-quality outputs persist | ~70%      |
| **Single perspective**     | Answers evaluated from only one viewpoint        | Critical flaws are missed                              | ~60%      |
| **Lack of stress testing** | No external challenge or questioning             | Logical and factual errors remain hidden               | ~45%      |
| **Subjective judgment**    | Evaluation standards are inconsistent and opaque | Users cannot trust evaluation results                  | ~55%      |

------------------------------------------------------------------------

### 1.2 Core Insight: From "Single Point" to "Triangle"

The triangular evaluation paradigm draws inspiration from human quality
assurance systems:

The triangular evaluation paradigm draws inspiration from established human quality-assurance systems:

** 🔺 Source 1: Academic Peer Review
Multiple reviewers independently evaluate papers before publication.
- Reviewer A questions → Reviewer B validates → Editor makes the final decision.
- Insight: Independent multi-role evaluation avoids single-perspective bias.
- Evidence: Double-blind peer review reduced error rates by ~40% in major journals.

** 🔺 Source 2: Stress Interviews
- Interviewers deliberately challenge candidates to test reasoning depth.
- Insight: Challenging questions expose hidden weaknesses.
- Applications: McKinsey consulting interviews, Google technical interviews.

** 🔺 Source 3: Audit Systems
- Independent auditors verify financial reports.
- Insight: Objective third-party validation.
- Principle: Independence as enforced by major auditing firms.

** 🔺 Source 4: Scientific Triangulation
- Research conclusions validated using multiple data sources.
- Insight: Cross-validation increases reliability.
- Methodology: Gold standard in qualitative research.

Three independent roles collaborate to reduce bias and increase
reliability.

------------------------------------------------------------------------

### 1.3 Theoretical Foundation: Wisdom of Crowds

When independent evaluators make judgments, aggregated decisions are
often more accurate than any single evaluator.

| Evaluation Mode           | Evaluators    | Perspective Diversity | Cost            |
| ------------------------- | ------------- | --------------------- | --------------- |
| Single self-evaluation    | 1 (A)         | Low                   | Low             |
| Dual evaluation           | 2 (A+B)       | Medium                | Medium          |
| **Triangular Evaluation** | **3 (A+B+C)** | **High**              | **Medium-High** |
| Multi-review              | 4+            | Very High             | High            |


------------------------------------------------------------------------

### 1.4 Three-Role Philosophy

                Role A (Worker)
                     /          \
                    /            \
                   /              \
            Role B              Role C
          (Leader)              (Auditor)
                   \              /
                    \            /
                     \          /
                   Final Evaluation

-   A ↔ B: Challenge relationship\
-   A ↔ C: Audit relationship\
-   B ↔ C: Balance and arbitration

------------------------------------------------------------------------

## 2. Effectiveness

### 2.1 Case Study

**Case 1: AI Research Report Evaluation** 

- **Scenario**: Generating a “Weekly AI Trending Projects” report

- **Initial Score**: 57/100
- **Issues Discovered**:
  - P0: Severe time inconsistency (claimed “last week” but included 2-month-old data)
  - P0: Marketing-style claims without evidence
  - P1: Non-transparent selection criteria

- **Improvements**:
  - Strict time filtering
  - Removal of promotional language
  - Explicit methodology disclosure

**Case 2: Code Review Recommendations**

- **Scenario**: Reviewing Python implementation against requirements

- **Initial Score**: 52/100
- **Issues Discovered**:
  - Missing search-node capability
  - Oversimplified classification logic

- **Improvements**:
  - Implemented search functionality
  - Adopted more advanced classification strategy

------------------------------------------------------------------------

### 2.2 Why It Works

#### Cognitive Diversity

| Role            | Cognitive Mode    | Focus              | Issues Found                    | Typical Question                          |
| --------------- | ----------------- | ------------------ | ------------------------------- | ----------------------------------------- |
| **B (Leader)**  | Critical thinking | “What is wrong?”   | Logical flaws, weak assumptions | “Where does this data come from?”         |
| **C (Auditor)** | System thinking   | “What is missing?” | Edge cases, blind spots         | “Have failure scenarios been considered?” |

Combined reasoning greatly increases issue discovery.

- **Synergy**
  - B detects surface problems → C identifies root causes.
  - B tests extreme cases → C validates general applicability.
  - B focuses on current errors → C anticipates future risks.


------------------------------------------------------------------------

#### Confidence Calibration

    Final Confidence =
    0.5 * Confidence_B +
    0.5 * Confidence_C

    if disagreement > threshold:
        trigger arbitration

Ensures important issues are not hidden by consensus bias.

------------------------------------------------------------------------

#### Traceability

Every evaluation score is evidence-based and auditable.

------------------------------------------------------------------------

### 2.3 Limitations

Not suitable for: - Simple factual queries - Highly subjective
questions - Ultra low-latency scenarios

Best for: - Technical design reviews - Research reports - Code
evaluation - Product analysis

------------------------------------------------------------------------

## 3. How to Use This Skill

### 3.1 Quick Start

Natural language trigger:
```
    Evaluate my answer
    Please review
    Triangle evaluation
```

Command trigger:
```
/triangle-eval Evaluate the above response
/triangle-eval Review the microservice architecture proposal
```
------------------------------------------------------------------------

### 3.2 Workflow

    User Question
        ↓
    LLM Answer
        ↓
    Trigger Evaluation
        ↓
    Agent B Challenge
        ↓
    Agent C Audit
        ↓
    Aggregation
        ↓
    Evaluation Report

- **Estimated latency**: 
  - Simple: 6--8s 
  - Medium: 8--12s 
  - Complex: 12--15s

------------------------------------------------------------------------

### 3.3 System Architecture

```
┌──────────────────────────────────────────────┐
│                User Layer                     │
│  User Question → Qoder Answer → Eval Trigger │
└──────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────┐
│              Orchestrator Layer              │
│ 1. Context collection                        │
│ 2. Invoke Agent B (Leader)                   │
│ 3. Invoke Agent C (Auditor)                  │
│ 4. Conflict resolution                       │
│ 5. Generate evaluation report                │
└──────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────┐
│              Evaluation Layer                │
│                                              │
│  Agent B (Leader)        Agent C (Auditor)   │
│  • Challenge analysis     • Independent audit│
│  • Stress testing         • Objective verify │
│  • Critical questioning   • Blind-spot find  │
│  • Final scoring          • Arbitration      │
└──────────────────────────────────────────────┘
```
