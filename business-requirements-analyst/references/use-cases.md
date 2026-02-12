# Detailed Use Cases

This document provides guides and templates for documenting use cases professionally.

## What is a Use Case?

A use case describes how an actor (user or system) interacts with the system to achieve a specific goal.

**Key components:**

- **Actor:** Who initiates the interaction
- **Goal:** What they want to achieve
- **Preconditions:** Necessary state before starting
- **Main Flow:** Normal execution steps
- **Alternative Flows:** Variations of the main flow
- **Postconditions:** System state after completion

## Standard Use Case Format

### Basic Template

```markdown
## UC-[ID]: [Use Case Name]

**Primary Actor:** [Actor Name]
**Goal:** [What the actor wants to achieve]
**Preconditions:** [Conditions that must be met beforehand]

### Main Flow

1. [Step 1]
2. [Step 2]
3. [Step 3]
   ...

### Alternative Flows

**AF-[ID]: [Alternative flow name]**

- **Trigger:** [When it activates]
- **Steps:**
  1. [Step 1]
  2. [Step 2]
     ...

### Exceptions

**EX-[ID]: [Exception name]**

- **Condition:** [When it occurs]
- **Action:** [What to do]

### Postconditions

**Success:** [System state if everything goes well]
**Failure:** [System state if it fails]

### Business Rules

- **BR-[ID]:** [Applicable rule]

### Related Requirements

- FR-[ID]: [Functional requirement]
- NFR-[ID]: [Non-functional requirement]
```

## Detail Levels

### Level 1: High Level (Brief)

**When to use:** Initial documentation, executive presentations.

**Example:**

```markdown
## UC-001: Buy Product

**Actor:** Customer
**Goal:** To acquire a product from the catalog

The customer browses the catalog, selects products, adds them to the cart,
proceeds to checkout, enters payment and shipping information, and confirms the purchase.
```

### Level 2: Casual

**When to use:** Planning, team discussions.

**Example:**

```markdown
## UC-001: Buy Product

**Actor:** Customer
**Goal:** To acquire a product from the catalog

### Main Flow

1. Customer browses product catalog
2. Customer selects product and adds it to the cart
3. Customer proceeds to checkout
4. Customer enters shipping information
5. Customer selects payment method
6. Customer confirms the purchase
7. System processes the payment
8. System confirms the order and sends an email
```

### Level 3: Fully Dressed

**When to use:** Development, QA, formal documentation.

**Complete example below.**

## Complete Example: Detailed Use Case

