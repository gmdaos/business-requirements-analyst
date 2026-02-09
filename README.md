# 📋 Business Requirements Analyst - Claude AI Skill

> Metodología profesional completa para levantamiento de requerimientos de software y negocios

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude AI](https://img.shields.io/badge/Claude-AI%20Skill-blue)](https://claude.ai)

---

## 🎯 ¿Qué es este Skill?

**Business Requirements Analyst** es un Skill para Claude AI que implementa una metodología profesional de levantamiento de requerimientos, combinando:

- ✅ **Business Analysis (BABOK)** - Análisis de negocio profesional
- ✅ **Design Thinking** - Validación de ideas centrada en usuarios
- ✅ **Documentación Completa** - Plantillas listas para usar

### ¿Qué produce?

Un **documento maestro de requerimientos** que incluye:

1. 📌 Visión del Negocio (problema, propuesta de valor, KPIs)
2. 👥 Stakeholders y Personas
3. 🔄 Procesos del Negocio (flujos detallados)
4. ⚙️ Requerimientos Funcionales (RF-001, RF-002...)
5. 🚀 Requerimientos No Funcionales (rendimiento, seguridad, escalabilidad)
6. 📊 Modelo de Datos (entidades, relaciones)
7. 🔌 Integraciones (APIs, servicios externos)
8. ⚠️ Riesgos y Supuestos
9. 🗺️ Roadmap en Fases (MVP, Fase 2, Fase 3...)

---

## 🚀 Instalación

### Opción 1: NPX (Recomendado)

```bash
npx skills add https://github.com/TU-USUARIO/business-requirements-analyst-skill --skill business-requirements-analyst
```

### Opción 2: Instalación Manual

#### En Claude Code:

```bash
# Clonar el repositorio
git clone https://github.com/TU-USUARIO/business-requirements-analyst-skill.git

# Copiar a tu proyecto
cp -r business-requirements-analyst-skill/business-requirements-analyst .agent/skills/
```

#### En Claude.ai:

1. Descarga el archivo [business-requirements-analyst.skill](https://github.com/TU-USUARIO/business-requirements-analyst-skill/releases/latest)
2. Ve a **Settings** → **Features** → **Skills**
3. Haz clic en **Upload Custom Skill**
4. Selecciona el archivo `.skill`

#### En Claude API:

```python
import anthropic

client = anthropic.Anthropic(api_key="tu-api-key")

# Subir el Skill
with open("business-requirements-analyst.skill", "rb") as f:
    skill = client.skills.create(file=f)

# Usar en conversación
message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    extra_headers={
        "anthropic-beta": "code-execution-2025-08-25,skills-2025-10-02,files-api-2025-04-14"
    },
    tools=[{
        "type": "code_execution_2025_08_25",
        "container": {
            "skill_id": "business-requirements-analyst"
        }
    }],
    messages=[{
        "role": "user",
        "content": "Ayúdame a documentar mi proyecto"
    }]
)
```

---

## 💡 Uso

### Activación Automática

El Skill se activa automáticamente cuando:

- Mencionas "levantamiento de requerimientos"
- Pides "documentar un proyecto nuevo"
- Solicitas "especificaciones técnicas"
- Necesitas "validar una idea de negocio"

### Ejemplo de Conversación

```
👤 Usuario:
"Necesito documentar mi proyecto de app de delivery de comida"

🤖 Claude (con Skill activo):
"Perfecto, voy a ayudarte con un levantamiento de requerimientos
completo usando la metodología Business Analysis.

Empecemos por la visión del negocio:

1. ¿Qué problema específico resuelve tu app?
2. ¿Quiénes son tus usuarios principales?
3. ¿Qué hace única a tu app vs. competidores?
..."

[Claude te guía paso a paso]

🤖 Claude (al final):
"He generado tu documento completo de requerimientos con:
- 15 requerimientos funcionales
- 8 requerimientos no funcionales
- 3 procesos mapeados
- Modelo de datos con 8 entidades
- Roadmap en 3 fases"
```

---

## 📚 Contenido del Skill

### Archivos Principales

- **SKILL.md** - Metodología principal y guías de uso
- **references/** - Documentación técnica detallada
  - `process-mapping.md` - Técnicas de mapeo de procesos (BPMN, flujos)
  - `data-modeling.md` - Modelado de datos, normalización, patrones
  - `use-cases.md` - Casos de uso detallados con plantillas
- **assets/** - Recursos y plantillas
  - `requirements-template.md` - Plantilla completa lista para usar

---

## 🎓 Metodologías Incluidas

### 1. Design Thinking

Para validación de ideas y descubrimiento de necesidades:

- Empatizar con usuarios
- Definir problemas
- Idear soluciones
- Prototipar
- Testear

### 2. Business Analysis (BABOK)

Para documentación profesional:

- Requerimientos del negocio
- Requerimientos funcionales y no funcionales
- Reglas del negocio
- Stakeholders y procesos

### 3. Lean Startup

Para visión rápida y validación:

- Canvas de modelo de negocio
- MVP definition
- Métricas clave

---

## 📖 Casos de Uso

### ✅ Ideal para:

- 🚀 **Startups** - Validar y documentar ideas de negocio
- 💼 **Consultores** - Crear especificaciones para clientes
- 👨‍💻 **Desarrolladores** - Entender alcance antes de cotizar
- 🏢 **Product Managers** - Documentar roadmap de producto
- 💰 **Inversores** - Evaluar viabilidad de proyectos

### 📋 Tipos de Proyectos:

- E-commerce / Marketplaces
- SaaS / Plataformas web
- Apps móviles
- Sistemas empresariales (ERP, CRM)
- APIs y microservicios
- Cualquier proyecto de software

---

## 🛠️ Requisitos

### Para Claude Code:

- Claude Code instalado
- Proyecto con estructura `.agent/skills/`

### Para Claude.ai:

- Plan Pro, Max, Team o Enterprise
- Code execution habilitado

### Para Claude API:

- API Key de Anthropic
- Headers beta habilitados:
  - `code-execution-2025-08-25`
  - `skills-2025-10-02`
  - `files-api-2025-04-14`

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas!

1. Fork el repositorio
2. Crea una rama: `git checkout -b feature/nueva-funcionalidad`
3. Commit tus cambios: `git commit -m 'Agrega nueva funcionalidad'`
4. Push a la rama: `git push origin feature/nueva-funcionalidad`
5. Abre un Pull Request

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

---

## 🙏 Créditos

Desarrollado por [Tu Nombre]

Basado en metodologías de:

- BABOK (Business Analysis Body of Knowledge)
- Design Thinking (IDEO, Stanford d.school)
- Lean Startup (Eric Ries)

---

## 📞 Soporte

- 🐛 **Issues**: [GitHub Issues](https://github.com/TU-USUARIO/business-requirements-analyst-skill/issues)
- 💬 **Discusiones**: [GitHub Discussions](https://github.com/TU-USUARIO/business-requirements-analyst-skill/discussions)
- 📧 **Email**: tu-email@ejemplo.com

---

## 🔗 Links Útiles

- [Documentación de Claude Skills](https://docs.anthropic.com/claude/docs/skills)
- [Claude API](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [BABOK Guide](https://www.iiba.org/career-resources/a-business-analysis-professionals-foundation-for-success/babok/)

---

**⭐ Si este Skill te fue útil, dale una estrella en GitHub!**
