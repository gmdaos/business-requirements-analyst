# Advanced Data Modeling

This document provides techniques and best practices for data modeling in software projects.

## Modeling Levels

### 1. Conceptual Model

**Objective:** High-level representation, technology-independent.

**Characteristics:**

- Focused on business
- No technical details
- Easy to understand by non-technical stakeholders

**Example:**

```
User ──< has >── Order ──< contains >── Product
                    │
                    └──< has >── Payment
```

### 2. Logical Model

**Objective:** Detailed structure, still independent of specific DBMS.

**Characteristics:**

- Defines attributes and data types
- Specifies relationships and cardinalities
- Includes constraints and rules

**Example:**

```
User
- id: Integer (PK)
- email: String (unique, not null)
- name: String (not null)
- registration_date: DateTime

Order
- id: Integer (PK)
- user_id: Integer (FK → User.id)
- status: Enum(pending, paid, shipped, delivered)
- total: Decimal(10,2)
- creation_date: DateTime
```

### 3. Physical Model

**Objective:** Specific implementation for a DBMS (PostgreSQL, MySQL, MongoDB, etc.).

**Characteristics:**

- Includes indexes
- Defines partitions
- Specifies DBMS-specific data types
- Considers performance optimizations

**Example (PostgreSQL):**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_email ON users(email);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    status VARCHAR(20) CHECK (status IN ('pending', 'paid', 'shipped', 'delivered')),
    total DECIMAL(10,2) NOT NULL,
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_user_date ON orders(user_id, creation_date DESC);
```

## Relationship Types

### 1. One-to-One (1:1)

**When to use:**

- Separate sensitive data
- Optimization (frequent vs. infrequent data)
- Entity extension

**Example:**

```
User ──1:1── DetailedProfile

User
- id
- email
- name

DetailedProfile
- id
- user_id (FK, unique)
- biography
- profile_picture
- preferences_json
```

### 2. One-to-Many (1:N)

**Most common in applications.**

**Example:**

```
User ──1:N── Order

User
- id
- email

Order
- id
- user_id (FK)
- total
```

### 3. Many-to-Many (N:M)

**Requires a join table (or intermediate table).**

**Example:**

```
Order ──N:M── Product

Order
- id
- total

Product
- id
- name
- price

OrderProduct (intermediate table)
- order_id (FK)
- product_id (FK)
- quantity
- unit_price (snapshot of the price at the time)
- PRIMARY KEY (order_id, product_id)
```

**⚠️ Important:** Store `unit_price` in the intermediate table to maintain history, as the product's price may change.

## Design Patterns

### Pattern 1: Soft Delete

**Problem:** We don't want to permanently delete data.

**Solution:** Add a `deleted_at` field.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(10,2),
    deleted_at TIMESTAMP NULL
);

-- Query only active items
SELECT * FROM products WHERE deleted_at IS NULL;

-- "Delete" (soft delete)
UPDATE products SET deleted_at = CURRENT_TIMESTAMP WHERE id = 123;
```

### Pattern 2: Data Versioning

**Problem:** We need to maintain a history of changes.

**Solution:** Version table.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(10,2),
    version INTEGER DEFAULT 1
);

CREATE TABLE products_history (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    name VARCHAR(255),
    price DECIMAL(10,2),
    version INTEGER,
    modified_by INTEGER,
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Pattern 3: Polymorphism (Inheritance)

**Problem:** Multiple types of entities with common fields.

**Solutions:**

#### Option A: Single Table Inheritance (STI)

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    type VARCHAR(20), -- 'customer', 'admin', 'vendor'
    email VARCHAR(255),
    name VARCHAR(100),
    -- Customer-specific fields
    shipping_address VARCHAR(255),
    -- Vendor-specific fields
    commission_percentage DECIMAL(5,2)
);
```

**Advantages:** Simple, single table
**Disadvantages:** Many NULL fields, not highly normalized

#### Option B: Class Table Inheritance (CTI)

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255),
    name VARCHAR(100)
);

CREATE TABLE customers (
    user_id INTEGER PRIMARY KEY REFERENCES users(id),
    shipping_address VARCHAR(255)
);

CREATE TABLE vendors (
    user_id INTEGER PRIMARY KEY REFERENCES users(id),
    commission_percentage DECIMAL(5,2)
);
```

