# Architecture Decision Record (ADR)

An ADR is a short text file that captures an important architectural decision made along with its context and consequences.

## Template

```markdown
# ADR-[ID]: [Title]

**Date:** [YYYY-MM-DD]  
**Status:** [Proposed | Accepted | Superseded | Deprecated]  
**Participants:** [List of people involved]

## Context

[Describe the problem at hand, the technical environment, and any constraints.]

## Decision

[Clearly state the chosen solution.]

## Rationale

[Explain why this decision was made. What alternatives were considered? Why was this one preferred?]

## Consequences

[What is the impact of this decision? Both positive and negative. What new constraints does it introduce?]

## References

[Links to related ADRs, documentation, or RFCs.]
```

## Example

### ADR-001: Use of TailwindCSS for Styling

**Date:** 2026-02-12  
**Status:** Accepted  
**Participants:** Gabriel Mayon, Tech Lead

**Context:**  
The project requires a highly customizable and modern UI. The development speed needs to be maximized for the MVP phase.

**Decision:**  
We will use TailwindCSS v3.x for all visual styling of the components.

**Rationale:**  
Tailwind provides utility-first classes that speed up UI development without leaving HTML files. It ensures a consistent design system and reduces the amount of custom CSS we need to maintain.

**Consequences:**

- **Pros:** Faster prototyping, built-in responsive design, unified spacing/color tokens.
- **Cons:** Learning curve for new developers, HTML template can become cluttered with classes.

---

## When to create an ADR?

- When choosing a database (Relational vs NoSQL).
- When selecting a framework (React vs Vue vs Nuxt).
- When deciding on an authentication strategy (JWT vs Session).
- When defining API standards (REST vs GraphQL).

**Rule of thumb:** If you have to choose between two or more valid ways to solve a problem, document it with an ADR.
