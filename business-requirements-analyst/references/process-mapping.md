# Mapeo de Procesos Complejos

Este documento proporciona técnicas avanzadas para mapear procesos de negocio complejos.

## Tipos de Diagramas de Procesos

### 1. Diagrama de Flujo Básico

**Cuándo usar:** Procesos lineales con pocas decisiones.

**Símbolos principales:**

- **Óvalo:** Inicio/Fin
- **Rectángulo:** Acción/Proceso
- **Rombo:** Decisión
- **Flecha:** Flujo

**Ejemplo:**

```
[Inicio] → [Validar datos] → {¿Válidos?}
                                ↓ Sí        ↓ No
                          [Procesar]    [Mostrar error]
                                ↓              ↓
                             [Fin]         [Fin]
```

### 2. BPMN (Business Process Model and Notation)

**Cuándo usar:** Procesos empresariales complejos con múltiples actores.

**Elementos clave:**

- **Eventos:** Círculos (inicio, intermedio, fin)
- **Actividades:** Rectángulos redondeados
- **Gateways:** Rombos (decisiones, paralelo, exclusivo)
- **Swimlanes:** Carriles por actor/rol

**Ejemplo de estructura:**

```
┌─────────────────────────────────────────┐
│ Cliente                                 │
├─────────────────────────────────────────┤
│ (○) → [Hacer pedido] → [Pagar]         │
└──────────────┬──────────────────────────┘
               ↓
┌──────────────┴──────────────────────────┐
│ Sistema                                 │
├─────────────────────────────────────────┤
│ [Validar pago] → ◇ → [Confirmar]       │
└──────────────┬──────────────────────────┘
               ↓
┌──────────────┴──────────────────────────┐
│ Almacén                                 │
├─────────────────────────────────────────┤
│ [Preparar envío] → [Despachar] → (●)   │
└─────────────────────────────────────────┘
```

### 3. Diagrama de Estados

**Cuándo usar:** Modelar estados de una entidad (pedido, usuario, etc.).

**Ejemplo: Estados de un Pedido**

```
[Creado] → [Pagado] → [En preparación] → [Enviado] → [Entregado]
    ↓          ↓              ↓
[Cancelado] ← [Cancelado] ← [Cancelado]
```

## Técnicas de Mapeo

### 1. Mapeo de Proceso Actual (AS-IS)

**Objetivo:** Documentar cómo funciona actualmente el proceso.

**Pasos:**

1. Identificar inicio y fin del proceso
2. Listar todas las actividades en orden
3. Identificar decisiones y bifurcaciones
4. Documentar actores involucrados
5. Marcar puntos de dolor (bottlenecks, errores frecuentes)

**Ejemplo:**

```markdown
## Proceso Actual: Gestión de Reclamos

### Actores

- Cliente
- Agente de soporte
- Supervisor
- Sistema

### Flujo

1. **Cliente** envía email con reclamo
2. **Sistema** crea ticket automático
3. **Agente** revisa email manualmente
4. **Agente** clasifica reclamo
5. **Decisión:** ¿Requiere supervisor?
   - Sí → **Supervisor** revisa y aprueba
   - No → **Agente** procede
6. **Agente** responde al cliente
7. **Sistema** cierra ticket

### Puntos de Dolor

- ⚠️ Paso 3: Demora promedio de 4 horas
- ⚠️ Paso 5: Criterios de escalación no claros
```

### 2. Mapeo de Proceso Deseado (TO-BE)

**Objetivo:** Diseñar cómo debería funcionar el proceso optimizado.

**Ejemplo:**

```markdown
## Proceso Optimizado: Gestión de Reclamos

### Mejoras Implementadas

- ✅ Clasificación automática con IA
- ✅ Criterios de escalación definidos
- ✅ SLA de respuesta: 2 horas

### Flujo

1. **Cliente** envía reclamo por formulario web
2. **Sistema** clasifica automáticamente (IA)
3. **Sistema** asigna a agente según categoría
4. **Agente** recibe notificación inmediata
5. **Decisión automática:** ¿Monto > $500 O categoría crítica?
   - Sí → **Supervisor** notificado automáticamente
   - No → **Agente** procede
6. **Agente** responde en < 2 horas
7. **Sistema** solicita feedback al cliente
8. **Sistema** cierra ticket si cliente confirma
```

## Patrones Comunes de Procesos

### Patrón 1: Aprobación en Cadena

**Escenario:** Múltiples niveles de aprobación.

```
[Solicitud] → [Aprobador 1] → ◇ ¿Aprueba?
                                ↓ Sí
                          [Aprobador 2] → ◇ ¿Aprueba?
                                            ↓ Sí
                                        [Ejecutar]

                                ↓ No (en cualquier nivel)
                              [Rechazar]
```

### Patrón 2: Procesamiento Paralelo

**Escenario:** Tareas que pueden ejecutarse simultáneamente.

```
[Inicio] → ┬→ [Tarea A] ┬→ [Sincronizar] → [Continuar]
           ├→ [Tarea B] ┤
           └→ [Tarea C] ┘
```

### Patrón 3: Compensación (Rollback)

**Escenario:** Deshacer acciones si algo falla.

```
[Reservar hotel] → [Reservar vuelo] → ◇ ¿Éxito?
                                        ↓ No
                                   [Cancelar hotel]
                                        ↓
                                   [Notificar error]
```

