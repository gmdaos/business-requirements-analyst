# Modelado de Datos Avanzado

Este documento proporciona técnicas y mejores prácticas para el modelado de datos en proyectos de software.

## Niveles de Modelado

### 1. Modelo Conceptual

**Objetivo:** Representación de alto nivel, independiente de tecnología.

**Características:**

- Enfocado en el negocio
- Sin detalles técnicos
- Fácil de entender por stakeholders no técnicos

**Ejemplo:**

```
Usuario ──< tiene >── Pedido ──< contiene >── Producto
                         │
                         └──< tiene >── Pago
```

### 2. Modelo Lógico

**Objetivo:** Estructura detallada, aún independiente de DBMS específico.

**Características:**

- Define atributos y tipos de datos
- Especifica relaciones y cardinalidades
- Incluye restricciones y reglas

**Ejemplo:**

```
Usuario
- id: Integer (PK)
- email: String (único, no nulo)
- nombre: String (no nulo)
- fecha_registro: DateTime

Pedido
- id: Integer (PK)
- usuario_id: Integer (FK → Usuario.id)
- estado: Enum(pendiente, pagado, enviado, entregado)
- total: Decimal(10,2)
- fecha_creacion: DateTime
```

### 3. Modelo Físico

**Objetivo:** Implementación específica para un DBMS (PostgreSQL, MySQL, MongoDB, etc.).

**Características:**

- Incluye índices
- Define particiones
- Especifica tipos de datos específicos del DBMS
- Considera optimizaciones de rendimiento

**Ejemplo (PostgreSQL):**

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_email (email)
);

CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
    estado VARCHAR(20) CHECK (estado IN ('pendiente', 'pagado', 'enviado', 'entregado')),
    total DECIMAL(10,2) NOT NULL,
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_usuario_fecha (usuario_id, fecha_creacion DESC)
);
```

## Tipos de Relaciones

### 1. Uno a Uno (1:1)

**Cuándo usar:**

- Separar datos sensibles
- Optimización (datos frecuentes vs. poco frecuentes)
- Extensión de entidad

**Ejemplo:**

```
Usuario ──1:1── PerfilDetallado

Usuario
- id
- email
- nombre

PerfilDetallado
- id
- usuario_id (FK, único)
- biografia
- foto_perfil
- preferencias_json
```

### 2. Uno a Muchos (1:N)

**Más común en aplicaciones.**

**Ejemplo:**

```
Usuario ──1:N── Pedido

Usuario
- id
- email

Pedido
- id
- usuario_id (FK)
- total
```

### 3. Muchos a Muchos (N:M)

**Requiere tabla intermedia (join table).**

**Ejemplo:**

```
Pedido ──N:M── Producto

Pedido
- id
- total

Producto
- id
- nombre
- precio

PedidoProducto (tabla intermedia)
- pedido_id (FK)
- producto_id (FK)
- cantidad
- precio_unitario (snapshot del precio al momento)
- PRIMARY KEY (pedido_id, producto_id)
```

**⚠️ Importante:** Almacenar `precio_unitario` en la tabla intermedia para mantener histórico, ya que el precio del producto puede cambiar.

## Patrones de Diseño

### Patrón 1: Soft Delete

**Problema:** No queremos eliminar datos permanentemente.

**Solución:** Agregar campo `deleted_at`.

```sql
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(255),
    precio DECIMAL(10,2),
    deleted_at TIMESTAMP NULL
);

-- Consultar solo activos
SELECT * FROM productos WHERE deleted_at IS NULL;

-- "Eliminar" (soft delete)
UPDATE productos SET deleted_at = CURRENT_TIMESTAMP WHERE id = 123;
```

### Patrón 2: Versionado de Datos

**Problema:** Necesitamos mantener historial de cambios.

**Solución:** Tabla de versiones.

```sql
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(255),
    precio DECIMAL(10,2),
    version INTEGER DEFAULT 1
);

