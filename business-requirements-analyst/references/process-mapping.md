# Complex Process Mapping

This document provides advanced techniques for mapping complex business processes.

## Types of Process Diagrams

### 1. Basic Flowchart

**When to use:** Linear processes with few decisions.

**Main symbols:**

- **Oval:** Start/End
- **Rectangle:** Action/Process
- **Diamond:** Decision
- **Arrow:** Flow

**Example:**

```
[Start] → [Validate data] → {Valid?}
                               ↓ Yes       ↓ No
                          [Process]    [Show error]
                               ↓              ↓
                             [End]          [End]
```

### 2. BPMN (Business Process Model and Notation)

**When to use:** Complex business processes with multiple actors.

**Key elements:**

- **Events:** Circles (start, intermediate, end)
- **Activities:** Rounded rectangles
- **Gateways:** Diamonds (decisions, parallel, exclusive)
- **Swimlanes:** Lanes by actor/role

**Structure example:**

```
┌─────────────────────────────────────────┐
│ Customer                                │
├─────────────────────────────────────────┤
│ (○) → [Place order] → [Pay]            │
└──────────────┬──────────────────────────┘
               ↓
┌──────────────┴──────────────────────────┐
│ System                                  │
├─────────────────────────────────────────┤
│ [Validate payment] → ◇ → [Confirm]     │
└──────────────┬──────────────────────────┘
               ↓
┌──────────────┴──────────────────────────┐
│ Warehouse                               │
├─────────────────────────────────────────┤
│ [Prepare shipping] → [Dispatch] → (●)   │
└─────────────────────────────────────────┘
```

### 3. State Diagram

**When to use:** To model states of an entity (order, user, etc.).

**Example: Order States**

```
[Created] → [Paid] → [In preparation] → [Shipped] → [Delivered]
    ↓          ↓              ↓
[Canceled] ← [Canceled] ← [Canceled]
```

## Mapping Techniques

### 1. Current Process Mapping (AS-IS)

**Objective:** Document how the process currently works.

**Steps:**

1. Identify start and end of the process
2. List all activities in order
3. Identify decisions and branches
4. Document involved actors
5. Mark pain points (bottlenecks, frequent errors)

**Example:**

```markdown
## Current Process: Complaint Management

### Actors

- Customer
- Support agent
- Supervisor
- System

### Flow

1. **Customer** sends email with complaint
2. **System** creates automatic ticket
3. **Agent** manually reviews email
4. **Agent** classifies complaint
5. **Decision:** Is supervisor required?
   - Yes → **Supervisor** reviews and approves
   - No → **Agent** proceeds
6. **Agent** responds to customer
7. **System** closes ticket

### Pain Points

- ⚠️ Step 3: Average delay of 4 hours
- ⚠️ Step 5: Escalation criteria not clear
```

### 2. Desired Process Mapping (TO-BE)

**Objective:** Design how the optimized process should work.

**Example:**

```markdown
## Optimized Process: Complaint Management

### Implemented Improvements

- ✅ Automatic classification with AI
- ✅ Defined escalation criteria
- ✅ Response SLA: 2 hours

### Flow

1. **Customer** sends complaint via web form
2. **System** automatically classifies (AI)
3. **System** assigns to agent based on category
4. **Agent** receives immediate notification
5. **Automatic decision:** Is amount > $500 OR critical category?
   - Yes → **Supervisor** automatically notified
   - No → **Agent** proceeds
6. **Agent** responds in < 2 hours
7. **System** requests feedback from customer
8. **System** closes ticket if customer confirms
```

## Common Process Patterns

### Pattern 1: Chain Approval

**Scenario:** Multiple levels of approval.

```
[Request] → [Approver 1] → ◇ Approved?
                                ↓ Yes
                          [Approver 2] → ◇ Approved?
                                             ↓ Yes
                                         [Execute]

                                ↓ No (at any level)
                              [Reject]
```

### Pattern 2: Parallel Processing

**Scenario:** Tasks that can be executed simultaneously.

```
[Start] → ┬→ [Task A] ┬→ [Synchronize] → [Continue]
           ├→ [Task B] ┤
           └→ [Task C] ┘
```

### Pattern 3: Compensation (Rollback)

**Scenario:** Undo actions if something fails.