```markdown
## UC-001: Purchase Product

**ID:** UC-001
**Name:** Purchase Product
**Primary Actor:** Registered customer
**Secondary Actors:** Payment System (Stripe), Email System (SendGrid)
**Goal:** To allow the customer to buy products from the catalog
**Level:** User
**Scope:** E-commerce system

### Preconditions

- The customer must be authenticated
- At least one product with available stock must exist
- The payment system must be operational

### Minimum Guarantees (if it fails)

- The customer is not charged
- Stock is not reduced
- The failed attempt is recorded in logs

### Success Guarantees

- A confirmed order is created
- Product stock is reduced
- The customer is charged
- A confirmation email is sent
- A shipping order is generated

### Main Flow (Success Scenario)

1. Customer accesses the catalog page
2. System shows available products with:
   - Image
   - Name
   - Price
   - Stock indicator
3. Customer searches/filters products (optional)
4. System updates list according to criteria
5. Customer selects a product
6. System shows product details:
   - Full description
   - Additional images
   - Reviews from other customers
   - Available stock
7. Customer selects desired quantity
8. Customer clicks "Add to cart"
9. System validates available stock
10. System adds product to the cart
11. System shows confirmation notification
12. Customer continues shopping (back to step 3) or proceeds to checkout
13. Customer clicks "Proceed to payment"
14. System shows cart summary:
    - List of products
    - Quantities
    - Unit prices
    - Subtotal
    - Taxes
    - Total
15. Customer reviews and clicks "Continue"
16. System requests shipping address
17. Customer selects a saved address or enters a new one:
    - Full name
    - Street and number
    - City
    - Zip code
    - Country
18. System validates address format
19. Customer clicks "Continue"
20. System shows available payment methods:
    - Credit/Debit card
    - PayPal
    - Bank transfer
21. Customer selects "Credit card"
22. System shows payment form (Stripe)
23. Customer enters card details:
    - Card number
    - Expiration date
    - CVV
    - Cardholder's name
24. System validates data format
25. Customer clicks "Confirm purchase"
26. System shows "Processing..." screen
27. System creates order in "pending_payment" status
28. System reserves product stock
29. System sends payment request to Stripe with:
    - Total amount
    - Card details (tokenized)
    - Order description
30. Stripe processes the payment
31. Stripe returns success confirmation
32. System updates order status to "paid"
33. System reduces product stock
34. System generates payment record
35. System creates shipping order
36. System sends confirmation email via SendGrid with:
    - Order number
    - Product summary
    - Shipping address
    - Total amount
    - Tracking link
37. System shows confirmation page with:
    - Success message
    - Order number
    - Estimated delivery time
38. **End of use case**

### Alternative Flows

#### AF-1: Unauthenticated Customer

**Trigger:** Step 1, customer is not authenticated

**Steps:**

1. System detects no active session
2. System redirects to login page
3. System shows options:
   - Log in
   - Sign up
   - Continue as guest
4. If customer logs in or signs up:
   - Continue at step 1 of the main flow
5. If customer continues as guest:
   - Continue at step 1 of the main flow
   - At step 17, also request email for confirmation

#### AF-2: Product Out of Stock

**Trigger:** Step 9, insufficient stock

**Steps:**

1. System detects stock < requested quantity
2. System shows message: "Insufficient stock. Available: [X] units"
3. System offers options:
   - Adjust quantity to available stock
   - Notify me when in stock
   - Cancel
4. If customer adjusts quantity:
   - Continue at step 10 with adjusted quantity
5. If customer requests notification:
   - System records notification request
   - Back to step 3 of the main flow
6. If customer cancels:
   - Back to step 3 of the main flow

#### AF-3: Customer Uses Saved Address

**Trigger:** Step 17, customer has saved addresses

**Steps:**

1. System shows list of saved addresses
2. Customer selects an address
3. System pre-fills form with saved data
4. Customer can edit data if desired
5. Continue at step 19 of the main flow

#### AF-4: Payment with PayPal

**Trigger:** Step 21, customer selects PayPal

**Steps:**

1. System redirects to PayPal page
2. Customer logs in to PayPal
3. Customer authorizes payment
4. PayPal redirects back to the system with a token
5. Continue at step 27 of the main flow

#### AF-5: Customer Applies Discount Coupon

**Trigger:** Step 14, customer has a coupon

**Steps:**

1. Customer clicks "I have a coupon"
2. System shows text field
3. Customer enters coupon code
4. System validates coupon:
   - Exists
   - Not expired
   - Applies to cart products
   - Has not been used (if single-use)
5. System applies discount
6. System updates totals
7. System shows applied discount
8. Continue at step 15 of the main flow

### Exceptions

#### EX-1: Payment Rejected

**Condition:** Step 31, Stripe rejects the payment

**Action:**

1. System receives error from Stripe with reason:
   - Insufficient funds
   - Expired card
   - Blocked card
   - Validation error
2. System updates order to "failed_payment" status
3. System releases stock reservation
4. System shows specific error message to the customer
5. System offers options:
   - Try with another card
   - Try with another payment method
   - Cancel purchase
6. If customer tries again:
   - Back to step 22 of the main flow
7. If customer cancels:
   - System marks order as "canceled"
   - End of use case (failure)

#### EX-2: Communication Error with Stripe

**Condition:** Step 29, no response from Stripe (timeout)

**Action:**

1. System detects timeout after 30 seconds
2. System records error in logs
3. System marks order as "payment_pending_verification"
4. System shows message to the customer:
   "We are verifying your payment. You will receive a confirmation email in the next few minutes."
5. System schedules asynchronous task to:
   - Check payment status in Stripe every 1 minute
   - Maximum 10 attempts
6. If payment is confirmed:
   - Continue at step 32 of the main flow
7. If not confirmed after 10 attempts:
   - System marks order as "requires_manual_review"
   - System alerts operations team
   - System sends email to customer requesting contact

#### EX-3: Error Sending Email

**Condition:** Step 36, SendGrid returns error

**Action:**

1. System records error in logs
2. System marks email as "pending_resend"
3. System schedules retry in 5 minutes
4. **Important:** Do not block the flow; the order is already confirmed
5. Continue at step 37 of the main flow
6. Asynchronous task retries sending up to 3 times
7. If it definitively fails:
   - System alerts operations team
   - Email remains available in admin panel for manual resend

#### EX-4: Stock Sold Out During Process

**Condition:** Step 28, when trying to reserve stock, it's no longer available

**Action:**

1. System detects another customer bought the last stock
2. System DOES NOT process the payment
3. System shows message:
   "Sorry, the product [X] sold out while we were processing your order."
4. System offers options:
   - Remove product from cart and continue with others
   - Notify me when in stock
   - Cancel the entire purchase
5. If customer removes product:
   - System updates cart
   - Back to step 14 of the main flow
6. If customer cancels:
   - End of use case (failure)

### Postconditions

**Success:**

- A "paid" order exists
- Product stock reduced
- Payment recorded and confirmed
- Confirmation email sent
- Shipping order created

**Failure:**

- Order in "canceled" or "failed_payment" status
- Stock not modified
- No charge made to the customer
- Error recorded in logs

### Business Rules

- **BR-01:** Stock is reserved only after confirming payment
- **BR-02:** Prices shown include taxes
- **BR-03:** A coupon can only be used once per user
- **BR-04:** Coupons expire at 23:59 on the indicated date
- **BR-05:** The cart is kept for 7 days for authenticated users
- **BR-06:** The cart is kept for 24 hours for guests (cookie)
- **BR-07:** Buying more than 10 units of the same product is not allowed
- **BR-08:** Payments are processed in the customer's local currency

### Related Requirements

**Functional:**

- FR-01: System must allow user registration
- FR-05: System must manage product catalog
- FR-12: System must process payments with Stripe
- FR-18: System must send confirmation emails
- FR-23: System must manage product stock

**Non-Functional:**

- NFR-01: Payment process must complete in < 5 seconds
- NFR-05: System must use HTTPS for all transactions
- NFR-08: Card data must be tokenized (PCI-DSS)
- NFR-12: System must support 100 concurrent purchases

### Usage Frequency

- **Estimated:** 500 purchases/day
- **Peak:** 2000 purchases/day (Black Friday, Cyber Monday)

### Priority

**High** - Core business functionality

### Additional Notes

- Consider implementing a "1-click purchase" system for frequent users
- Evaluate adding a "save for later" option in the cart
- Future: Integrate with loyalty points program
```