CREATE TABLE productos_historial (
    id SERIAL PRIMARY KEY,
    producto_id INTEGER REFERENCES productos(id),
    nombre VARCHAR(255),
    precio DECIMAL(10,2),
    version INTEGER,
    modificado_por INTEGER,
    modificado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Patrón 3: Polimorfismo (Herencia)

**Problema:** Múltiples tipos de entidades con campos comunes.

**Soluciones:**

#### Opción A: Single Table Inheritance (STI)

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    tipo VARCHAR(20), -- 'cliente', 'admin', 'vendedor'
    email VARCHAR(255),
    nombre VARCHAR(100),
    -- Campos específicos de cliente
    direccion_envio VARCHAR(255),
    -- Campos específicos de vendedor
    comision_porcentaje DECIMAL(5,2)
);
```

**Ventajas:** Simple, una sola tabla
**Desventajas:** Muchos campos NULL, no muy normalizado

#### Opción B: Class Table Inheritance (CTI)

```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255),
    nombre VARCHAR(100)
);

CREATE TABLE clientes (
    usuario_id INTEGER PRIMARY KEY REFERENCES usuarios(id),
    direccion_envio VARCHAR(255)
);

CREATE TABLE vendedores (
    usuario_id INTEGER PRIMARY KEY REFERENCES usuarios(id),
    comision_porcentaje DECIMAL(5,2)
);
```

**Ventajas:** Normalizado, sin campos NULL innecesarios
**Desventajas:** Requiere JOINs

### Patrón 4: Auditoría

**Problema:** Rastrear quién y cuándo modificó datos.

**Solución:** Campos de auditoría.

```sql
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    total DECIMAL(10,2),
    -- Campos de auditoría
    creado_por INTEGER REFERENCES usuarios(id),
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modificado_por INTEGER REFERENCES usuarios(id),
    modificado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Patrón 5: Estado Finito (State Machine)

**Problema:** Entidad con estados y transiciones válidas.

**Solución:** Enum + validación de transiciones.

```sql
CREATE TYPE estado_pedido AS ENUM (
    'carrito',
    'pendiente_pago',
    'pagado',
    'en_preparacion',
    'enviado',
    'entregado',
    'cancelado'
);

CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    estado estado_pedido DEFAULT 'carrito',
    estado_anterior estado_pedido
);

-- Tabla de transiciones válidas
CREATE TABLE transiciones_pedido (
    estado_origen estado_pedido,
    estado_destino estado_pedido,
    PRIMARY KEY (estado_origen, estado_destino)
);

-- Insertar transiciones válidas
INSERT INTO transiciones_pedido VALUES
    ('carrito', 'pendiente_pago'),
    ('pendiente_pago', 'pagado'),
    ('pendiente_pago', 'cancelado'),
    ('pagado', 'en_preparacion'),
    ('en_preparacion', 'enviado'),
    ('enviado', 'entregado');
```

### Patrón 6: Datos Jerárquicos

**Problema:** Categorías, comentarios anidados, org charts.

#### Opción A: Adjacency List (más simple)

```sql
CREATE TABLE categorias (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    parent_id INTEGER REFERENCES categorias(id)
);

-- Ejemplo de datos
-- Electrónica (id=1, parent_id=NULL)
--   ├─ Computadoras (id=2, parent_id=1)
--   │   ├─ Laptops (id=3, parent_id=2)
--   │   └─ Desktops (id=4, parent_id=2)
--   └─ Celulares (id=5, parent_id=1)
```

**Ventajas:** Simple de entender e implementar
**Desventajas:** Consultas recursivas pueden ser lentas

#### Opción B: Nested Sets (más eficiente para lectura)

```sql
CREATE TABLE categorias (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    lft INTEGER NOT NULL,
    rgt INTEGER NOT NULL
);

-- Ejemplo de datos (mismo árbol)
-- Electrónica (lft=1, rgt=10)
--   ├─ Computadoras (lft=2, rgt=7)
--   │   ├─ Laptops (lft=3, rgt=4)
--   │   └─ Desktops (lft=5, rgt=6)
--   └─ Celulares (lft=8, rgt=9)

-- Obtener todos los descendientes de "Computadoras"
SELECT * FROM categorias
WHERE lft > 2 AND rgt < 7;
```