**Advantages:** Normalized, no unnecessary NULL fields
**Disadvantages:** Requires JOINs

### Pattern 4: Audit

**Problem:** Track who and when data was modified.

**Solution:** Audit fields.

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    total DECIMAL(10,2),
    -- Audit fields
    created_by INTEGER REFERENCES users(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modified_by INTEGER REFERENCES users(id),
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Pattern 5: Finite State Machine

**Problem:** Entity with valid states and transitions.

**Solution:** Enum + transition validation.

```sql
CREATE TYPE order_status AS ENUM (
    'cart',
    'pending_payment',
    'paid',
    'in_preparation',
    'shipped',
    'delivered',
    'canceled'
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    status order_status DEFAULT 'cart',
    previous_status order_status
);

-- Valid transitions table
CREATE TABLE order_transitions (
    origin_status order_status,
    destination_status order_status,
    PRIMARY KEY (origin_status, destination_status)
);

-- Insert valid transitions
INSERT INTO order_transitions VALUES
    ('cart', 'pending_payment'),
    ('pending_payment', 'paid'),
    ('pending_payment', 'canceled'),
    ('paid', 'in_preparation'),
    ('in_preparation', 'shipped'),
    ('shipped', 'delivered');
```

### Pattern 6: Hierarchical Data

**Problem:** Categories, nested comments, org charts.

#### Option A: Adjacency List (simplest)

```sql
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    parent_id INTEGER REFERENCES categories(id)
);

-- Data example
-- Electronics (id=1, parent_id=NULL)
--   ├─ Computers (id=2, parent_id=1)
--   │   ├─ Laptops (id=3, parent_id=2)
--   │   └─ Desktops (id=4, parent_id=2)
--   └─ Mobile Phones (id=5, parent_id=1)
```

**Advantages:** Simple to understand and implement
**Disadvantages:** Recursive queries can be slow

#### Option B: Nested Sets (more efficient for reading)

```sql
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    lft INTEGER NOT NULL,
    rgt INTEGER NOT NULL
);

-- Data example (same tree)
-- Electronics (lft=1, rgt=10)
--   ├─ Computers (lft=2, rgt=7)
--   │   ├─ Laptops (lft=3, rgt=4)
--   │   └─ Desktops (lft=5, rgt=6)
--   └─ Mobile Phones (lft=8, rgt=9)

-- Get all descendants of "Computers"
SELECT * FROM categories
WHERE lft > 2 AND rgt < 7;
```

**Advantages:** Very efficient queries
**Disadvantages:** Complex insertion/update

## Normalization

### First Normal Form (1NF)

**Rule:** Each field must contain atomic values (no lists).

**❌ Incorrect:**

```sql
CREATE TABLE orders (
    id INTEGER,
    products VARCHAR(255) -- "Laptop,Mouse,Keyboard"
);
```

**✅ Correct:**

```sql
CREATE TABLE orders (
    id INTEGER
);

CREATE TABLE order_products (
    order_id INTEGER,
    product_id INTEGER
);
```

### Second Normal Form (2NF)

**Rule:** Complies with 1NF + all non-key attributes depend on the full key.

**❌ Incorrect:**

```sql
CREATE TABLE order_products (
    order_id INTEGER,
    product_id INTEGER,
    product_name VARCHAR(255), -- Depends only on product_id
    PRIMARY KEY (order_id, product_id)
);
```

**✅ Correct:**

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name VARCHAR(255)
);

CREATE TABLE order_products (
    order_id INTEGER,
    product_id INTEGER REFERENCES products(id),
    PRIMARY KEY (order_id, product_id)
);
```

### Third Normal Form (3NF)

**Rule:** Complies with 2NF + there are no transitive dependencies.

**❌ Incorrect:**

```sql
CREATE TABLE orders (
    id INTEGER,
    user_id INTEGER,
    user_email VARCHAR(255) -- Depends on user_id, not id
);
```

**✅ Correct:**

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    email VARCHAR(255)
);

CREATE TABLE orders (
    id INTEGER,
    user_id INTEGER REFERENCES users(id)
);
```

## Strategic Denormalization

**When to denormalize:**

- Read optimization (reports, dashboards)
- Reduce complex JOINs
- Maintain historical snapshots

**Example: Price Snapshot**