## Use Case Diagrams (UML)

**Visual representation of actors and use cases.**

```
┌─────────────────────────────────────────────┐
│         E-commerce System                   │
│                                             │
│  ┌──────────────────┐                      │
│  │ Search Product   │                      │
│  └──────────────────┘                      │
│           │                                 │
│  ┌──────────────────┐                      │
│  │ Add to Cart      │                      │
│  └──────────────────┘                      │
│           │                                 │
│  ┌──────────────────┐    ┌──────────────┐ │
│  │ Purchase Product │───→│ Process Payment│ │
│  └──────────────────┘    └──────────────┘ │
│           │                      │         │
│           │              ┌───────┴────┐    │
│           │              │   Stripe   │    │
│           │              └────────────┘    │
│  ┌──────────────────┐                      │
│  │ View History     │                      │
│  └──────────────────┘                      │
│                                             │
└─────────────────────────────────────────────┘
         │
    ┌────┴────┐
    │ Customer│
    └─────────┘
```

## Traceability Matrix

**Relate use cases with requirements.**

| Use Case                 | FR-01 | FR-05 | FR-12 | FR-18 | NFR-01 | NFR-05 |
| ------------------------ | ----- | ----- | ----- | ----- | ------ | ------ |
| UC-001: Purchase Product | X     | X     | X     | X     | X      | X      |
| UC-002: Manage Cart      |       | X     |       |       | X      |        |
| UC-003: View History     | X     |       |       |       | X      |        |

## Tips for Writing Good Use Cases

1. **Use business language** - Not technical terms
2. **Focus on the goal** - What the user wants to achieve, not how
3. **Be specific in alternative flows** - Cover edge cases
4. **Document exceptions** - What happens when it fails
5. **Keep consistency** - Use the same template for all
6. **Review with stakeholders** - Validate that it reflects reality
7. **Update when it changes** - Living document

## Recommended Tools

- **Enterprise Architect** - Professional UML modeling
- **Lucidchart** - Collaborative diagrams
- **Confluence** - Use case documentation
- **Jira** - Link use cases with user stories
