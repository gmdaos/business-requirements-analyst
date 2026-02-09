# Casos de Uso Detallados

Este documento proporciona guías y plantillas para documentar casos de uso de forma profesional.

## ¿Qué es un Caso de Uso?

Un caso de uso describe cómo un actor (usuario o sistema) interactúa con el sistema para lograr un objetivo específico.

**Componentes clave:**

- **Actor:** Quién inicia la interacción
- **Objetivo:** Qué quiere lograr
- **Precondiciones:** Estado necesario antes de empezar
- **Flujo principal:** Pasos normales de ejecución
- **Flujos alternativos:** Variaciones del flujo principal
- **Postcondiciones:** Estado del sistema después de completar

## Formato Estándar de Caso de Uso

### Plantilla Básica

```markdown
## CU-[ID]: [Nombre del Caso de Uso]

**Actor principal:** [Nombre del actor]
**Objetivo:** [Qué quiere lograr el actor]
**Precondiciones:** [Condiciones que deben cumplirse antes]

### Flujo Principal

1. [Paso 1]
2. [Paso 2]
3. [Paso 3]
   ...

### Flujos Alternativos

**FA-[ID]: [Nombre del flujo alternativo]**

- **Trigger:** [Cuándo se activa]
- **Pasos:**
  1. [Paso 1]
  2. [Paso 2]
     ...

### Excepciones

**EX-[ID]: [Nombre de la excepción]**

- **Condición:** [Cuándo ocurre]
- **Acción:** [Qué hacer]

### Postcondiciones

**Éxito:** [Estado del sistema si todo sale bien]
**Fallo:** [Estado del sistema si falla]

### Reglas de Negocio

- **RN-[ID]:** [Regla aplicable]

### Requerimientos Relacionados

- RF-[ID]: [Requerimiento funcional]
- RNF-[ID]: [Requerimiento no funcional]
```

## Niveles de Detalle

### Nivel 1: Alto Nivel (Brief)

**Cuándo usar:** Documentación inicial, presentaciones ejecutivas.

**Ejemplo:**

```markdown
## CU-001: Comprar Producto

**Actor:** Cliente
**Objetivo:** Adquirir un producto del catálogo

El cliente navega el catálogo, selecciona productos, los agrega al carrito,
procede al checkout, ingresa información de pago y envío, y confirma la compra.
```

### Nivel 2: Casual (Casual)

**Cuándo usar:** Planificación, discusiones de equipo.

**Ejemplo:**

```markdown
## CU-001: Comprar Producto

**Actor:** Cliente
**Objetivo:** Adquirir un producto del catálogo

### Flujo Principal

1. Cliente navega catálogo de productos
2. Cliente selecciona producto y lo agrega al carrito
3. Cliente procede al checkout
4. Cliente ingresa información de envío
5. Cliente selecciona método de pago
6. Cliente confirma la compra
7. Sistema procesa el pago
8. Sistema confirma el pedido y envía email
```

### Nivel 3: Completamente Detallado (Fully Dressed)

**Cuándo usar:** Desarrollo, QA, documentación formal.

**Ejemplo completo a continuación.**

## Ejemplo Completo: Caso de Uso Detallado

