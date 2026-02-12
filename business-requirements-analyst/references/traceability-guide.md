# Traceability Guide & Cumulative Tagging

Traceability ensures that every line of code can be traced back to its business origin. This is vital for security, audit, and impact analysis.

## Cumulative Tagging Principle

Every artifact includes tags from ALL upstream layers. This creates an unbreakable chain of evidence.

### Tag Hierarchy

1. `@brd:` - Business Requirements (Layer 1)
2. `@prd:` - Product Requirements (Layer 2)
3. `@req:` - Atomic Requirements (Layer 7)
4. `@impl-status:` - Current state (pending|in-progress|complete)

## How to use Tags in Code

Embed tags in docstrings or comments using the following format:

**Example in Python:**

```python
"""
Order placement service.
@brd: BRD.01.05
@prd: PRD.02.10
@req: REQ-045
@impl-status: complete
"""
def place_order(user_id, cart_id):
    # logic here...
    pass
```

## Traceability Matrix

A Traceability Matrix is a table that maps requirements to their implementation.

| BRD ID | PRD ID | REQ ID | Source Code File   | Status      |
| ------ | ------ | ------ | ------------------ | ----------- |
| BRD.01 | PRD.02 | REQ-01 | `services/auth.js` | Complete    |
| BRD.01 | PRD.03 | REQ-02 | `api/login.py`     | In-progress |

## Benefits for Development

1. **Impact Analysis:** If a BRD changes, you can grep for the ID and see exactly which files must be updated.
2. **Regulatory Compliance:** Essential for systems in finance (SEC, FINRA), health (HIPAA, FDA), or high-security environments.
3. **Automated Validation:** Scripts can verify that every file in the project has at least one requirement tag.

## Recommendation for Analysts

- **ID Stability:** Once a Requirement ID is assigned, NEVER change it. If the requirement is removed, mark it as `DEPRECATED` instead of deleting the ID.
- **Naming Standard:** Use `TYPE.NN.TT` (e.g., `BRD.01.05` where 01 is module and 05 is requirement).
