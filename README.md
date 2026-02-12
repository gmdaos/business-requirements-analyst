# 📋 Business Requirements Analyst - Claude AI Skill

> Complete professional methodology for software and business requirements gathering.

<p align="center">
  <img src="https://img.shields.io/badge/Claude-AI_Skill-blue?style=for-the-badge&logo=anthropic" alt="Claude Skill">
  <img src="https://img.shields.io/badge/Standard-BABOK_%2F_EARS-green?style=for-the-badge" alt="Standards">
  <img src="https://img.shields.io/badge/Architecture-ADR_Ready-orange?style=for-the-badge" alt="ADR Ready">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge" alt="License">
</p>

---

## 🎯 What is this Skill?

**Business Requirements Analyst** is a Skill for Claude AI that implements a professional requirements gathering methodology, combining:

- ✅ **Business Analysis (BABOK)** - Professional business analysis standards.
- ✅ **Design Thinking** - User-centered idea validation.
- ✅ **Lean Startup** - Rapid vision and MVP validation.
- ✅ **SDD (Specification-Driven Development)** - AI-optimized technical documentation using EARS and ADR.

### What does it produce?

A **master requirements document** that includes:

1. 📌 **Business Vision** (Problem, value proposition, KPIs).
2. 👥 **Stakeholders & Personas**.
3. 🔄 **Business Processes** (Detailed flows).
4. 📐 **Functional Requirements (EARS Syntax)** - FR-001, FR-002...
5. 🚀 **Non-Functional Requirements** (Performance, security, scalability).
6. 📊 **Data Model (Auto-ERD)** - Entites, relationships, and Mermaid diagrams.
7. 🔌 **Integrations** (APIs, external services).
8. 📝 **Architecture Decision Records (ADR)**.
9. 🔗 **Traceability Matrix** (Cumulative tagging from business to code).
10. ⚖️ **Quality Gates** (Automated maturity scoring).
11. ⚠️ **Risks & Assumptions**.
12. 🗺️ **Roadmap in Phases** (MVP, Phase 2, Phase 3...).

---

## 🌟 Professional Enhancement (v2.0)

This version includes advanced automation and traceability features inspired by the `aidoc-flow-framework`:

- **🧠 Smart Discovery**: Automatically scans provided documents, images, and previous context to avoid repetitive questions.
- **📐 EARS Methodology**: Standardized syntax to eliminate ambiguity in functional requirements.
- **� ADR Support**: Document the "Why" behind critical technical choices.
- **🔗 Cumulative Traceability**: Embedded tags that create an audit trail from the Business Goal to the implementation code.
- **📊 Mermaid.js Integration**: Automatically generates visual Entity-Relationship Diagrams (ERD).

---

## 🚀 Installation

### Option 1: NPX (Recommended)

```bash
npx skills add gmdaos/business-requirements-analyst
```

### Option 2: Manual Installation

#### In Claude Code:

```bash
# Clone the repository
git clone https://github.com/gmdaos/business-requirements-analyst.git

# Copy to your project
cp -r business-requirements-analyst/business-requirements-analyst .agent/skills/
```

#### In Claude.ai:

1. Download the [business-requirements-analyst.skill](https://github.com/gmdaos/business-requirements-analyst/releases/latest) file.
2. Go to **Settings** → **Features** → **Skills**.
3. Click on **Upload Custom Skill**.
4. Select the `.skill` file.

#### In Claude API:

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# Upload the Skill
with open("business-requirements-analyst.skill", "rb") as f:
    skill = client.skills.create(file=f)

# Use in conversation
message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    extra_headers={
        "anthropic-beta": "code-execution-2025-08-25,skills-2025-10-02,files-api-2025-04-14"
    },
    tools=[{
        "type": "code_execution_2025_08_25",
        "container": {
            "skill_id": "business-requirements-analyst"
        }
    }],
    messages=[{
        "role": "user",
        "content": "Help me document my project"
    }]
)
```

---

## 💡 Usage

### Automatic Activation

The Skill activates automatically when:

- You mention "requirements gathering".
- You upload documents or images for analysis.
- You ask to "document a new project".
- You request "technical specifications" or "ADR".
- You need to "validate a business idea".

### Conversation Example

**👤 User:**
"I need to document my food delivery app for home chefs. Here is a PDF with the concept."

🤖 **Claude (with active Skill):**
"Great! I'm your Business Requirements Analyst. To turn your idea into a production-ready blueprint, we'll follow this roadmap: **Discovery → Vision → Processes → Specs → Data Model → Roadmap.**

I've analyzed the PDF you uploaded. I've already captured the problem statement and the 3 types of users. Let's focus on the gaps:

1. What specific business rules apply to the delivery fee calculation?
2. Are there any specific architecture decisions (ADR) we should document regarding the payment gateway?
   ..."

---

## 📖 Live Example: FastBite (Food Delivery)

### Functional Requirements (EARS Syntax)

| ID        | Requirement                                                                             | EARS Pattern      | Traceability    |
| --------- | --------------------------------------------------------------------------------------- | ----------------- | --------------- |
| **FR-01** | **WHEN** the user confirms the order **THE** System shall initiate the payment process. | Event-Driven      | @brd: BRD.01.01 |
| **FR-02** | **IF** the payment fails **THEN** the System shall notify the user and allow a retry.   | Unwanted Behavior | @brd: BRD.01.05 |

### Data Model (Mermaid ERD)

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    RESTAURANT ||--o{ ORDER : receives
    ORDER ||--|| PAYMENT : has
    USER { string id PK, string name }
    ORDER { string id PK, float total }
```

