# 📋 Business Requirements Analyst - Claude AI Skill

> Complete professional methodology for software and business requirements gathering.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude AI](https://img.shields.io/badge/Claude-AI%20Skill-blue)](https://claude.ai)

---

## 🎯 What is this Skill?

**Business Requirements Analyst** is a Claude AI Skill that implements a professional requirements gathering methodology, combining:

- ✅ **Business Analysis (BABOK)** - Professional business analysis
- ✅ **Design Thinking** - User-centered idea validation
- ✅ **Complete Documentation** - Ready-to-use templates

### What does it produce?

A **master requirements document** that includes:

1. 📌 Business Vision (problem, value proposition, KPIs)
2. 👥 Stakeholders and Personas
3. 🔄 Business Processes (detailed flows)
4. ⚙️ Functional Requirements (FR-001, FR-002...)
5. 🚀 Non-Functional Requirements (performance, security, scalability)
6. 📊 Data Model (entities, relationships)
7. 🔌 Integrations (APIs, external services)
8. ⚠️ Risks and Assumptions
9. 🗺️ Roadmap in Phases (MVP, Phase 2, Phase 3...)

---

## 🚀 Installation

### Option 1: NPX (Recommended)

```bash
npx skills add gmdaos/business-requirements-analyst-skill
```

### Option 2: Manual Installation

#### In Claude Code:

```bash
# Clone the repository
git clone https://github.com/gmdaos/business-requirements-analyst-skill.git

# Copy to your project
cp -r business-requirements-analyst-skill/business-requirements-analyst .agent/skills/
```

#### In Claude.ai:

1. Download the [business-requirements-analyst.skill](https://github.com/gmdaos/business-requirements-analyst-skill/releases/latest) file
2. Go to **Settings** → **Features** → **Skills**
3. Click on **Upload Custom Skill**
4. Select the `.skill` file

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

- You mention "requirements gathering"
- You ask to "document a new project"
- You request "technical specifications"
- You need to "validate a business idea"

### Conversation Example

```
👤 User:
"I need to document my food delivery app project"

🤖 Claude (with active Skill):
"Great, I'm going to help you with a complete requirements gathering
using the Business Analysis methodology.

Let's start with the business vision:

1. What specific problem does your app solve?
2. Who are your primary users?
3. What makes your app unique vs. competitors?
..."

[Claude guides you step-by-step]

🤖 Claude (at the end):
"I've generated your complete requirements document with:
- 15 functional requirements
- 8 non-functional requirements
- 3 mapped processes
- Data model with 8 entities
- 3-phase roadmap"
```

---

## 📚 Skill Content

### Main Files

- **SKILL.md** - Core methodology and usage guides
- **references/** - Detailed technical documentation
  - `process-mapping.md` - Process mapping techniques (BPMN, flows)
  - `data-modeling.md` - Data modeling, normalization, patterns
  - `use-cases.md` - Detailed use cases with templates
- **assets/** - Resources and templates
  - `requirements-template.md` - Complete ready-to-use template

---

## 🎓 Included Methodologies

### 1. Design Thinking

For idea validation and needs discovery:

- Empathize with users
- Define key problems
- Ideate solutions
- Prototype
- Test

### 2. Business Analysis (BABOK)

For professional documentation:

- Business requirements
- Functional and non-functional requirements
- Business rules
- Stakeholders and processes

### 3. Lean Startup

For quick vision and validation:

- Business model canvas
- MVP definition
- Key metrics

---

## 📖 Use Cases

### ✅ Ideal for:

- 🚀 **Startups** - Validate and document business ideas
- 💼 **Consultants** - Create specifications for clients
- 👨‍💻 **Developers** - Understand scope before quoting
- 🏢 **Product Managers** - Document product roadmap
- 💰 **Investors** - Evaluate project viability

### 📋 Project Types:

- E-commerce / Marketplaces
- SaaS / Web platforms
- Mobile apps
- Enterprise systems (ERP, CRM)
- APIs and microservices
- Any software project

---

## 🛠️ Requirements

### For Claude Code:

- Claude Code installed
- Project with `.agent/skills/` structure

### For Claude.ai:

- Pro, Max, Team, or Enterprise plan
- Code execution enabled

### For Claude API:

- Anthropic API Key
- Beta headers enabled:
  - `code-execution-2025-08-25`
  - `skills-2025-10-02`
  - `files-api-2025-04-14`

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a branch: `git checkout -b feature/new-feature`
3. Commit your changes: `git commit -m 'Add new feature'`
4. Push to the branch: `git push origin feature/new-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

Developed by [Gabriel Mayon](https://github.com/gmdaos)

Based on methodologies from:

- BABOK (Business Analysis Body of Knowledge)
- Design Thinking (IDEO, Stanford d.school)
- Lean Startup (Eric Ries)

---

## 📞 Support

- 🐛 **Issues**: [GitHub Issues](https://github.com/gmdaos/business-requirements-analyst-skill/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/gmdaos/business-requirements-analyst-skill/discussions)
- 📧 **Email**: gmgmdaos@gmail.com

---

## 🔗 Useful Links

- [Claude Skills Documentation](https://docs.anthropic.com/claude/docs/skills)
- [Claude API](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [BABOK Guide](https://www.iiba.org/career-resources/a-business-analysis-professionals-foundation-for-success/babok/)

---

**⭐ If this Skill was useful to you, give it a star on GitHub!**
