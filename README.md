# Guía de Spec-Driven Development con GitHub Spec-Kit

> 📦 **Repositorio oficial**: [github/spec-kit](https://github.com/github/spec-kit)
> 📖 **Blog oficial**: [Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
> 📝 **Guía detallada**: [Diving Into SDD With GitHub Spec Kit](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
> 🎥 **Video overview**: [GitHub Spec Kit en acción](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)

## ¿Qué es Spec-Driven Development (SDD)?

**Spec-Driven Development** es una metodología de desarrollo que prioriza la creación de especificaciones detalladas antes de la implementación del código. En lugar de hacer *vibe coding* y generar código ad-hoc con prompts, SDD establece la especificación como la fuente de verdad compartida que los agentes de IA ejecutan, validan y verifican en cada paso.

### Principios Fundamentales

- **Especificaciones como Lingua Franca**: La especificación se convierte en el artefacto principal. El código es su expresión en un lenguaje y framework particular. Mantener software significa evolucionar especificaciones.
- **Especificaciones Ejecutables**: Las especificaciones deben ser lo suficientemente precisas, completas y no ambiguas para generar sistemas funcionales, eliminando la brecha entre intención e implementación.
- **Refinamiento Continuo**: La validación de consistencia ocurre de forma continua, no como una compuerta única.

## ¿Qué es GitHub Spec-Kit?

[GitHub Spec-Kit](https://github.com/github/spec-kit) es un toolkit open source (licencia MIT) que facilita el desarrollo basado en especificaciones. Tiene dos componentes clave:

1. **Specify CLI**: Un CLI basado en Python que inicializa y estructura proyectos para SDD. Descarga las plantillas oficiales del repositorio de GitHub para el agente de codificación y plataforma de tu elección.
2. **Plantillas y scripts helper**: Establecen la base de la experiencia SDD. Definen cómo luce una spec, qué abarca un plan técnico, y cómo todo se desglosa en tareas individuales que un agente de IA puede ejecutar.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado y configurado lo siguiente:

- **Python 3.11 o superior** + **[uv](https://docs.astral.sh/uv/)**: Se recomienda `uv` para instalar y ejecutar el CLI. Puedes descargar Python desde [python.org](https://www.python.org/downloads/).
- **Git**: Asegúrate de tener Git instalado y configurado en tu sistema. Puedes descargarlo desde [git-scm.com](https://git-scm.com/downloads).
- **PowerShell (recomendado para Windows)**: Para una experiencia óptima en Windows, se recomienda instalar la última versión de PowerShell. [Instalar PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.4).
- **Un entorno de terminal/shell moderno**: Asegúrate de tener un terminal funcional (como Bash, Zsh, o el terminal integrado de tu IDE) para ejecutar los comandos.
- **Un agente de codificación asistido por IA**: Es altamente recomendado para aprovechar al máximo el Spec-Kit y acelerar el desarrollo.

## Instalación del Specify CLI

### Opción 1: Instalación global con `uv`

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

### Opción 2: Ejecución directa con `uvx` (sin instalación)

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <NOMBRE_PROYECTO>
```

### Verificar requisitos del sistema

```bash
specify check
```

### Inicializar un proyecto

```bash
# Crear un nuevo proyecto
uvx --from git+https://github.com/github/spec-kit.git specify init <NOMBRE_PROYECTO>

# Inicializar en el directorio actual
uvx --from git+https://github.com/github/spec-kit.git specify init . --ai claude

# O usando el flag --here
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai claude

# Forzar merge en directorio no vacío
specify init . --force --ai copilot

# Saltar inicialización de git
specify init mi-proyecto --ai gemini --no-git

# Con debug output
specify init mi-proyecto --ai claude --debug

# Con GitHub token (útil para entornos corporativos)
specify init mi-proyecto --ai claude --github-token ghp_tu_token

# Instalar skills del agente junto con el proyecto
specify init mi-proyecto --ai claude --ai-skills
```

Al ejecutar `specify init`, se te pedirá seleccionar uno de los agentes soportados mediante un menú interactivo. Specify descargará la versión correcta de plantillas para el agente que elijas.

### Agentes de IA Soportados

| Agente | Soporte | Formato de Comandos |
|--------|---------|-------------------|
| [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) | ✅ | `.md` |
| [GitHub Copilot](https://github.com/features/copilot) | ✅ | `.agent.md` |
| [Gemini CLI](https://ai.google.dev/gemini-api/docs) | ✅ | `.toml` |
| [Cursor](https://cursor.sh) | ✅ | `.md` |
| [Qwen Code](https://qwenlm.github.io/) | ✅ | `.toml` |
| [opencode](https://opencode.com) | ✅ | `.md` |
| [Windsurf](https://windsurf.ai) | ✅ | `.md` |
| [Kilo Code](https://kilocode.ai) | ✅ | `.md` |
| [Auggie CLI](https://auggie.ai) | ✅ | `.md` |
| [Roo Code](https://roocode.ai) | ✅ | `.md` |
| [Codex CLI](https://openai.com/blog/openai-codex) | ✅ | `.md` |
| Genérico (`--ai generic`) | ✅ | `.md` (con `--ai-commands-dir`) |

> 💡 También se soportan alias cortos (por ejemplo, `--ai kiro` se resuelve a `kiro-cli`).

### Descargar plantillas manualmente

También puedes descargar las plantillas directamente desde la [página de Releases](https://github.com/github/spec-kit/releases) y extraerlas en tu directorio de proyecto.

## Flujo de Trabajo SDD (8 Pasos)

El flujo de trabajo se organiza en fases con checkpoints explícitos. No se avanza hasta que la fase actual esté validada.

```
┌─────────────────────────────────────────────┐
│   Constitución y Especificación             │
│                                             │
│  1. /speckit.constitution                   │
│  2. /speckit.specify                        │
│  3. /speckit.clarify (opcional)             │
├─────────────────────────────────────────────┤
│   Plan de Implementación y Tareas           │
│                                             │
│  4. /speckit.plan                           │
│  5. /speckit.checklist (opcional)           │
│  6. /speckit.tasks                          │
│  7. /speckit.analyze (opcional)             │
├─────────────────────────────────────────────┤
│   Implementación                            │
│                                             │
│  8. /speckit.implement                      │
└─────────────────────────────────────────────┘
```

---

### 1. 🏛️ Constitución (`/speckit.constitution`)

**Objetivo**: Define los principios de gobernanza no negociables del proyecto.

**Resultado**: Crea/actualiza `.specify/memory/constitution.md` con principios fundamentales, reglas de decisión, estándares de calidad y políticas de desarrollo. Utiliza versionado semántico para los cambios y propaga las enmiendas a las plantillas dependientes.

Todos los comandos posteriores (especialmente `/speckit.plan`) leen este archivo como compuerta antes de proceder.

**Ejemplos de uso**:
```bash
# Aplicación de gestión de tareas
/speckit.constitution Crear una aplicación de gestión de tareas con principios de simplicidad, usabilidad y productividad

# E-commerce
/speckit.constitution Desarrollar una plataforma de e-commerce enfocada en seguridad, rendimiento y experiencia de usuario

# API REST
/speckit.constitution Construir una API REST robusta con principios de escalabilidad, mantenibilidad y documentación clara
```

---

### 2. 📋 Especificación (`/speckit.specify`)

**Objetivo**: Convierte descripciones de características en especificaciones detalladas con user stories, requisitos funcionales y criterios de aceptación.

**Resultado**: Genera:
- Rama de Git nueva (ej. `001-create-taskify`)
- Directorio `specs/NNN-feature/`
- Archivo `specs/NNN-feature/spec.md` con user stories, requisitos funcionales y criterios de éxito

**Ejemplos de uso**:
```bash
# Sistema de autenticación
/speckit.specify Sistema de autenticación de usuarios con login, registro, recuperación de contraseña y 2FA

# Panel de administración
/speckit.specify Panel de administración para gestionar usuarios, productos, pedidos y estadísticas

# Visualizador de cámaras
/speckit.specify Visualizador de snapshots de cámaras con landing page que muestre todas las cámaras disponibles en tarjetas uniformes
```

> 💡 **No trates el primer intento como final**. Usa la interacción con tu agente como oportunidad para clarificar y hacer preguntas sobre la especificación.

---

### 3. 🔍 Clarificación (`/speckit.clarify`) — *Opcional*

**Objetivo**: Identifica información faltante, ambigüedades, contradicciones o incompletitud en la especificación antes de planificar. Ejecuta un ciclo de Q&A estructurado donde el agente de IA hace preguntas derivadas de la spec y la constitución.

**Resultado**: Actualiza la sección de Clarificaciones en `spec.md` con las respuestas proporcionadas.

El agente analiza tu spec y presenta preguntas organizadas por categoría, cada una con opciones recomendadas basadas en los principios de la constitución.

**Cuándo usarlo**:
- Cuando la especificación inicial está incompleta o ambigua
- Antes de crear el plan técnico para reducir retrabajo
- Cuando quieras abordar *unknown unknowns*

**Ejemplos de uso**:
```bash
# Ejecutar clarificación estructurada
/speckit.clarify

# Si aún quedan cosas vagas después de clarify, puedes refinar con free-form:
Para cada proyecto de ejemplo debe haber entre 5 y 15 tareas distribuidas aleatoriamente en diferentes estados de completitud
```

> Si intencionalmente quieres saltar la clarificación (ej. spike o prototipo exploratorio), decláralo explícitamente para que el agente no bloquee.

---

### 4. 🛠️ Plan Técnico (`/speckit.plan`)

**Objetivo**: Define la arquitectura, stack tecnológico y restricciones del proyecto. Consulta la constitución para asegurar que los principios no negociables se respeten.

**Resultado**: Genera archivos como:
- `specs/NNN-feature/plan.md` — Plan de implementación con stack tecnológico
- `specs/NNN-feature/data-model.md` — Modelo de datos
- `specs/NNN-feature/contracts/` — Contratos de API
- `specs/NNN-feature/research.md` — Investigación de tecnologías

**Ejemplos de uso**:
```bash
# Especificar stack concreto
/speckit.plan Esto es un sitio Hugo. No debe usar otros frameworks. Templates personalizados con HTML, CSS y JavaScript puro.

# Stack .NET
/speckit.plan Generar usando .NET Aspire con Postgres. Frontend con Blazor server con drag-and-drop, real-time updates. REST API con projects, tasks y notifications API.

# Stack JavaScript
/speckit.plan Stack: Next.js + React + TypeScript + Tailwind. Backend: Next.js API Routes. BD: PostgreSQL + Prisma ORM.
```

> 💡 **Tip**: Antes de implementar, vale la pena pedirle al agente que verifique si hay piezas sobrediseñadas en el plan.

---

### 5. ✅ Checklist de Calidad (`/speckit.checklist`) — *Opcional*

**Objetivo**: Genera checklists de calidad personalizados que validan la completitud, claridad y consistencia de los requisitos — como "unit tests para inglés". Combina requisitos funcionales y técnicos.

**Resultado**: Genera checklists en `specs/NNN-feature/checklists/` para dominios específicos como UX, seguridad, accesibilidad, rendimiento, etc.

**Ejemplos de uso**:
```bash
# Generar checklist de UX
/speckit.checklist

# El agente te hará preguntas clarificatorias para asegurar
# que el checklist refleje la intención correcta
```

---

### 6. 📝 Tareas (`/speckit.tasks`)

**Objetivo**: Desglosa el plan técnico en tareas de desarrollo específicas, accionables y pequeñas que pueden ser implementadas y validadas de forma aislada.

**Resultado**: Genera `specs/NNN-feature/tasks.md` con:
- Lista de tareas específicas
- Priorización
- Estimaciones de tiempo
- Dependencias entre tareas

**Ejemplos de uso**:
```bash
# Generar tareas automáticamente desde el plan
/speckit.tasks
```

---

### 7. 🔬 Análisis de Consistencia (`/speckit.analyze`) — *Opcional*

**Objetivo**: Realiza una verificación de consistencia de solo lectura entre la spec, el plan y las tareas. Detecta duplicaciones, ambigüedades, brechas de cobertura y contradicciones entre artefactos. También verifica la alineación con la constitución.

**Resultado**: Genera un reporte de análisis con hallazgos y recomendaciones.

**Cuándo usarlo**: Después de `/speckit.tasks` y antes de `/speckit.implement`.

```bash
# Ejecutar análisis de consistencia
/speckit.analyze
```

---

### 8. 🚀 Implementación (`/speckit.implement`)

**Objetivo**: Ejecuta todas las tareas definidas, generando código, scripts, documentación, tests y demás artefactos de soporte.

**Resultado**:
- Genera artefactos de código
- Actualiza el progreso
- Implementa las funcionalidades según spec, plan y tareas

**Ejemplos de uso**:
```bash
# Implementar todas las tareas
/speckit.implement

# Puedes especificar cómo quieres proceder
/speckit.implement Ejecutar todas las tareas. Detenerse en cada una para preguntar si hay dudas antes de avanzar.

# Feature específica
/speckit.implement Implementar únicamente la funcionalidad de notificaciones push
```

> 💡 **Tip**: Puedes generar múltiples variantes de implementación desde la misma spec para comparar trade-offs entre distintos enfoques.

## Tabla Resumen de Comandos

| Comando | Tipo | Descripción | Archivos Generados |
|---------|------|-------------|-------------------|
| `/speckit.constitution` | Core | Establece principios de gobernanza | `.specify/memory/constitution.md` |
| `/speckit.specify` | Core | Crea especificaciones detalladas | `specs/NNN-feature/spec.md` |
| `/speckit.clarify` | Opcional | Clarificación estructurada de la spec | Actualiza `spec.md` |
| `/speckit.plan` | Core | Define stack tecnológico y arquitectura | `plan.md`, `data-model.md`, `contracts/` |
| `/speckit.checklist` | Opcional | Genera checklists de calidad | `checklists/` |
| `/speckit.tasks` | Core | Desglosa en tareas específicas | `tasks.md` |
| `/speckit.analyze` | Opcional | Análisis de consistencia entre artefactos | Reporte de análisis |
| `/speckit.implement` | Core | Ejecuta la implementación | Código y artefactos |

## Sistema de Extensiones

Spec Kit cuenta con un sistema de extensiones que permite agregar funcionalidades adicionales al flujo de trabajo SDD.

### Gestión de Extensiones

```bash
# Buscar extensiones disponibles en el catálogo
specify extension search

# Instalar una extensión del catálogo
specify extension add <extension-id>

# Instalar desde URL directa
specify extension add --from https://github.com/org/spec-kit-ext/archive/refs/tags/v1.0.0.zip

# Instalar en modo desarrollo (local)
specify extension add --dev /path/to/my-extension

# Listar extensiones instaladas
specify extension list

# Habilitar/deshabilitar extensión
specify extension enable <extension-id>
specify extension disable <extension-id>

# Actualizar extensiones
specify extension update

# Eliminar extensión
specify extension remove <extension-id>
```

### Catálogo de Extensiones

Existen dos catálogos:

- **Catálogo oficial** (`catalog.json`): Extensiones curadas y aprobadas.
- **Catálogo comunitario** (`catalog.community.json`): Extensiones contribuidas por la comunidad.

Se puede configurar un catálogo organizacional personalizado:

```bash
export SPECKIT_CATALOG_URL="https://tu-org.com/spec-kit/catalog.json"
specify extension search  # Ahora usa tu catálogo organizacional
```

### Crear tu propia extensión

Spec Kit incluye un [template de extensión](https://github.com/github/spec-kit/tree/main/extensions/template) con la estructura necesaria:

```
mi-extension/
├── extension.yml          # Manifiesto
├── commands/              # Slash commands
│   └── mi-comando.md
├── config-template.yml    # Configuración por defecto
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## Estructura de Archivos del Proyecto

```
tu-proyecto/
├── .specify/
│   ├── memory/
│   │   └── constitution.md          # Principios de gobernanza
│   ├── scripts/
│   │   ├── bash/
│   │   │   ├── create-new-feature.sh
│   │   │   ├── check-prerequisites.sh
│   │   │   ├── setup-plan.sh
│   │   │   ├── update-agent-context.sh
│   │   │   └── common.sh
│   │   └── powershell/
│   │       ├── create-new-feature.ps1
│   │       ├── check-prerequisites.ps1
│   │       ├── setup-plan.ps1
│   │       └── common.ps1
│   ├── templates/
│   │   ├── spec-template.md
│   │   ├── plan-template.md
│   │   ├── tasks-template.md
│   │   └── agent-file-template.md
│   └── extensions/                   # Extensiones instaladas
│       ├── .registry
│       └── {extension-id}/
├── .<agent-folder>/                  # Ej: .claude/, .github/agents/, .cursor/
│   └── commands/
│       ├── speckit.constitution.md
│       ├── speckit.specify.md
│       ├── speckit.clarify.md
│       ├── speckit.plan.md
│       ├── speckit.tasks.md
│       ├── speckit.implement.md
│       ├── speckit.analyze.md
│       └── speckit.checklist.md
├── specs/
│   └── 001-mi-feature/
│       ├── spec.md                   # Especificación
│       ├── plan.md                   # Plan técnico
│       ├── tasks.md                  # Tareas
│       ├── data-model.md             # Modelo de datos
│       ├── research.md               # Investigación
│       ├── quickstart.md             # Guía rápida
│       ├── contracts/                # Contratos de API
│       └── checklists/               # Checklists de calidad
└── README.md
```

## Variables de Entorno

| Variable | Descripción |
|----------|-------------|
| `SPECIFY_FEATURE` | Override para detección de feature en repos sin Git. Setear al nombre del directorio de feature (ej. `001-photo-albums`) |
| `SPECKIT_CATALOG_URL` | URL personalizada para el catálogo de extensiones |

## Escenarios de Uso de SDD

### Greenfield (zero-to-one)
Cuando arrancás un proyecto nuevo, un poco de trabajo previo para crear una spec y un plan asegura que la IA construya lo que realmente querés, no solo una solución genérica basada en patrones comunes.

### Brownfield (extensión de código existente)
Spec Kit se adapta a codebases existentes sin necesidad de specs previas o una constitución. Podés agregar nuevas features con el flujo completo de SDD.

### Multi-variante
Dado que las specs están desacopladas del código, es posible crear múltiples implementaciones con facilidad. ¿Querés comparar rendimiento entre Rust y Go? Pedile al agente que produzca dos implementaciones distintas basadas en la misma spec.

## Ejemplos Prácticos Completos

### Ejemplo 1: Sistema de Blog

```bash
# Paso 1: Constitución
/speckit.constitution Crear un sistema de blog personal con énfasis en simplicidad, rendimiento y facilidad de uso

# Paso 2: Especificación
/speckit.specify Sistema de gestión de posts con editor markdown, categorías, tags y comentarios

# Paso 3: Clarificación (opcional)
/speckit.clarify

# Paso 4: Plan técnico
/speckit.plan Stack: Next.js + React + TypeScript + Tailwind. BD: PostgreSQL + Prisma ORM. Auth: NextAuth.js. Deploy: Vercel.

# Paso 5: Checklist (opcional)
/speckit.checklist

# Paso 6: Tareas
/speckit.tasks

# Paso 7: Análisis (opcional)
/speckit.analyze

# Paso 8: Implementación
/speckit.implement
```

### Ejemplo 2: E-commerce Básico

```bash
# Paso 1: Constitución
/speckit.constitution Desarrollar una tienda online con enfoque en seguridad, usabilidad y conversión de ventas

# Paso 2: Especificación
/speckit.specify Plataforma de e-commerce con catálogo de productos, carrito de compras, checkout y gestión de pedidos

# Paso 3: Clarificación
/speckit.clarify

# Paso 4: Plan técnico
/speckit.plan Frontend: React + Next.js + TypeScript. Backend: Node.js + Express. BD: PostgreSQL + Prisma. Pagos: Stripe + PayPal. Deploy: Vercel + Railway.

# Paso 5-8: Checklist, Tareas, Análisis, Implementación
/speckit.checklist
/speckit.tasks
/speckit.analyze
/speckit.implement
```

### Ejemplo 3: API de Microservicios

```bash
# Paso 1: Constitución
/speckit.constitution Construir una arquitectura de microservicios robusta con principios de escalabilidad y mantenibilidad

# Paso 2: Especificación
/speckit.specify API de microservicios con autenticación, gestión de usuarios, logging y monitoreo

# Paso 3: Clarificación
/speckit.clarify

# Paso 4: Plan técnico
/speckit.plan Backend: Node.js + Express + TypeScript. BD: PostgreSQL + Redis. Contenedores: Docker + Docker Compose. Orquestación: Kubernetes. Monitoreo: Prometheus + Grafana.

# Paso 5-8: Checklist, Tareas, Análisis, Implementación
/speckit.checklist
/speckit.tasks
/speckit.analyze
/speckit.implement
```

## Walkthroughs de la Comunidad

- **[Greenfield .NET CLI tool](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Construye una utilidad de Timezone como herramienta .NET single-binary CLI desde un directorio vacío, cubriendo el flujo completo: constitution, specify, plan, tasks e implement multi-pass con GitHub Copilot.

- **[Greenfield Spring Boot + React platform](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Construye una plataforma de analytics de rendimiento de LLM (REST API, gráficos, tracking de iteraciones) desde cero con Spring Boot, React embebido, PostgreSQL y Docker Compose, incluyendo paso de clarify y análisis de consistencia cross-artifact.

- **[Brownfield ASP.NET CMS extension](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Extiende un CMS .NET open-source existente (CarrotCakeCMS-Core) con dos nuevas features, demostrando cómo spec-kit se integra en codebases existentes sin specs previas.

## Integración con GitHub

### Configuración Inicial

```bash
# Inicializar repositorio Git
git init

# Configurar usuario (si no está configurado)
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"

# Añadir archivos
git add .

# Primer commit
git commit -m "Inicialización del proyecto con Spec-Driven Development"

# Conectar con repositorio remoto
git remote add origin https://github.com/tu-usuario/tu-repositorio.git

# Subir a GitHub
git push -u origin main
```

### Workflow de Desarrollo

```bash
# spec-kit crea ramas automáticamente con /speckit.specify
# pero también puedes crear ramas manualmente:
git checkout -b feature/nueva-funcionalidad

# Trabajar con spec-kit
/speckit.specify "Nueva funcionalidad"
/speckit.clarify
/speckit.plan "Detalles técnicos..."
/speckit.checklist
/speckit.tasks
/speckit.analyze
/speckit.implement

# Commit y push
git add .
git commit -m "Implementar nueva funcionalidad"
git push origin feature/nueva-funcionalidad

# Crear Pull Request en GitHub
# (también puedes pedirle al agente que lo haga por vos si tenés GitHub CLI instalado)
```

## Ventajas del Spec-Driven Development

### 🎯 Claridad de Objetivos
Requisitos claramente definidos desde el inicio, reducción de ambigüedades y mejor comunicación entre equipos.

### 🔄 Iteración Controlada
Desarrollo incremental basado en especificaciones, validación continua de requisitos y facilita la retroalimentación.

### 📈 Calidad Mejorada
Código más robusto y mantenible, menos bugs en producción y mejor cobertura de casos de uso.

### ⏱️ Eficiencia
Menos tiempo en debugging, desarrollo más predecible y mejor estimación de tiempos.

### 🔀 Exploración Multi-variante
Se pueden generar múltiples variantes de implementación desde la misma spec para comparar trade-offs.

## Mejores Prácticas

1. **Especificaciones Detalladas**: Incluí casos de uso específicos, definí criterios de aceptación claros y considerá casos edge y errores.

2. **Clarificación Temprana**: Usá `/speckit.clarify` antes de planificar para reducir retrabajo downstream. Los *unknown unknowns* son el mayor enemigo de una buena spec.

3. **Planificación Realista**: Desglosá tareas en componentes pequeños, estimá tiempos de manera conservadora e identificá dependencias críticas.

4. **Análisis de Consistencia**: Ejecutá `/speckit.analyze` antes de implementar para detectar contradicciones entre artefactos.

5. **Implementación Iterativa**: Implementá y probá en pequeños incrementos, validá contra especificaciones continuamente y mantené documentación actualizada.

6. **Human-in-the-Loop**: Recordá que vos dirigís la dirección y las decisiones. El agente es una herramienta poderosa, pero necesita tu supervisión.

## Recursos Adicionales

- [Repositorio oficial de GitHub Spec-Kit](https://github.com/github/spec-kit)
- [Releases y plantillas](https://github.com/github/spec-kit/releases)
- [Blog de anuncio — GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [Guía detallada — Microsoft Developer Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
- [Explicación en profundidad — Den Delimarsky](https://den.dev/blog/github-spec-kit/)
- [Curso en LinkedIn Learning](https://github.com/LinkedInLearning/spec-driven-development-with-github-spec-kit-4641001)
- [Sistema de extensiones](https://github.com/github/spec-kit/tree/main/extensions)
- [Template para crear extensiones](https://github.com/github/spec-kit/tree/main/extensions/template)

### Agentes de IA Recomendados
- [GitHub Copilot](https://github.com/features/copilot)
- [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
- [Cursor](https://cursor.sh)
- [Windsurf](https://windsurf.ai)
- [Gemini CLI](https://ai.google.dev/gemini-api/docs)

### Proyectos Relacionados
- [IaC Spec Kit (IBM)](https://github.com/IBM/iac-spec-kit) — Adaptación de Spec Kit para Infrastructure as Code
- [Autospec](https://github.com/search?q=autospec+spec-driven) — CLI inspirado en Spec Kit para uso con Claude Code

## Contribuciones

¡Las contribuciones son bienvenidas! Si tienes sugerencias, mejoras o encuentras bugs:

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

> ⚠️ **Nota del equipo de Spec Kit**: No envíes PRs que reescriban toda la arquitectura. Si tenés cambios grandes, discutílos primero en un issue.

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

---

**¿Necesitas ayuda?** Abre un [issue en el repositorio oficial](https://github.com/github/spec-kit/issues) o contacta al equipo de desarrollo.

**¿Te gustó esta guía?** ¡Dale una estrella al repositorio! ⭐