### Architecture Decision (ADR)

- **ADR-001:** Use **PostgreSQL** for transactional integrity.
- **ADR-002:** Integrate **Stripe** for out-of-the-box PCI compliance.

---

## 📚 Skill Content

### Main Files

- **`SKILL.md`** - Core methodology and logic.
- **`references/`** - Detailed technical deep-dives.
  - `ears-methodology.md` - Standard for non-ambiguous requirements.
  - `traceability-guide.md` - Connecting business to implementation code.
  - `adr-template.md` - Format for recording architectural decisions.
  - `process-mapping.md` - BPMN and flow techniques.
  - `data-modeling.md` - Patterns and normalization.
  - `use-cases.md` - Detailed use cases with templates.
- **`assets/`** - Templates and resources.
  - `requirements-template.md` - Master template with **Quality Gates**.
- **`examples/`** - Reference projects.
  - `food-delivery-app.md` - Complete documentation example.

---

## 🎓 Included Methodologies

### 1. Design Thinking

For idea validation and discovery:

- Empathize with users.
- Define the problem.
- Ideate solutions.
- Prototype and test.

### 2. Business Analysis (BABOK)

For professional documentation:

- Business requirements.
- Functional & Non-Functional requirements.
- Business rules.
- Stakeholders and processes.

### 3. Lean Startup

For quick vision and validation:

- Business Model Canvas.
- MVP (Minimum Viable Product) definition.
- Key metrics.

---

## 📖 Use Cases

### ✅ Ideal for:

- 🚀 **Startups** - Validate and document business ideas.
- 💼 **Consultants** - Create professional specs for clients.
- 👨‍💻 **Developers** - Understand scope before quoting.
- 🏢 **Product Managers** - Document product roadmaps.
- 💰 **Investors** - Evaluate project feasibility.

### 📋 Project Types:

- E-commerce / Marketplaces
- SaaS / Web Platforms
- Mobile Apps
- Enterprise Systems (ERP, CRM)
- APIs and Microservices

---

## 🛠️ Requirements

### For Claude Code:

- Claude Code installed.
- Project with `.agent/skills/` structure.

### For Claude.ai:

- Pro, Max, Team, or Enterprise plan.
- Code execution enabled.

### For Claude API:

- Anthropic API Key.
- Beta headers enabled (`code-execution`, `skills`, `files-api`).

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository.
2. Create a branch: `git checkout -b feature/new-feature`.
3. Commit changes: `git commit -m 'Add new feature'`.
4. Push: `git push origin feature/new-feature`.
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

Developed by [Gabriel Mayon](https://github.com/gmdaos)

Based on:

- **BABOK** (Business Analysis Body of Knowledge)
- **Design Thinking** (IDEO, Stanford d.school)
- **Lean Startup** (Eric Ries)
- **EARS** (Easy Approach to Requirements Syntax)

---

## 📞 Support

- 🐛 **Issues**: [GitHub Issues](https://github.com/gmdaos/business-requirements-analyst/issues)
- 📧 **Email**: gmgmdaos@gmail.com

---

## 🔗 Useful Links

- [Claude Skills Documentation](https://docs.anthropic.com/claude/docs/skills)
- [Claude API Reference](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [BABOK Guide](https://www.iiba.org/career-resources/a-business-analysis-professionals-foundation-for-success/babok/)

**⭐ If this Skill was useful, give it a star on GitHub!**
