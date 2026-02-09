# Documento de Requerimientos - [Nombre del Proyecto]

**Versión:** 1.0  
**Fecha:** [Fecha]  
**Autor:** [Nombre]  
**Estado:** [Borrador | En Revisión | Aprobado]

---

## Tabla de Contenidos

1. [Visión del Negocio](#1-visión-del-negocio)
2. [Stakeholders](#2-stakeholders)
3. [Tipos de Usuarios (Personas)](#3-tipos-de-usuarios-personas)
4. [Procesos del Negocio](#4-procesos-del-negocio)
5. [Requerimientos Funcionales](#5-requerimientos-funcionales)
6. [Requerimientos No Funcionales](#6-requerimientos-no-funcionales)
7. [Reglas del Negocio](#7-reglas-del-negocio)
8. [Modelo de Datos](#8-modelo-de-datos)
9. [Integraciones](#9-integraciones)
10. [Riesgos y Supuestos](#10-riesgos-y-supuestos)
11. [Roadmap / Fases](#11-roadmap--fases)
12. [Anexos](#12-anexos)

---

## 1. Visión del Negocio

### 1.1 Problema que Resuelve

**Descripción del problema:**
[Describe el dolor o necesidad que atiende el proyecto]

**Situación actual:**
[Cómo se resuelve actualmente este problema, si aplica]

**Impacto del problema:**
[Consecuencias de no resolver este problema]

### 1.2 Propuesta de Valor

**¿Qué hace único a este producto/servicio?**
[Describe la propuesta de valor diferencial]

**Ventajas competitivas:**

- [Ventaja 1]
- [Ventaja 2]
- [Ventaja 3]

**Comparación con alternativas:**
| Característica | Nuestra Solución | Competidor A | Competidor B |
|----------------|------------------|--------------|--------------|
| [Característica 1] | [Valor] | [Valor] | [Valor] |
| [Característica 2] | [Valor] | [Valor] | [Valor] |

### 1.3 Objetivos del Negocio

**Objetivo General:**
[Objetivo principal del proyecto]

**Objetivos Específicos:**

1. [Objetivo específico 1]
2. [Objetivo específico 2]
3. [Objetivo específico 3]

### 1.4 KPIs Principales

| KPI     | Métrica        | Meta             | Plazo    |
| ------- | -------------- | ---------------- | -------- |
| [KPI 1] | [Cómo se mide] | [Valor objetivo] | [Cuándo] |
| [KPI 2] | [Cómo se mide] | [Valor objetivo] | [Cuándo] |
| [KPI 3] | [Cómo se mide] | [Valor objetivo] | [Cuándo] |

**Ejemplo:**
| KPI | Métrica | Meta | Plazo |
|-----|---------|------|-------|
| Usuarios activos | Usuarios que inician sesión al menos 1 vez por semana | 10,000 | 6 meses |
| Tasa de conversión | % de visitantes que completan una compra | 3% | 3 meses |
| NPS | Net Promoter Score | > 50 | 12 meses |

### 1.5 Alcance

**Incluye (In Scope):**

- ✅ [Elemento 1]
- ✅ [Elemento 2]
- ✅ [Elemento 3]

**No Incluye (Out of Scope):**

- ❌ [Elemento 1]
- ❌ [Elemento 2]
- ❌ [Elemento 3]

**Futuras Consideraciones:**

- 🔮 [Elemento que podría agregarse en el futuro]
- 🔮 [Elemento que podría agregarse en el futuro]

---

## 2. Stakeholders

| Tipo          | Nombre/Rol | Responsabilidad   | Interés | Influencia | Contacto |
| ------------- | ---------- | ----------------- | ------- | ---------- | -------- |
| Sponsor       | [Nombre]   | [Responsabilidad] | Alto    | Alta       | [Email]  |
| Product Owner | [Nombre]   | [Responsabilidad] | Alto    | Alta       | [Email]  |
| Usuario Final | [Tipo]     | [Responsabilidad] | Alto    | Media      | -        |
| Proveedor     | [Nombre]   | [Responsabilidad] | Medio   | Baja       | [Email]  |

**Matriz de Poder/Interés:**

```
        Alta Influencia
              │
    Gestionar │ Mantener
    de cerca  │ satisfecho
──────────────┼──────────────
    Mantener  │ Monitorear
    informado │ (mínimo esfuerzo)
              │
        Baja Influencia
```

---

## 3. Tipos de Usuarios (Personas)

### 3.1 [Tipo de Usuario 1]

**Demografía:**

- **Edad:** [Rango]
- **Ocupación:** [Descripción]
- **Nivel técnico:** [Bajo | Medio | Alto]

**Necesidades:**

- [Necesidad 1]
- [Necesidad 2]
- [Necesidad 3]

**Dolores (Pain Points):**

- [Dolor 1]
- [Dolor 2]
- [Dolor 3]

**Expectativas del Sistema:**

- [Expectativa 1]
- [Expectativa 2]
- [Expectativa 3]

**Escenario de Uso:**
[Describe un día típico de este usuario interactuando con el sistema]

### 3.2 [Tipo de Usuario 2]

[Repetir estructura anterior]

---

## 4. Procesos del Negocio

### 4.1 [Nombre del Proceso 1]

**Objetivo:** [Qué se busca lograr con este proceso]

**Actores involucrados:**

- [Actor 1]
- [Actor 2]
- [Actor 3]

**Flujo del Proceso:**

```
1. [Paso 1]
2. [Paso 2]
3. [Paso 3]
   ├─ Si [condición]: [Acción A]
   └─ Si no: [Acción B]
4. [Paso 4]
5. [Paso 5]
```

**Diagrama de Flujo:**
[Insertar diagrama o descripción visual]

**Puntos de Dolor Actuales:**

- ⚠️ [Problema 1]
- ⚠️ [Problema 2]

**Mejoras Propuestas:**

- ✅ [Mejora 1]
- ✅ [Mejora 2]

**Métricas:**

- **Tiempo promedio:** [X minutos/horas]
- **Tasa de éxito:** [X%]
- **Tasa de error:** [X%]

### 4.2 [Nombre del Proceso 2]

[Repetir estructura anterior]

---

## 5. Requerimientos Funcionales

### 5.1 Módulo: [Nombre del Módulo]

#### RF-001: [Nombre del Requerimiento]

**Descripción:**
[Descripción detallada de la funcionalidad]

**Prioridad:** [Alta | Media | Baja]

**Criterios de Aceptación:**

- [ ] [Criterio 1]
- [ ] [Criterio 2]
- [ ] [Criterio 3]

**Dependencias:**

- [RF-XXX]: [Descripción de la dependencia]

**Notas:**
[Información adicional relevante]

---

**Ejemplo Completo:**

### 5.1 Módulo: Autenticación y Autorización

#### RF-001: Registro de Usuario

**Descripción:**
El sistema debe permitir a nuevos usuarios crear una cuenta proporcionando email, contraseña y nombre completo.

**Prioridad:** Alta

**Criterios de Aceptación:**

- [ ] El formulario solicita: email, contraseña, confirmar contraseña, nombre
- [ ] El email debe tener formato válido
- [ ] La contraseña debe tener mínimo 8 caracteres
- [ ] El sistema valida que el email no esté ya registrado
- [ ] El sistema envía email de verificación
- [ ] El usuario puede reenviar el email de verificación si no lo recibe

**Dependencias:**

- [RNF-003]: Integración con servicio de email

**Notas:**
Considerar agregar verificación con CAPTCHA para prevenir bots.

---

#### RF-002: Inicio de Sesión

**Descripción:**
El sistema debe permitir a usuarios registrados autenticarse con email y contraseña.

**Prioridad:** Alta

**Criterios de Aceptación:**

- [ ] El formulario solicita email y contraseña
- [ ] El sistema valida credenciales contra la base de datos
- [ ] Si las credenciales son correctas, se crea una sesión
- [ ] Si las credenciales son incorrectas, se muestra mensaje de error genérico
- [ ] Después de 5 intentos fallidos, se bloquea temporalmente la cuenta (15 minutos)
- [ ] El sistema registra todos los intentos de inicio de sesión

**Dependencias:**

- [RF-001]: Registro de Usuario
- [RNF-005]: Seguridad de contraseñas

**Notas:**
Implementar rate limiting para prevenir ataques de fuerza bruta.

---

### 5.2 Módulo: [Otro Módulo]

[Continuar con más requerimientos funcionales]

---

## 6. Requerimientos No Funcionales

### 6.1 Rendimiento

#### RNF-001: Tiempo de Respuesta

**Descripción:**
El sistema debe responder a las peticiones del usuario en tiempos aceptables.

**Criterios:**

- El 95% de las peticiones deben responder en < 2 segundos
- El 99% de las peticiones deben responder en < 5 segundos
- Las búsquedas deben retornar resultados en < 1 segundo

**Medición:**
[Cómo se medirá este requerimiento]

---

#### RNF-002: Capacidad

**Descripción:**
El sistema debe soportar la carga esperada de usuarios concurrentes.

**Criterios:**

- Soportar 1,000 usuarios concurrentes en operación normal
- Soportar 5,000 usuarios concurrentes en picos (Black Friday, etc.)
- Procesar 100 transacciones por segundo

**Medición:**
Pruebas de carga con herramientas como JMeter o Locust.

---

### 6.2 Seguridad

#### RNF-003: Autenticación y Autorización

**Descripción:**
El sistema debe implementar mecanismos robustos de seguridad.

**Criterios:**

- Todas las contraseñas deben hashearse con bcrypt (cost factor >= 12)
- Las sesiones deben expirar después de 24 horas de inactividad
- Implementar HTTPS/TLS 1.3 para todas las comunicaciones
- Implementar CORS restrictivo
- Validar y sanitizar todas las entradas de usuario

---

#### RNF-004: Protección de Datos

**Descripción:**
El sistema debe cumplir con regulaciones de protección de datos.

**Criterios:**

- Cumplir con GDPR para usuarios europeos
- Implementar derecho al olvido (eliminar datos de usuario)
- Encriptar datos sensibles en reposo (AES-256)
- Registrar auditoría de acceso a datos sensibles

---

### 6.3 Disponibilidad

#### RNF-005: Uptime

**Descripción:**
El sistema debe estar disponible la mayor parte del tiempo.

**Criterios:**

- 99.9% de uptime (máximo 8.76 horas de downtime al año)
- Ventana de mantenimiento: Domingos 2:00 AM - 4:00 AM
- Implementar health checks y monitoreo

---

### 6.4 Escalabilidad

#### RNF-006: Crecimiento

**Descripción:**
El sistema debe poder escalar para soportar crecimiento futuro.

**Criterios:**

- Arquitectura horizontal escalable (agregar más servidores)
- Base de datos debe soportar hasta 10 millones de registros sin degradación
- Implementar caché (Redis) para reducir carga en BD

---

### 6.5 Usabilidad

#### RNF-007: Experiencia de Usuario

**Descripción:**
El sistema debe ser fácil de usar e intuitivo.

**Criterios:**

- Diseño responsive (mobile, tablet, desktop)
- Cumplir con WCAG 2.1 nivel AA (accesibilidad)
- Soportar navegadores: Chrome, Firefox, Safari, Edge (últimas 2 versiones)
- Mensajes de error claros y accionables

---

### 6.6 Mantenibilidad

#### RNF-008: Código y Documentación

**Descripción:**
El código debe ser mantenible y estar bien documentado.

**Criterios:**

- Cobertura de tests >= 80%
- Documentación de API (OpenAPI/Swagger)
- Código debe seguir guías de estilo (ESLint, Prettier)
- Comentarios en código para lógica compleja

---

## 7. Reglas del Negocio

### RN-001: [Nombre de la Regla]

**Descripción:**
[Descripción detallada de la regla]

**Ejemplo:**
[Ejemplo concreto de aplicación]

**Excepciones:**
[Si existen excepciones a esta regla]

---

**Ejemplos Completos:**

### RN-001: Cancelación de Pedidos

**Descripción:**
Un pedido solo puede cancelarse si está en estado "pendiente" o "pagado" y no han transcurrido más de 30 minutos desde su creación.

**Ejemplo:**

- Pedido creado a las 10:00 AM, estado "pagado"
- Usuario intenta cancelar a las 10:25 AM → ✅ Permitido
- Usuario intenta cancelar a las 10:35 AM → ❌ No permitido

**Excepciones:**
Los administradores pueden cancelar pedidos en cualquier momento, pero deben justificar la razón.

---

### RN-002: Límite de Compra por Producto

**Descripción:**
Un usuario no puede comprar más de 10 unidades del mismo producto en un solo pedido.

**Ejemplo:**

- Usuario intenta agregar 15 unidades de "Laptop XYZ" → ❌ Sistema limita a 10

**Excepciones:**
Clientes corporativos (con cuenta verificada) pueden solicitar compras mayores contactando a ventas.

---

### RN-003: Aplicación de Impuestos

**Descripción:**
Los impuestos se calculan según la ubicación del cliente:

- México: IVA 16%
- España: IVA 21%
- USA: Varía por estado (tabla de referencia)

**Ejemplo:**

- Producto: $100
- Cliente en México: Total = $116 (incluye IVA)
- Cliente en España: Total = $121 (incluye IVA)

---

## 8. Modelo de Datos

### 8.1 Entidades Principales

#### Entidad: Usuario

**Descripción:**
Representa a un usuario del sistema.

**Atributos:**
| Atributo | Tipo | Restricciones | Descripción |
|----------|------|---------------|-------------|
| id | Integer | PK, Auto-increment | Identificador único |
| email | String(255) | Único, No nulo | Email del usuario |
| password_hash | String(255) | No nulo | Contraseña hasheada |
| nombre | String(100) | No nulo | Nombre completo |
| rol | Enum | 'cliente', 'admin' | Rol del usuario |
| email_verificado | Boolean | Default: false | Si verificó su email |
| creado_en | DateTime | Default: NOW() | Fecha de registro |
| actualizado_en | DateTime | Default: NOW() | Última actualización |

**Índices:**

- `idx_email` en campo `email`

---

#### Entidad: Producto

**Descripción:**
Representa un producto del catálogo.

**Atributos:**
| Atributo | Tipo | Restricciones | Descripción |
|----------|------|---------------|-------------|
| id | Integer | PK, Auto-increment | Identificador único |
| nombre | String(255) | No nulo | Nombre del producto |
| descripcion | Text | Nullable | Descripción detallada |
| precio | Decimal(10,2) | No nulo, >= 0 | Precio unitario |
| stock | Integer | Default: 0, >= 0 | Cantidad disponible |
| categoria_id | Integer | FK → Categoria.id | Categoría del producto |
| activo | Boolean | Default: true | Si está disponible |
| creado_en | DateTime | Default: NOW() | Fecha de creación |

**Índices:**

- `idx_categoria` en campo `categoria_id`
- `idx_activo` en campo `activo`

---

### 8.2 Relaciones

```
Usuario ──1:N── Pedido
Pedido ──N:M── Producto (a través de PedidoProducto)
Producto ──N:1── Categoria
Pedido ──1:1── Pago
Usuario ──1:N── Direccion
```

**Diagrama ER:**
[Insertar diagrama de entidad-relación]

---

### 8.3 Diccionario de Datos Completo

[Para proyectos grandes, incluir tabla completa de todas las entidades y atributos]

---

## 9. Integraciones

### 9.1 [Nombre de la Integración 1]

**Proveedor:** [Nombre del servicio externo]

**Propósito:**
[Para qué se usa esta integración]

**Tipo de Integración:**

- [ ] API REST
- [ ] API GraphQL
- [ ] Webhook
- [ ] SDK
- [ ] Otro: [Especificar]

**Autenticación:**
[Tipo de autenticación: API Key, OAuth, JWT, etc.]

**Endpoints Utilizados:**
| Endpoint | Método | Propósito |
|----------|--------|-----------|
| [URL] | GET/POST/etc. | [Descripción] |

**Datos Intercambiados:**

- **Enviamos:** [Qué datos enviamos]
- **Recibimos:** [Qué datos recibimos]

**Frecuencia:**
[Con qué frecuencia se usa: por transacción, cada hora, etc.]

**SLA del Proveedor:**
[Disponibilidad garantizada, tiempo de respuesta]

**Plan de Contingencia:**
[Qué hacer si el servicio falla]

**Costos:**
[Modelo de pricing del servicio]

---

**Ejemplo Completo:**

### 9.1 Stripe (Procesamiento de Pagos)

**Proveedor:** Stripe Inc.

**Propósito:**
Procesar pagos con tarjeta de crédito/débito de forma segura y cumpliendo con PCI-DSS.

**Tipo de Integración:**

- [x] API REST
- [x] SDK (JavaScript)

**Autenticación:**
API Key (Secret Key para backend, Publishable Key para frontend)

**Endpoints Utilizados:**
| Endpoint | Método | Propósito |
|----------|--------|-----------|
| `/v1/payment_intents` | POST | Crear intención de pago |
| `/v1/payment_intents/:id` | GET | Consultar estado de pago |
| `/v1/refunds` | POST | Procesar reembolso |

**Datos Intercambiados:**

- **Enviamos:**
  - Monto (en centavos)
  - Moneda (USD, MXN, EUR, etc.)
  - Token de tarjeta (generado por Stripe.js)
  - Metadata (ID de pedido, ID de usuario)
- **Recibimos:**
  - ID de transacción
  - Estado (succeeded, failed, pending)
  - Detalles de error (si aplica)

**Frecuencia:**
Por cada transacción de compra (estimado: 500/día)

**SLA del Proveedor:**

- 99.99% uptime
- Tiempo de respuesta: < 500ms (p95)

**Plan de Contingencia:**

- Implementar cola de reintentos (3 intentos con backoff exponencial)
- Mostrar mensaje al usuario: "Problema temporal, intenta en unos minutos"
- Alertar a equipo de operaciones si falla > 5 minutos

**Costos:**

- 2.9% + $0.30 USD por transacción exitosa
- Sin costo mensual fijo
- Estimado mensual: $1,500 USD (basado en 500 transacciones/día, ticket promedio $50)

---

### 9.2 [Otra Integración]

[Repetir estructura anterior]

---

## 10. Riesgos y Supuestos

### 10.1 Riesgos

#### R-001: [Nombre del Riesgo]

**Categoría:** [Técnico | Legal | Operacional | Financiero]

**Descripción:**
[Descripción detallada del riesgo]

**Probabilidad:** [Alta | Media | Baja]

**Impacto:** [Alto | Medio | Bajo]

**Nivel de Riesgo:** [Probabilidad × Impacto]

**Mitigación:**
[Acciones para reducir probabilidad o impacto]

**Plan de Contingencia:**
[Qué hacer si el riesgo se materializa]

**Responsable:**
[Quién monitorea este riesgo]

---

**Ejemplos:**

#### R-001: Dependencia de API Externa (Stripe)

**Categoría:** Técnico

**Descripción:**
Si Stripe tiene downtime prolongado, no podemos procesar pagos, lo que detiene las ventas completamente.

**Probabilidad:** Baja (Stripe tiene 99.99% uptime)

**Impacto:** Alto (pérdida directa de ingresos)

**Nivel de Riesgo:** Medio

**Mitigación:**

- Implementar sistema de caché para reintentos
- Monitoreo activo del status de Stripe
- Considerar integración con pasarela alternativa (PayPal) como backup

**Plan de Contingencia:**

1. Detectar falla de Stripe
2. Activar modo "mantenimiento programado" en checkout
3. Notificar a clientes vía email/redes sociales
4. Si downtime > 2 horas, activar pasarela alternativa

**Responsable:** Tech Lead

---

#### R-002: Cambios en Regulación de Protección de Datos

**Categoría:** Legal

**Descripción:**
Nuevas leyes de privacidad pueden requerir cambios significativos en cómo manejamos datos de usuarios.

**Probabilidad:** Media

**Impacto:** Alto (multas, rediseño de sistema)

**Nivel de Riesgo:** Alto

**Mitigación:**

- Diseñar arquitectura modular para facilitar cambios
- Implementar desde el inicio: consentimiento explícito, derecho al olvido, portabilidad de datos
- Consultar con asesor legal especializado en protección de datos

**Plan de Contingencia:**

1. Monitoreo continuo de cambios legislativos
2. Presupuesto de contingencia (20% del presupuesto técnico)
3. Equipo legal en retainer

**Responsable:** Legal + CTO

---

### 10.2 Supuestos

#### S-001: [Nombre del Supuesto]

**Descripción:**
[Qué estamos asumiendo]

**Impacto si es falso:**
[Qué pasa si este supuesto no se cumple]

**Validación:**
[Cómo validaremos este supuesto]

---

**Ejemplos:**

#### S-001: Acceso a Internet Estable

**Descripción:**
Asumimos que los usuarios tienen acceso a internet estable con velocidad >= 1 Mbps.

**Impacto si es falso:**
La aplicación puede ser lenta o inusable para usuarios con conexiones pobres.

**Validación:**

- Analítica de velocidad de conexión de usuarios
- Pruebas de usabilidad en conexiones 3G

**Mitigación si es falso:**

- Implementar modo offline limitado
- Optimizar assets (imágenes, JS)
- Implementar lazy loading

---

#### S-002: Volumen Inicial de Usuarios

**Descripción:**
Asumimos que en los primeros 6 meses no excederemos 10,000 usuarios activos mensuales.

**Impacto si es falso:**
Si crecemos más rápido, podemos tener problemas de rendimiento o costos de infraestructura no presupuestados.

**Validación:**

- Monitoreo de crecimiento semanal
- Alertas cuando alcancemos 70% de capacidad

**Mitigación si es falso:**

- Plan de escalamiento preparado
- Presupuesto de contingencia para infraestructura
- Arquitectura diseñada para escalar horizontalmente

---

## 11. Roadmap / Fases

### Fase 1: MVP (Producto Mínimo Viable)

**Duración:** [X semanas/meses]

**Objetivo:**
[Qué se busca lograr con el MVP]

**Funcionalidades Incluidas:**

- ✅ [Funcionalidad 1] - [RF-XXX]
- ✅ [Funcionalidad 2] - [RF-XXX]
- ✅ [Funcionalidad 3] - [RF-XXX]

**Criterios de Éxito:**

- [Criterio 1]
- [Criterio 2]

**Entregables:**

- [Entregable 1]
- [Entregable 2]

---

### Fase 2: [Nombre de la Fase]

**Duración:** [X semanas/meses]

**Objetivo:**
[Qué se busca lograr]

**Funcionalidades Incluidas:**

- ✅ [Funcionalidad 1]
- ✅ [Funcionalidad 2]

**Dependencias:**

- Completar Fase 1
- [Otra dependencia]

---

### Fase 3: [Nombre de la Fase]

[Repetir estructura]

---

**Diagrama de Gantt:**
[Insertar diagrama temporal de fases]

---

## 12. Anexos

### 12.1 Glosario

| Término     | Definición   |
| ----------- | ------------ |
| [Término 1] | [Definición] |
| [Término 2] | [Definición] |

### 12.2 Referencias

- [Documento 1]
- [Documento 2]
- [URL de recurso externo]

### 12.3 Historial de Cambios

| Versión | Fecha   | Autor    | Cambios                  |
| ------- | ------- | -------- | ------------------------ |
| 1.0     | [Fecha] | [Nombre] | Versión inicial          |
| 1.1     | [Fecha] | [Nombre] | [Descripción de cambios] |

---

## Aprobaciones

| Rol           | Nombre   | Firma | Fecha |
| ------------- | -------- | ----- | ----- |
| Product Owner | [Nombre] |       |       |
| Tech Lead     | [Nombre] |       |       |
| Stakeholder   | [Nombre] |       |       |

---

**Fin del Documento**