```sql
CREATE TABLE order_products (
    order_id INTEGER,
    product_id INTEGER,
    unit_price DECIMAL(10,2), -- Intentionally denormalized
    quantity INTEGER
);
```

**Reason:** The product price can change, but we want to keep the price at the time of purchase.

## Indexes

### When to Create Indexes

**Create index if:**

- Field is used frequently in WHERE
- Field is used in JOINs
- Field is used in ORDER BY
- Field has high cardinality (many unique values)

**DO NOT create index if:**

- Table is very small (< 1000 rows)
- Field has low cardinality (e.g., boolean)
- Field changes frequently

### Index Types

```sql
-- Simple index
CREATE INDEX idx_email ON users(email);

-- Composite index (order matters)
CREATE INDEX idx_user_date ON orders(user_id, creation_date);

-- Unique index
CREATE UNIQUE INDEX idx_email_unique ON users(email);

-- Partial index (PostgreSQL)
CREATE INDEX idx_active_orders ON orders(status)
WHERE status != 'canceled';

-- Full-text index
CREATE INDEX idx_products_name ON products
USING GIN(to_tsvector('english', name));
```

## Constraints

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total DECIMAL(10,2) CHECK (total >= 0),
    status VARCHAR(20) DEFAULT 'pending',
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    -- Named constraint
    CONSTRAINT chk_status CHECK (status IN ('pending', 'paid', 'shipped'))
);
```

## Complete Example: E-commerce

```sql
-- Users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    name VARCHAR(100) NOT NULL,
    role VARCHAR(20) DEFAULT 'customer',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);

-- Categories (hierarchical)
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,
    parent_id INTEGER REFERENCES categories(id)
);

CREATE INDEX idx_categories_parent ON categories(parent_id);

-- Products
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    category_id INTEGER REFERENCES categories(id),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL CHECK (price >= 0),
    stock INTEGER DEFAULT 0 CHECK (stock >= 0),
    active BOOLEAN DEFAULT true,
    deleted_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_products_active ON products(active) WHERE deleted_at IS NULL;

-- Orders
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    status VARCHAR(20) DEFAULT 'cart',
    subtotal DECIMAL(10,2) DEFAULT 0,
    taxes DECIMAL(10,2) DEFAULT 0,
    total DECIMAL(10,2) DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_orders_user_status ON orders(user_id, status);
CREATE INDEX idx_orders_date ON orders(created_at DESC);

-- Order - Products (N:M)
CREATE TABLE order_products (
    order_id INTEGER REFERENCES orders(id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(id),
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10,2) NOT NULL, -- Snapshot
    subtotal DECIMAL(10,2) GENERATED ALWAYS AS (quantity * unit_price) STORED,
    PRIMARY KEY (order_id, product_id)
);

-- Shipping Addresses
CREATE TABLE addresses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(100),
    street VARCHAR(255),
    city VARCHAR(100),
    zip_code VARCHAR(20),
    country VARCHAR(50),
    is_default BOOLEAN DEFAULT false
);

CREATE INDEX idx_addresses_user ON addresses(user_id);

-- Payments
CREATE TABLE payments (
    id SERIAL PRIMARY KEY,
    order_id INTEGER UNIQUE REFERENCES orders(id),
    method VARCHAR(50), -- 'card', 'paypal', 'transfer'
    amount DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) DEFAULT 'pending',
    transaction_id VARCHAR(255),
    processed_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_payments_order ON payments(order_id);
CREATE INDEX idx_payments_status ON payments(status);

-- Reviews
CREATE TABLE reviews (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id) ON DELETE CASCADE,
    user_id INTEGER REFERENCES users(id),
    rating INTEGER CHECK (rating BETWEEN 1 AND 5),
    comment TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (product_id, user_id) -- One user, one review per product
);

CREATE INDEX idx_reviews_product ON reviews(product_id);
```

## Final Tips

1. **Start normalized** - Denormalize only if there is a performance reason
2. **Use appropriate data types** - Don't use VARCHAR(255) for everything
3. **Define constraints in the DB** - Not just in the application
4. **Document decisions** - Why a certain structure was chosen
5. **Plan for scale** - Consider partitioning if you expect millions of rows
6. **Use migrations** - Schema versioning (Alembic, Flyway, etc.)
7. **Test with real data** - Performance can be surprising