**Ventajas:** Consultas muy eficientes
**Desventajas:** Inserción/actualización compleja

## Normalización

### Primera Forma Normal (1NF)

**Regla:** Cada campo debe contener valores atómicos (no listas).

**❌ Incorrecto:**

```sql
CREATE TABLE pedidos (
    id INTEGER,
    productos VARCHAR(255) -- "Laptop,Mouse,Teclado"
);
```

**✅ Correcto:**

```sql
CREATE TABLE pedidos (
    id INTEGER
);

CREATE TABLE pedido_productos (
    pedido_id INTEGER,
    producto_id INTEGER
);
```

### Segunda Forma Normal (2NF)

**Regla:** Cumple 1NF + todos los atributos no-clave dependen de la clave completa.

**❌ Incorrecto:**

```sql
CREATE TABLE pedido_productos (
    pedido_id INTEGER,
    producto_id INTEGER,
    nombre_producto VARCHAR(255), -- Depende solo de producto_id
    PRIMARY KEY (pedido_id, producto_id)
);
```

**✅ Correcto:**

```sql
CREATE TABLE productos (
    id INTEGER PRIMARY KEY,
    nombre VARCHAR(255)
);

CREATE TABLE pedido_productos (
    pedido_id INTEGER,
    producto_id INTEGER REFERENCES productos(id),
    PRIMARY KEY (pedido_id, producto_id)
);
```

### Tercera Forma Normal (3NF)

**Regla:** Cumple 2NF + no hay dependencias transitivas.

**❌ Incorrecto:**

```sql
CREATE TABLE pedidos (
    id INTEGER,
    usuario_id INTEGER,
    usuario_email VARCHAR(255) -- Depende de usuario_id, no de id
);
```

**✅ Correcto:**

```sql
CREATE TABLE usuarios (
    id INTEGER PRIMARY KEY,
    email VARCHAR(255)
);

CREATE TABLE pedidos (
    id INTEGER,
    usuario_id INTEGER REFERENCES usuarios(id)
);
```

## Desnormalización Estratégica

**Cuándo desnormalizar:**

- Optimización de lectura (reportes, dashboards)
- Reducir JOINs complejos
- Mantener snapshots históricos

**Ejemplo: Snapshot de Precio**

```sql
CREATE TABLE pedido_productos (
    pedido_id INTEGER,
    producto_id INTEGER,
    precio_unitario DECIMAL(10,2), -- Desnormalizado intencionalmente
    cantidad INTEGER
);
```

**Razón:** El precio del producto puede cambiar, pero queremos mantener el precio al momento de la compra.

## Índices

### Cuándo Crear Índices

**Crear índice si:**

- Campo usado frecuentemente en WHERE
- Campo usado en JOINs
- Campo usado en ORDER BY
- Campo con alta cardinalidad (muchos valores únicos)

**NO crear índice si:**

- Tabla muy pequeña (< 1000 filas)
- Campo con baja cardinalidad (ej: booleano)
- Campo que cambia frecuentemente

### Tipos de Índices

```sql
-- Índice simple
CREATE INDEX idx_email ON usuarios(email);

-- Índice compuesto (orden importa)
CREATE INDEX idx_usuario_fecha ON pedidos(usuario_id, fecha_creacion);

-- Índice único
CREATE UNIQUE INDEX idx_email_unique ON usuarios(email);

-- Índice parcial (PostgreSQL)
CREATE INDEX idx_pedidos_activos ON pedidos(estado)
WHERE estado != 'cancelado';

-- Índice de texto completo
CREATE INDEX idx_productos_nombre ON productos
USING GIN(to_tsvector('spanish', nombre));
```

## Restricciones (Constraints)