### Patrón 4: Retry con Backoff

**Escenario:** Reintentar operación con espera incremental.

```
[Intentar operación] → ◇ ¿Éxito?
                        ↓ No
                   ◇ ¿Intentos < 3?
                    ↓ Sí
              [Esperar 2^intentos segundos]
                    ↓
              [Incrementar contador]
                    ↓
              [Intentar operación]

                    ↓ No (intentos >= 3)
                [Fallar definitivamente]
```

## Documentación de Excepciones

**Importante:** Documentar qué pasa cuando las cosas fallan.

### Plantilla de Excepción

```markdown
### Excepción: [Nombre]

**Condición:** [Cuándo ocurre]
**Impacto:** [Qué afecta]
**Acción:** [Qué hacer]
**Responsable:** [Quién lo maneja]

**Ejemplo:**

### Excepción: Pago Rechazado

**Condición:** La pasarela de pago rechaza la transacción
**Impacto:** El pedido no se confirma
**Acción:**

1. Mostrar mensaje de error al usuario
2. Permitir cambiar método de pago
3. Registrar intento fallido en logs
4. Enviar email al usuario con instrucciones
   **Responsable:** Sistema (automático)
```

## Métricas de Proceso

Definir KPIs para cada proceso:

```markdown
## Métricas: Proceso de Compra

- **Tiempo promedio:** 3 minutos
- **Tasa de abandono:** < 20%
- **Tasa de éxito de pago:** > 95%
- **Tiempo de respuesta del sistema:** < 2 segundos
```

## Herramientas Recomendadas

- **Lucidchart** - Diagramas BPMN profesionales
- **Draw.io** - Gratuito, integración con Google Drive
- **Miro** - Colaborativo, ideal para workshops
- **PlantUML** - Diagramas como código (versionable)

## Ejemplo Completo: Proceso de Onboarding

```markdown
## Proceso: Onboarding de Usuario

### Objetivo

Registrar y activar nuevos usuarios en menos de 5 minutos.

### Actores

- Usuario nuevo
- Sistema
- Servicio de email (SendGrid)

### Flujo Principal

1. **Usuario** accede a /registro
2. **Sistema** muestra formulario
3. **Usuario** completa datos:
   - Email
   - Contraseña
   - Nombre
4. **Sistema** valida datos:
   - Email formato válido
   - Contraseña >= 8 caracteres
   - Nombre no vacío
5. **Decisión:** ¿Datos válidos?
   - No → Mostrar errores, volver a paso 3
   - Sí → Continuar
6. **Sistema** verifica email único en BD
7. **Decisión:** ¿Email ya existe?
   - Sí → Mostrar "Email ya registrado", volver a paso 3
   - No → Continuar
8. **Sistema** hashea contraseña (bcrypt)
9. **Sistema** crea usuario en BD
10. **Sistema** genera token de verificación
11. **Sistema** envía email de verificación vía SendGrid
12. **Sistema** muestra mensaje "Revisa tu email"
13. **Usuario** abre email y hace clic en link
14. **Sistema** valida token
15. **Decisión:** ¿Token válido y no expirado?
    - No → Mostrar error, ofrecer reenviar
    - Sí → Continuar
16. **Sistema** marca email como verificado
17. **Sistema** autentica usuario automáticamente
18. **Sistema** redirige a dashboard
19. **Fin**

### Flujos Alternativos

#### FA-1: Token Expirado

- **Trigger:** Paso 15, token expirado (> 24 horas)
- **Acción:**
  1. Mostrar "Link expirado"
  2. Ofrecer botón "Reenviar email"
  3. Si usuario acepta, volver a paso 10

#### FA-2: Error al Enviar Email

- **Trigger:** Paso 11, SendGrid retorna error
- **Acción:**
  1. Registrar error en logs
  2. Marcar usuario como "pendiente verificación"
  3. Mostrar mensaje "Problema temporal, intenta más tarde"
  4. Programar reintento automático en 5 minutos

### Excepciones

#### EX-1: Base de Datos No Disponible

- **Condición:** BD no responde en paso 6 o 9
- **Impacto:** No se puede completar registro
- **Acción:**
  1. Mostrar "Servicio temporalmente no disponible"
  2. Registrar en logs de monitoreo
  3. Alertar a equipo de operaciones
- **Responsable:** Sistema + Ops

### Reglas de Negocio

- **RN-01:** El token de verificación expira en 24 horas
- **RN-02:** La contraseña debe tener mínimo 8 caracteres
- **RN-03:** No se permiten emails desechables (usar lista de dominios bloqueados)

### Métricas

- **Tiempo promedio de registro:** < 2 minutos
- **Tasa de verificación de email:** > 70% en 24 horas
- **Tasa de error:** < 1%

### Mejoras Futuras

- [ ] Registro con Google/Facebook OAuth
- [ ] Verificación de email en 2 pasos
- [ ] Captcha para prevenir bots
```

## Consejos Finales

1. **Empieza simple** - No intentes mapear todo de una vez
2. **Valida con usuarios** - Muestra los diagramas a quienes ejecutan el proceso
3. **Itera** - Los procesos evolucionan, el mapa también debe hacerlo
4. **Usa notación consistente** - Elige un estándar (BPMN, UML) y mantenlo
5. **Documenta decisiones** - Por qué se eligió cierto flujo sobre otro
