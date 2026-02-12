# 💼 Business Requirements Analyst Skill

<p align="center">
  <img src="https://img.shields.io/badge/Claude-AI_Skill-blue?style=for-the-badge&logo=anthropic" alt="Claude Skill">
  <img src="https://img.shields.io/badge/Standard-BABOK_%2F_EARS-green?style=for-the-badge" alt="Standards">
  <img src="https://img.shields.io/badge/Architecture-ADR_Ready-orange?style=for-the-badge" alt="ADR Ready">
</p>

A powerful, professional AI Agent designed to transform vague business ideas into **production-ready documentation**. Optimized for AI-assisted development (SDD), this skill bridges the gap between stakeholders and engineers.

---

## 🌟 Key Smart Features

- **🧠 Smart Discovery**: Analyzes previous context and uploaded files (PDFs, images) to avoid redundant questions.
- **🧭 Process Transparency**: Presents a clear 6-phase roadmap at the start of every project.
- **📐 EARS Syntax**: Functional requirements formatted to eliminate ambiguity.
- **📊 Auto-ERD**: Automatic entity extraction and **Mermaid.js** diagram generation.
- **📝 ADR Management**: Capture the "Why" behind technical decisions.
- **🔗 Integrated Traceability**: Cumulative tagging from Business Goal to Code.
- **⚖️ Quality Gates**: Automatic maturity scoring (0-100%) for development readiness.

---

## 🛠️ What it Generates

The final output is a **Master Requirements Document** including:

1. **Business Vision** (Problem/Value/Growth)
2. **Processes** (BPMN/Flows)
3. **UX/UI Specs** (Mockups/Interactions)
4. **Functional Specs** (EARS Pattern)
5. **Non-Functional** (SLA/Security)
6. **Data Model** (ERD + Attributes)
7. **Integrations** (API Contracts)
8. **ADR** (Architecture Decisions)
9. **Traceability Matrix**
10. **Roadmap** (MVP & Scoping)

---

## 🚀 Realistic Usage Example

**User:** "I want to build a food delivery app for local chefs. Here is a PDF with my idea."

🤖 **Claude (with Skill):**

> "Hello! I'm your Business Requirements Analyst. To turn your idea into a development-ready blueprint, we'll follow this roadmap: **Discovery → Vision → Processes → Specs → Data Model → Roadmap.**
>
> I've analyzed your PDF. I see you want to focus on 'Home Chefs' to reduce costs. I've already captured the 3 main user types. To complete the **Discovery Phase**, I only need to know:
>
> 1. How will the commission be split between the chef and the platform?
> 2. Do we need a specific ADR for the payment provider?"

---

## 📖 Live Example: FastBite (Food Delivery)

Here is a preview of the quality you can expect:

### 3. Functional Requirements (EARS Syntax)

| ID        | Requirement                                                                             | EARS Pattern      | Traceability    |
| --------- | --------------------------------------------------------------------------------------- | ----------------- | --------------- |
| **FR-01** | **WHEN** the user confirms the order **THE** System shall initiate the payment process. | Event-Driven      | @brd: BRD.01.01 |
| **FR-02** | **IF** the payment fails **THEN** the System shall notify the user and allow a retry.   | Unwanted Behavior | @brd: BRD.01.05 |

### 4. Data Model (Auto-generated ERD)

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    RESTAURANT ||--o{ ORDER : receives
    ORDER ||--|| PAYMENT : has
    USER { string id PK, string name }
    ORDER { string id PK, float total }
```

### 5. Architecture Decisions (ADR)

- **ADR-001:** Use **PostgreSQL** for transactional integrity.
- **ADR-002:** Integrate **Stripe** for out-of-the-box PCI compliance.

---

## 📂 Project Structure

- **`SKILL.md`**: The brain of the agent. Methodology and logic.
- **`references/`**: Technical deep-dives.
  - `ears-methodology.md`: The standard for writing requirements.
  - `adr-template.md`: How to document architectural choices.
  - `traceability-guide.md`: Business-to-code connection.
  - `data-modeling.md`: Database patterns.
- **`assets/`**:
  - `requirements-template.md`: The master template with Quality Gates.

---

## 🎯 Target Audience

- **Founders**: Validate and scope your MVP.
- **Product Managers**: Generate PRDs in minutes.
- **Developers**: Receive clear, non-ambiguous specifications.

---

<p align="center">
  <i>"Don't just gather requirements. Engineer them."</i>
</p>