```
[Book hotel] → [Book flight] → ◇ Success?
                                        ↓ No
                                   [Cancel hotel]
                                        ↓
                                   [Notify error]
```

### Pattern 4: Retry with Backoff

**Scenario:** Retry operation with incremental wait.

```
[Attempt operation] → ◇ Success?
                        ↓ No
                   ◇ Attempts < 3?
                    ↓ Yes
              [Wait 2^attempts seconds]
                    ↓
              [Increment counter]
                    ↓
              [Attempt operation]

                    ↓ No (attempts >= 3)
                [Definitive failure]
```

## Exception Documentation

**Important:** Document what happens when things fail.

### Exception Template

```markdown
### Exception: [Name]

**Condition:** [When it occurs]
**Impact:** [What it affects]
**Action:** [What to do]
**Responsible:** [Who handles it]

**Example:**

### Exception: Payment Rejected

**Condition:** Payment gateway rejects transaction
**Impact:** Order is not confirmed
**Action:**

1. Show error message to user
2. Allow changing payment method
3. Log failed attempt
4. Send email to user with instructions
   **Responsible:** System (automatic)
```

## Process Metrics

Define KPIs for each process:

```markdown
## Metrics: Purchase Process

- **Average time:** 3 minutes
- **Abandonment rate:** < 20%
- **Payment success rate:** > 95%
- **System response time:** < 2 seconds
```

## Recommended Tools

- **Lucidchart** - Professional BPMN diagrams
- **Draw.io** - Free, Google Drive integration
- **Miro** - Collaborative, ideal for workshops
- **PlantUML** - Diagrams as code (versionable)

## Complete Example: Onboarding Process

```markdown
## Process: User Onboarding

### Objective

Register and activate new users in less than 5 minutes.

### Actors

- New user
- System
- Email service (SendGrid)

### Main Flow

1. **User** accesses /register
2. **System** shows form
3. **User** completes details:
   - Email
   - Password
   - Name
4. **System** validates data:
   - Email valid format
   - Password >= 8 characters
   - Name not empty
5. **Decision:** Is data valid?
   - No → Show errors, back to step 3
   - Yes → Continue
6. **System** verifies unique email in DB
7. **Decision:** Does email already exist?
   - Yes → Show "Email already registered," back to step 3
   - No → Continue
8. **System** hashes password (bcrypt)
9. **System** creates user in DB
10. **System** generates verification token
11. **System** sends verification email via SendGrid
12. **System** shows "Check your email" message
13. **User** opens email and clicks on link
14. **System** validates token
15. **Decision:** Is token valid and not expired?
    - No → Show error, offer to resend
    - Yes → Continue
16. **System** marks email as verified
17. **System** automatically authenticates user
18. **System** redirects to dashboard
19. **End**

### Alternative Flows

#### AF-1: Expired Token

- **Trigger:** Step 15, expired token (> 24 hours)
- **Action:**
  1. Show "Link expired"
  2. Offer "Resend email" button
  3. If user accepts, back to step 10

#### AF-2: Email Send Error

- **Trigger:** Step 11, SendGrid returns error
- **Action:**
  1. Log error
  2. Mark user as "pending verification"
  3. Show message "Temporary issue, please try later"
  4. Schedule automatic retry in 5 minutes

### Exceptions

#### EX-1: Database Not Available

- **Condition:** DB does not respond in step 6 or 9
- **Impact:** Cannot complete registration
- **Action:**
  1. Show "Service temporarily unavailable"
  2. Log monitoring error
  3. Alert operations team
- **Responsible:** System + Ops

### Business Rules

- **BR-01:** Verification token expires in 24 hours
- **BR-02:** Password must be at least 8 characters
- **BR-03:** Disposable emails not allowed (use blocked domains list)

### Metrics

- **Average registration time:** < 2 minutes
- **Email verification rate:** > 70% in 24 hours
- **Error rate:** < 1%

### Future Improvements

- [ ] Register with Google/Facebook OAuth
- [ ] 2-step email verification
- [ ] Captcha to prevent bots
```

## Final Tips

1. **Start simple** - Don't try to map everything at once
2. **Validate with users** - Show diagrams to those who execute the process
3. **Iterate** - Processes evolve, your map should too
4. **Use consistent notation** - Choose a standard (BPMN, UML) and stick to it
5. **Document decisions** - Why a certain flow was chosen over another