```sql
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
    total DECIMAL(10,2) CHECK (total >= 0),
    estado VARCHAR(20) DEFAULT 'pendiente',
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    -- Constraint con nombre
    CONSTRAINT chk_estado CHECK (estado IN ('pendiente', 'pagado', 'enviado'))
);
```

## Ejemplo Completo: E-commerce

```sql
-- Usuarios
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    rol VARCHAR(20) DEFAULT 'cliente',
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_email (email)
);

-- Categorías (jerárquicas)
CREATE TABLE categorias (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,
    parent_id INTEGER REFERENCES categorias(id),
    INDEX idx_parent (parent_id)
);

-- Productos
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    categoria_id INTEGER REFERENCES categorias(id),
    nombre VARCHAR(255) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10,2) NOT NULL CHECK (precio >= 0),
    stock INTEGER DEFAULT 0 CHECK (stock >= 0),
    activo BOOLEAN DEFAULT true,
    deleted_at TIMESTAMP NULL,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_categoria (categoria_id),
    INDEX idx_activo (activo) WHERE deleted_at IS NULL
);

-- Pedidos
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER NOT NULL REFERENCES usuarios(id),
    estado VARCHAR(20) DEFAULT 'carrito',
    subtotal DECIMAL(10,2) DEFAULT 0,
    impuestos DECIMAL(10,2) DEFAULT 0,
    total DECIMAL(10,2) DEFAULT 0,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    actualizado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_usuario_estado (usuario_id, estado),
    INDEX idx_fecha (creado_en DESC)
);

-- Pedido - Productos (N:M)
CREATE TABLE pedido_productos (
    pedido_id INTEGER REFERENCES pedidos(id) ON DELETE CASCADE,
    producto_id INTEGER REFERENCES productos(id),
    cantidad INTEGER NOT NULL CHECK (cantidad > 0),
    precio_unitario DECIMAL(10,2) NOT NULL, -- Snapshot
    subtotal DECIMAL(10,2) GENERATED ALWAYS AS (cantidad * precio_unitario) STORED,
    PRIMARY KEY (pedido_id, producto_id)
);

-- Direcciones de Envío
CREATE TABLE direcciones (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER REFERENCES usuarios(id) ON DELETE CASCADE,
    nombre VARCHAR(100),
    calle VARCHAR(255),
    ciudad VARCHAR(100),
    codigo_postal VARCHAR(20),
    pais VARCHAR(50),
    es_predeterminada BOOLEAN DEFAULT false,
    INDEX idx_usuario (usuario_id)
);

-- Pagos
CREATE TABLE pagos (
    id SERIAL PRIMARY KEY,
    pedido_id INTEGER UNIQUE REFERENCES pedidos(id),
    metodo VARCHAR(50), -- 'tarjeta', 'paypal', 'transferencia'
    monto DECIMAL(10,2) NOT NULL,
    estado VARCHAR(20) DEFAULT 'pendiente',
    transaccion_id VARCHAR(255),
    procesado_en TIMESTAMP,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_pedido (pedido_id),
    INDEX idx_estado (estado)
);

-- Reseñas
CREATE TABLE resenas (
    id SERIAL PRIMARY KEY,
    producto_id INTEGER REFERENCES productos(id) ON DELETE CASCADE,
    usuario_id INTEGER REFERENCES usuarios(id),
    calificacion INTEGER CHECK (calificacion BETWEEN 1 AND 5),
    comentario TEXT,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (producto_id, usuario_id), -- Un usuario, una reseña por producto
    INDEX idx_producto (producto_id)
);
```

## Consejos Finales

1. **Empieza normalizado** - Desnormaliza solo si hay razón de rendimiento
2. **Usa tipos de datos apropiados** - No uses VARCHAR(255) para todo
3. **Define restricciones en la BD** - No solo en la aplicación
4. **Documenta decisiones** - Por qué se eligió cierta estructura
5. **Planifica para escala** - Considera particionamiento si esperas millones de filas
6. **Usa migraciones** - Versionado de esquema (Alembic, Flyway, etc.)
7. **Testea con datos reales** - El rendimiento puede sorprender
