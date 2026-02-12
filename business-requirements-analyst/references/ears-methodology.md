# EARS Methodology (Easy Approach to Requirements Syntax)

EARS is a mechanism to help analysts write better, less ambiguous requirements. It provides a set of patterns to follow when drafting functional requirements.

## Why use EARS?

- **Eliminates Ambiguity:** Avoids vague terms like "fast," "easy," or "intuitive."
- **Standardization:** Every requirement follows a predictable structure.
- **AI-Optimized:** Claude and other AI models follow structured syntax much more accurately than loose natural language.

## The 5 Basic Patterns

### 1. Ubiquitous Requirements

_Used for requirements that are always true for the system._

- **Syntax:** The <system name> shall <system response>.
- **Example:** The System shall maintain a secure connection with the database.

### 2. Event-Driven Requirements

_Used for requirements that trigger on a specific event._

- **Syntax:** WHEN <trigger> THE <system name> shall <system response>.
- **Example:** WHEN the user clicks "Export," THE System shall generate a PDF report.

### 3. State-Driven Requirements

_Used when the system is in a specific state._

- **Syntax:** WHILE <in a specific state> THE <system name> shall <system response>.
- **Example:** WHILE the system is in "Maintenance Mode," THE System shall display a generic downtime page.

### 4. Unwanted Behavior Requirements

_Used for handling errors or unexpected events._

- **Syntax:** IF <unwanted condition or event> THEN THE <system name> shall <system response>.
- **Example:** IF the user enters an invalid password THEN THE System shall display an "Invalid Credentials" message.

### 5. Optional Feature Requirements

_Used when a requirement depends on an optional component._

- **Syntax:** WHERE <feature is included> THE <system name> shall <system response>.
- **Example:** WHERE the Bio-metric sensor is available THE System shall allow login via fingerprint.

## Complex / Hybrid Requirements

You can combine these patterns for more complex scenarios.

- **Syntax:** WHEN <trigger> WHILE <in a state> THE <system name> shall <system response>.
- **Example:** WHEN the user initiates a checkout WHILE the cart is empty THE System shall prevent the transaction.

## Best Practices

1. **Be Specific:** Define the "<system name>" consistently (e.g., "The Order Service").
2. **One Shell per Requirement:** Each requirement should only have one "shall." If it has more, split it.
3. **Avoid Conjunctions:** Stay away from "and," "or," and "also" within the response part.

---

## EARS Cheat Sheet

| Pattern      | Keyword  | Formula                       |
| ------------ | -------- | ----------------------------- |
| Ubiquitous   | (Always) | The [S] shall [R]             |
| Event-Driven | WHEN     | WHEN [T] THE [S] shall [R]    |
| State-Driven | WHILE    | WHILE [ST] THE [S] shall [R]  |
| Unwanted     | IF       | IF [U] THEN THE [S] shall [R] |
| Optional     | WHERE    | WHERE [F] THE [S] shall [R]   |