```markdown
## CU-001: Realizar Compra de Producto

**ID:** CU-001
**Nombre:** Realizar Compra de Producto
**Actor principal:** Cliente registrado
**Actores secundarios:** Sistema de pagos (Stripe), Sistema de email (SendGrid)
**Objetivo:** Permitir al cliente comprar productos del catálogo
**Nivel:** Usuario
**Alcance:** Sistema de e-commerce

### Precondiciones

- El cliente debe estar autenticado
- Debe existir al menos un producto con stock disponible
- El sistema de pagos debe estar operativo

### Garantías Mínimas (si falla)

- No se cobra al cliente
- El stock no se reduce
- Se registra el intento fallido en logs

### Garantías de Éxito

- Se crea un pedido confirmado
- Se reduce el stock de productos
- Se cobra al cliente
- Se envía email de confirmación
- Se genera orden de envío

### Flujo Principal (Escenario de Éxito)

1. Cliente accede a la página de catálogo
2. Sistema muestra productos disponibles con:
   - Imagen
   - Nombre
   - Precio
   - Indicador de stock
3. Cliente busca/filtra productos (opcional)
4. Sistema actualiza listado según criterios
5. Cliente selecciona un producto
6. Sistema muestra detalle del producto:
   - Descripción completa
   - Imágenes adicionales
   - Reseñas de otros clientes
   - Stock disponible
7. Cliente selecciona cantidad deseada
8. Cliente hace clic en "Agregar al carrito"
9. Sistema valida stock disponible
10. Sistema agrega producto al carrito
11. Sistema muestra notificación de confirmación
12. Cliente continúa comprando (volver a paso 3) o procede al checkout
13. Cliente hace clic en "Proceder al pago"
14. Sistema muestra resumen del carrito:
    - Lista de productos
    - Cantidades
    - Precios unitarios
    - Subtotal
    - Impuestos
    - Total
15. Cliente revisa y hace clic en "Continuar"
16. Sistema solicita dirección de envío
17. Cliente selecciona dirección guardada o ingresa nueva:
    - Nombre completo
    - Calle y número
    - Ciudad
    - Código postal
    - País
18. Sistema valida formato de dirección
19. Cliente hace clic en "Continuar"
20. Sistema muestra métodos de pago disponibles:
    - Tarjeta de crédito/débito
    - PayPal
    - Transferencia bancaria
21. Cliente selecciona "Tarjeta de crédito"
22. Sistema muestra formulario de pago (Stripe)
23. Cliente ingresa datos de tarjeta:
    - Número de tarjeta
    - Fecha de vencimiento
    - CVV
    - Nombre del titular
24. Sistema valida formato de datos
25. Cliente hace clic en "Confirmar compra"
26. Sistema muestra pantalla de "Procesando..."
27. Sistema crea pedido en estado "pendiente_pago"
28. Sistema reserva stock de productos
29. Sistema envía solicitud de pago a Stripe con:
    - Monto total
    - Datos de tarjeta (tokenizados)
    - Descripción del pedido
30. Stripe procesa el pago
31. Stripe retorna confirmación de pago exitoso
32. Sistema actualiza pedido a estado "pagado"
33. Sistema reduce stock de productos
34. Sistema genera registro de pago
35. Sistema crea orden de envío
36. Sistema envía email de confirmación vía SendGrid con:
    - Número de pedido
    - Resumen de productos
    - Dirección de envío
    - Monto total
    - Enlace de seguimiento
37. Sistema muestra página de confirmación con:
    - Mensaje de éxito
    - Número de pedido
    - Tiempo estimado de entrega
38. **Fin del caso de uso**

### Flujos Alternativos

#### FA-1: Cliente No Autenticado

**Trigger:** Paso 1, cliente no está autenticado

**Pasos:**

1. Sistema detecta que no hay sesión activa
2. Sistema redirige a página de login
3. Sistema muestra opciones:
   - Iniciar sesión
   - Registrarse
   - Continuar como invitado
4. Si cliente inicia sesión o se registra:
   - Continuar en paso 1 del flujo principal
5. Si cliente continúa como invitado:
   - Continuar en paso 1 del flujo principal
   - En paso 17, solicitar también email para confirmación

#### FA-2: Producto Sin Stock

**Trigger:** Paso 9, stock insuficiente

**Pasos:**

1. Sistema detecta que stock < cantidad solicitada
2. Sistema muestra mensaje: "Stock insuficiente. Disponible: [X] unidades"
3. Sistema ofrece opciones:
   - Ajustar cantidad a stock disponible
   - Notificarme cuando haya stock
   - Cancelar
4. Si cliente ajusta cantidad:
   - Continuar en paso 10 con cantidad ajustada
5. Si cliente solicita notificación:
   - Sistema registra solicitud de notificación
   - Volver a paso 3 del flujo principal
6. Si cliente cancela:
   - Volver a paso 3 del flujo principal

#### FA-3: Cliente Usa Dirección Guardada

**Trigger:** Paso 17, cliente tiene direcciones guardadas

**Pasos:**

1. Sistema muestra lista de direcciones guardadas
2. Cliente selecciona una dirección
3. Sistema pre-llena formulario con datos guardados
4. Cliente puede editar datos si lo desea
5. Continuar en paso 19 del flujo principal

#### FA-4: Pago con PayPal

**Trigger:** Paso 21, cliente selecciona PayPal

**Pasos:**

1. Sistema redirige a página de PayPal
2. Cliente inicia sesión en PayPal
3. Cliente autoriza el pago
4. PayPal redirige de vuelta al sistema con token
5. Continuar en paso 27 del flujo principal

#### FA-5: Cliente Aplica Cupón de Descuento

**Trigger:** Paso 14, cliente tiene cupón

**Pasos:**

1. Cliente hace clic en "Tengo un cupón"
2. Sistema muestra campo de texto
3. Cliente ingresa código de cupón
4. Sistema valida cupón:
   - Existe
   - No está expirado
   - Aplica a los productos del carrito
   - No ha sido usado (si es de un solo uso)
5. Sistema aplica descuento
6. Sistema actualiza totales
7. Sistema muestra descuento aplicado
8. Continuar en paso 15 del flujo principal

### Excepciones

#### EX-1: Pago Rechazado

**Condición:** Paso 31, Stripe rechaza el pago

**Acción:**

1. Sistema recibe error de Stripe con razón:
   - Fondos insuficientes
   - Tarjeta expirada
   - Tarjeta bloqueada
   - Error de validación
2. Sistema actualiza pedido a estado "pago_fallido"
3. Sistema libera reserva de stock
4. Sistema muestra mensaje de error específico al cliente
5. Sistema ofrece opciones:
   - Intentar con otra tarjeta
   - Intentar con otro método de pago
   - Cancelar compra
6. Si cliente intenta de nuevo:
   - Volver a paso 22 del flujo principal
7. Si cliente cancela:
   - Sistema marca pedido como "cancelado"
   - Fin del caso de uso (fallo)

#### EX-2: Error de Comunicación con Stripe

**Condición:** Paso 29, no hay respuesta de Stripe (timeout)

**Acción:**

1. Sistema detecta timeout después de 30 segundos
2. Sistema registra error en logs
3. Sistema marca pedido como "pago_pendiente_verificacion"
4. Sistema muestra mensaje al cliente:
   "Estamos verificando tu pago. Recibirás un email de confirmación en los próximos minutos."
5. Sistema programa tarea asíncrona para:
   - Consultar estado del pago en Stripe cada 1 minuto
   - Máximo 10 intentos
6. Si se confirma el pago:
   - Continuar en paso 32 del flujo principal
7. Si no se confirma después de 10 intentos:
   - Sistema marca pedido como "requiere_revision_manual"
   - Sistema alerta a equipo de operaciones
   - Sistema envía email al cliente solicitando contacto

#### EX-3: Error al Enviar Email

**Condición:** Paso 36, SendGrid retorna error

**Acción:**

1. Sistema registra error en logs
2. Sistema marca email como "pendiente_reenvio"
3. Sistema programa reintento en 5 minutos
4. **Importante:** No bloquear el flujo, el pedido ya está confirmado
5. Continuar en paso 37 del flujo principal
6. Tarea asíncrona reintenta envío hasta 3 veces
7. Si falla definitivamente:
   - Sistema alerta a equipo de operaciones
   - Email queda disponible en panel de administración para reenvío manual

#### EX-4: Stock Agotado Durante el Proceso

**Condición:** Paso 28, al intentar reservar stock ya no hay disponible

**Acción:**

1. Sistema detecta que otro cliente compró el último stock
2. Sistema NO procesa el pago
3. Sistema muestra mensaje:
   "Lo sentimos, el producto [X] se agotó mientras procesábamos tu pedido."
4. Sistema ofrece opciones:
   - Eliminar producto del carrito y continuar con los demás
   - Notificarme cuando haya stock
   - Cancelar toda la compra
5. Si cliente elimina producto:
   - Sistema actualiza carrito
   - Volver a paso 14 del flujo principal
6. Si cliente cancela:
   - Fin del caso de uso (fallo)

### Postcondiciones

**Éxito:**

- Existe un pedido en estado "pagado"
- Stock de productos reducido
- Pago registrado y confirmado
- Email de confirmación enviado
- Orden de envío creada

**Fallo:**

- Pedido en estado "cancelado" o "pago_fallido"
- Stock no modificado
- No se realizó cargo al cliente
- Error registrado en logs

### Reglas de Negocio

- **RN-01:** El stock se reserva solo después de confirmar el pago
- **RN-02:** Los precios mostrados incluyen impuestos
- **RN-03:** Un cupón solo puede usarse una vez por usuario
- **RN-04:** Los cupones expiran a las 23:59 de la fecha indicada
- **RN-05:** El carrito se mantiene por 7 días para usuarios autenticados
- **RN-06:** El carrito se mantiene por 24 horas para invitados (cookie)
- **RN-07:** No se permite comprar más de 10 unidades del mismo producto
- **RN-08:** Los pagos se procesan en la moneda local del cliente

### Requerimientos Relacionados

**Funcionales:**

- RF-01: El sistema debe permitir registro de usuarios
- RF-05: El sistema debe gestionar catálogo de productos
- RF-12: El sistema debe procesar pagos con Stripe
- RF-18: El sistema debe enviar emails de confirmación
- RF-23: El sistema debe gestionar stock de productos

**No Funcionales:**

- RNF-01: El proceso de pago debe completarse en < 5 segundos
- RNF-05: El sistema debe usar HTTPS para todas las transacciones
- RNF-08: Los datos de tarjeta deben tokenizarse (PCI-DSS)
- RNF-12: El sistema debe soportar 100 compras concurrentes

### Frecuencia de Uso

- **Estimada:** 500 compras/día
- **Pico:** 2000 compras/día (Black Friday, Cyber Monday)

### Prioridad

**Alta** - Funcionalidad core del negocio

### Notas Adicionales

- Considerar implementar sistema de "compra en 1 clic" para usuarios frecuentes
- Evaluar agregar opción de "guardar para después" en el carrito
- Futuro: Integrar con programa de puntos de lealtad
```

## Diagramas de Casos de Uso (UML)

**Representación visual de actores y casos de uso.**

```
┌─────────────────────────────────────────────┐
│         Sistema E-commerce                  │
│                                             │
│  ┌──────────────────┐                      │
│  │ Buscar Producto  │                      │
│  └──────────────────┘                      │
│           │                                 │
│  ┌──────────────────┐                      │
│  │ Agregar a Carrito│                      │
│  └──────────────────┘                      │
│           │                                 │
│  ┌──────────────────┐    ┌──────────────┐ │
│  │ Realizar Compra  │───→│ Procesar Pago│ │
│  └──────────────────┘    └──────────────┘ │
│           │                      │         │
│           │              ┌───────┴────┐    │
│           │              │   Stripe   │    │
│           │              └────────────┘    │
│  ┌──────────────────┐                      │
│  │ Ver Historial    │                      │
│  └──────────────────┘                      │
│                                             │
└─────────────────────────────────────────────┘
         │
    ┌────┴────┐
    │ Cliente │
    └─────────┘
```

## Matriz de Trazabilidad

**Relacionar casos de uso con requerimientos.**

| Caso de Uso               | RF-01 | RF-05 | RF-12 | RF-18 | RNF-01 | RNF-05 |
| ------------------------- | ----- | ----- | ----- | ----- | ------ | ------ |
| CU-001: Realizar Compra   | X     | X     | X     | X     | X      | X      |
| CU-002: Gestionar Carrito |       | X     |       |       | X      |        |
| CU-003: Ver Historial     | X     |       |       |       | X      |        |

## Consejos para Escribir Buenos Casos de Uso

1. **Usa lenguaje del negocio** - No términos técnicos
2. **Enfócate en el objetivo** - Qué quiere lograr el usuario, no cómo
3. **Sé específico en flujos alternativos** - Cubre edge cases
4. **Documenta excepciones** - Qué pasa cuando falla
5. **Mantén consistencia** - Usa la misma plantilla para todos
6. **Revisa con stakeholders** - Valida que refleja la realidad
7. **Actualiza cuando cambie** - Documento vivo

## Herramientas Recomendadas

- **Enterprise Architect** - Modelado UML profesional
- **Lucidchart** - Diagramas colaborativos
- **Confluence** - Documentación de casos de uso
- **Jira** - Vincular casos de uso con historias de usuario
