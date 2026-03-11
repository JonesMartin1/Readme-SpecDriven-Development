# Guía de Spec-Driven Development con GitHub Spec-Kit

> 📦 **Repositorio oficial**: [github/spec-kit](https://github.com/github/spec-kit)
> 📖 **Blog oficial**: [Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
> 📝 **Guía detallada**: [Diving Into SDD With GitHub Spec Kit](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
> 🎥 **Video overview**: [GitHub Spec Kit en acción](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)

## ¿Qué es Spec-Driven Development (SDD)?

**Spec-Driven Development** es una metodología de desarrollo que pone las especificaciones por delante del código. En vez de ponerse a hacer *vibe coding* y generar código a las apuradas con prompts sueltos, SDD plantea que la especificación sea la fuente de verdad compartida: los agentes de IA la ejecutan, la validan y la verifican en cada paso.

### Principios fundamentales

- **Las especificaciones como lenguaje natural**: La spec pasa a ser el artefacto principal del proyecto. El código es simplemente cómo se expresa esa specificación en un lenguaje y framework particular. Mantener software es igual a evolucionar especificaciones.
- **Especificaciones ejecutables**: Tienen que ser lo suficientemente precisas, completas y sin ambigüedades como para generar sistemas funcionales. La idea es eliminar la brecha entre lo que querés hacer y lo que termina implementado.
- **Refinamiento continuo**: La validación de consistencia pasa de forma continua a lo largo de todo el proceso, no como un checkpoint aislado.

## ¿Qué es GitHub Spec-Kit?

[GitHub Spec-Kit](https://github.com/github/spec-kit) es un toolkit open source (licencia MIT) que te facilita trabajar con desarrollo basado en especificaciones. Tiene dos componentes principales:

1. **Specify CLI**: Un CLI hecho en Python que te inicializa y estructura proyectos para SDD. Descarga las plantillas oficiales del repo de GitHub para el agente de codificación y plataforma que elijas.
2. **Plantillas y scripts helper**: Son la base de toda la experiencia SDD. Definen cómo se ve una spec, qué incluye un plan técnico y cómo se desglosa todo en tareas individuales que un agente de IA puede ejecutar.

## Requisitos previos

Antes de arrancar, asegurate de tener instalado y configurado lo siguiente:

- **Python 3.11 o superior** + **[uv](https://docs.astral.sh/uv/)**: Se recomienda `uv` para instalar y ejecutar el CLI. Podés descargar Python desde [python.org](https://www.python.org/downloads/).
- **Git**: Tené Git instalado y configurado. Descargalo desde [git-scm.com](https://git-scm.com/downloads).
- **PowerShell (recomendado para Windows)**: Para una mejor experiencia en Windows, instalá la última versión de PowerShell. [Instalar PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.4).
- **Un entorno de terminal/shell moderno**: Cualquier terminal funcional sirve (Bash, Zsh o el terminal integrado de tu IDE).
- **Un agente de codificación asistido por IA**: Es muy recomendable para sacarle el jugo al Spec-Kit y acelerar el desarrollo.

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

Cuando ejecutás `specify init`, te aparece un menú interactivo para elegir el agente que usás. Specify se encarga de descargar la versión correcta de plantillas para ese agente.

### Agentes de IA soportados

| Agente | Soporte | Formato de comandos |
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

Si preferís, podés descargar las plantillas directo desde la [página de Releases](https://github.com/github/spec-kit/releases) y extraerlas en la carpeta de tu proyecto.

## Flujo de trabajo SDD (8 pasos)

El flujo se organiza en fases con checkpoints explícitos. No se avanza hasta que la fase actual esté validada.

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

**Objetivo**: Definir los principios de gobernanza no negociables del proyecto.

**Resultado**: Crea o actualiza `.specify/memory/constitution.md` con los principios fundamentales, reglas de decisión, estándares de calidad y políticas de desarrollo. Usa versionado semántico para los cambios y propaga las enmiendas a las plantillas que dependen de él.

Todos los comandos que vienen después (especialmente `/speckit.plan`) leen este archivo como guía antes de seguir adelante.

**Ejemplos de uso**:
```bash
# App de gestión de tareas
/speckit.constitution Crear una aplicación de gestión de tareas con principios de simplicidad, usabilidad y productividad

# E-commerce
/speckit.constitution Desarrollar una plataforma de e-commerce enfocada en seguridad, rendimiento y experiencia de usuario

# API REST
/speckit.constitution Construir una API REST robusta con principios de escalabilidad, mantenibilidad y documentación clara
```

---

### 2. 📋 Especificación (`/speckit.specify`)

**Objetivo**: Convertir descripciones de funcionalidades en especificaciones detalladas con user stories, requisitos funcionales y criterios de aceptación.

**Resultado**: Genera:
- Una rama de Git nueva (ej. `001-create-taskify`)
- El directorio `specs/NNN-feature/`
- El archivo `specs/NNN-feature/spec.md` con user stories, requisitos funcionales y criterios de éxito

**Ejemplos de uso**:
```bash
# Sistema de autenticación
/speckit.specify Sistema de autenticación de usuarios con login, registro, recuperación de contraseña y 2FA

# Panel de administración
/speckit.specify Panel de administración para gestionar usuarios, productos, pedidos y estadísticas

# Visualizador de cámaras
/speckit.specify Visualizador de snapshots de cámaras con landing page que muestre todas las cámaras disponibles en tarjetas uniformes
```

> 💡 **No tomes el primer intento como la versión final.** Aprovechá la interacción con el agente para ir clarificando y haciendo preguntas sobre la spec.

---

### 3. 🔍 Clarificación (`/speckit.clarify`) — *Opcional*

**Objetivo**: Encontrar información que falta, ambigüedades, contradicciones o cosas incompletas en la spec antes de pasar a planificar. Ejecuta un ciclo de preguntas y respuestas estructurado donde el agente te consulta cosas derivadas de la spec y la constitución.

**Resultado**: Actualiza la sección de Clarificaciones en `spec.md` con las respuestas que le diste.

El agente analiza tu spec y te presenta preguntas organizadas por categoría, cada una con opciones recomendadas basadas en los principios de la constitución.

**Cuándo usarlo**:
- Cuando la spec inicial quedó incompleta o ambigua
- Antes de armar el plan técnico, para evitar retrabajo
- Cuando querés atacar los *unknown unknowns* (lo que no sabés que no sabés)

**Ejemplos de uso**:
```bash
# Ejecutar clarificación estructurada
/speckit.clarify

# Si después del clarify todavía quedan cosas vagas, podés refinar con free-form:
Para cada proyecto de ejemplo debe haber entre 5 y 15 tareas distribuidas aleatoriamente en diferentes estados de completitud
```

> Si querés saltear la clarificación a propósito (ej. para un spike o prototipo exploratorio), decilo explícitamente para que el agente no se quede esperando.

---

### 4. 🛠️ Plan técnico (`/speckit.plan`)

**Objetivo**: Definir la arquitectura, el stack tecnológico y las restricciones del proyecto. Consulta la constitución para asegurar que los principios no negociables se respeten.

**Resultado**: Genera archivos como:
- `specs/NNN-feature/plan.md` — Plan de implementación con stack tecnológico
- `specs/NNN-feature/data-model.md` — Modelo de datos
- `specs/NNN-feature/contracts/` — Contratos de API
- `specs/NNN-feature/research.md` — Investigación de tecnologías

**Ejemplos de uso**:
```bash
# Especificar un stack concreto
/speckit.plan Esto es un sitio Hugo. No debe usar otros frameworks. Templates personalizados con HTML, CSS y JavaScript puro.

# Stack .NET
/speckit.plan Generar usando .NET Aspire con Postgres. Frontend con Blazor server con drag-and-drop, real-time updates. REST API con projects, tasks y notifications API.

# Stack JavaScript
/speckit.plan Stack: Next.js + React + TypeScript + Tailwind. Backend: Next.js API Routes. BD: PostgreSQL + Prisma ORM.
```

> 💡 **Tip**: Antes de implementar, está bueno pedirle al agente que revise si hay piezas sobrediseñadas en el plan.

---

### 5. ✅ Checklist de calidad (`/speckit.checklist`) — *Opcional*

**Objetivo**: Generar checklists de calidad personalizados que validen que los requisitos estén completos, claros y sean consistentes. Pensalo como "unit tests para texto". Combina requisitos funcionales y técnicos.

**Resultado**: Genera checklists en `specs/NNN-feature/checklists/` para dominios específicos como UX, seguridad, accesibilidad, rendimiento, etc.

**Ejemplos de uso**:
```bash
# Generar checklist de UX
/speckit.checklist

# El agente te va a hacer preguntas para asegurarse
# de que el checklist refleje bien lo que querés
```

---

### 6. 📝 Tareas (`/speckit.tasks`)

**Objetivo**: Desglosar el plan técnico en tareas de desarrollo específicas, accionables y chicas, que se puedan implementar y validar de forma aislada.

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

### 7. 🔬 Análisis de consistencia (`/speckit.analyze`) — *Opcional*

**Objetivo**: Hacer una verificación de consistencia (de solo lectura) entre la spec, el plan y las tareas. Detecta duplicaciones, ambigüedades, huecos de cobertura y contradicciones entre los artefactos. También chequea que todo siga alineado con la constitución.

**Resultado**: Genera un reporte de análisis con hallazgos y recomendaciones.

**Cuándo usarlo**: Después de `/speckit.tasks` y antes de `/speckit.implement`.

```bash
# Ejecutar análisis de consistencia
/speckit.analyze
```

---

### 8. 🚀 Implementación (`/speckit.implement`)

**Objetivo**: Ejecutar todas las tareas definidas, generando código, scripts, documentación, tests y todo lo que haga falta.

**Resultado**:
- Genera los artefactos de código
- Actualiza el progreso de las tareas
- Implementa las funcionalidades según la spec, el plan y las tareas

**Ejemplos de uso**:
```bash
# Implementar todas las tareas
/speckit.implement

# Podés especificar cómo querés que avance
/speckit.implement Ejecutar todas las tareas. Detenerse en cada una para preguntar si hay dudas antes de avanzar.

# Una feature específica
/speckit.implement Implementar únicamente la funcionalidad de notificaciones push
```

> 💡 **Tip**: Podés generar múltiples variantes de implementación desde la misma spec para comparar trade-offs entre distintos enfoques.

## Tabla resumen de comandos

| Comando | Tipo | Descripción | Archivos generados |
|---------|------|-------------|-------------------|
| `/speckit.constitution` | Core | Establece principios de gobernanza | `.specify/memory/constitution.md` |
| `/speckit.specify` | Core | Crea especificaciones detalladas | `specs/NNN-feature/spec.md` |
| `/speckit.clarify` | Opcional | Clarificación estructurada de la spec | Actualiza `spec.md` |
| `/speckit.plan` | Core | Define stack tecnológico y arquitectura | `plan.md`, `data-model.md`, `contracts/` |
| `/speckit.checklist` | Opcional | Genera checklists de calidad | `checklists/` |
| `/speckit.tasks` | Core | Desglosa en tareas específicas | `tasks.md` |
| `/speckit.analyze` | Opcional | Análisis de consistencia entre artefactos | Reporte de análisis |
| `/speckit.implement` | Core | Ejecuta la implementación | Código y artefactos |

## Sistema de extensiones

Spec Kit tiene un sistema de extensiones que te permite agregar funcionalidades extra al flujo de trabajo SDD.

### Gestión de extensiones

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

### Catálogo de extensiones

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

## Estructura de archivos del proyecto

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

## Variables de entorno

| Variable | Descripción |
|----------|-------------|
| `SPECIFY_FEATURE` | Override para detección de feature en repos sin Git. Se setea al nombre del directorio de feature (ej. `001-photo-albums`) |
| `SPECKIT_CATALOG_URL` | URL personalizada para el catálogo de extensiones |

## Escenarios de uso de SDD

### Greenfield (de cero)
Cuando arrancás un proyecto nuevo, invertir un rato en crear una spec y un plan asegura que la IA construya lo que realmente querés, y no una solución genérica basada en patrones comunes.

### Brownfield (proyecto existente)
Spec Kit se adapta a codebases que ya existen sin necesidad de tener specs previas ni una constitución armada. Podés agregar features nuevas usando el flujo completo de SDD.

### Multi-variante
Como las specs están desacopladas del código, es posible crear múltiples implementaciones sin drama. ¿Querés comparar rendimiento entre Rust y Go? Pedile al agente que genere dos implementaciones distintas desde la misma spec.

## Ejemplos prácticos completos

### Ejemplo 1: Sistema de blog

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

### Ejemplo 2: E-commerce básico

```bash
# Paso 1: Constitución
/speckit.constitution Desarrollar una tienda online con enfoque en seguridad, usabilidad y conversión de ventas

# Paso 2: Especificación
/speckit.specify Plataforma de e-commerce con catálogo de productos, carrito de compras, checkout y gestión de pedidos

# Paso 3: Clarificación
/speckit.clarify

# Paso 4: Plan técnico
/speckit.plan Frontend: React + Next.js + TypeScript. Backend: Node.js + Express. BD: PostgreSQL + Prisma. Pagos: Stripe + PayPal. Deploy: Vercel + Railway.

# Pasos 5-8: Checklist, Tareas, Análisis, Implementación
/speckit.checklist
/speckit.tasks
/speckit.analyze
/speckit.implement
```

### Ejemplo 3: API de microservicios

```bash
# Paso 1: Constitución
/speckit.constitution Construir una arquitectura de microservicios robusta con principios de escalabilidad y mantenibilidad

# Paso 2: Especificación
/speckit.specify API de microservicios con autenticación, gestión de usuarios, logging y monitoreo

# Paso 3: Clarificación
/speckit.clarify

# Paso 4: Plan técnico
/speckit.plan Backend: Node.js + Express + TypeScript. BD: PostgreSQL + Redis. Contenedores: Docker + Docker Compose. Orquestación: Kubernetes. Monitoreo: Prometheus + Grafana.

# Pasos 5-8: Checklist, Tareas, Análisis, Implementación
/speckit.checklist
/speckit.tasks
/speckit.analyze
/speckit.implement
```

### Ejemplo 4: Brownfield — Agregar una feature a un proyecto existente

Este es el caso más común en la realidad: ya tenés un proyecto andando, con su arquitectura, sus convenciones y su código, y necesitás meterle una funcionalidad nueva. El ejemplo que vamos a seguir es agregar un sistema de feedback con formulario popup, integración con tracking de uso (Amplitude) y notificaciones por email a una app web existente (pensá en un CMS o un dashboard empresarial).

#### Paso 0: Instalar Spec Kit en el proyecto existente

```bash
# Ir al directorio del proyecto que ya tenés
cd mi-proyecto-existente

# Inicializar Spec Kit en el directorio actual
specify init --here --ai claude --force

# O con Copilot
specify init . --force --ai copilot
```

> El flag `--force` permite la inicialización en un directorio que ya tiene archivos, mergeando lo de Spec Kit sin pisar nada de lo que ya existe.

#### Paso 1: Generar un documento de investigación del codebase

Antes de tocar la constitución, lo ideal es que el agente investigue tu codebase y genere un documento de research que funcione como una "vista comprimida" del proyecto. Este paso es clave en brownfield porque le da al agente contexto real sobre la arquitectura, los patrones y las convenciones que ya están implementadas.

```bash
# Pedirle al agente que investigue el codebase existente
Analizá todo el codebase actual. Documentá la arquitectura, los módulos principales,
los flujos de datos, puntos de integración y patrones o convenciones existentes.
Guardá todo en docs/codebase-research.md
```

#### Paso 2: Constitución basada en el proyecto real

En brownfield, el agente analiza los archivos del proyecto para generar una constitución que refleje cómo es el codebase de verdad, no principios genéricos sacados de la nada.

```bash
/speckit.constitution

# El agente va a analizar el proyecto y generar principios basados en:
# - El stack tecnológico actual (ej: .NET 8, Blazor, SQL Server)
# - Las convenciones de naming y estilo de código que ya se usan
# - Los patrones de arquitectura implementados
# - Los estándares de testing del equipo
```

> 💡 **Clave para brownfield**: El stack tecnológico ya está definido — la constitución tiene que documentar eso como un hecho, no redefinirlo. Revisá la constitución generada de punta a punta para asegurarte de que tenga sentido con tu proyecto.

#### Paso 3: Especificación de la nueva feature

```bash
/speckit.specify Sistema de feedback con botón que dispara un formulario popup.
El usuario selecciona una reacción, agrega texto y opcionalmente adjunta un archivo.
Al enviar, se manda un email al equipo de soporte.
Debe integrarse con el tracking de uso existente (Amplitude) y seguir los patrones
establecidos del proyecto.
```

#### Paso 4: Clarificación — fundamental en brownfield

En proyectos que ya existen, la clarificación es especialmente importante porque hay un montón de decisiones implícitas que el agente necesita entender.

```bash
/speckit.clarify

# El agente te va a preguntar cosas como:
# - ¿El popup usa el sistema de modales existente o uno nuevo?
# - ¿Las notificaciones por email usan el servicio de email actual?
# - ¿Qué eventos específicos de Amplitude hay que trackear?
# - ¿El upload de archivos tiene límite de tamaño?
```

#### Paso 5: Plan técnico — referenciando el codebase existente

En brownfield, el plan funciona distinto: en vez de elegir tecnologías, se trata de traducir documentación de alto nivel (diagramas de arquitectura, contratos de API) en estructuras de archivos concretas y modificaciones de código sobre lo que ya existe.

```bash
/speckit.plan Usar los componentes y servicios existentes del proyecto.
El formulario debe seguir el patrón de modales que ya usamos.
El servicio de email debe extender la clase EmailService existente.
Referir al documento docs/codebase-research.md para contexto del codebase.

# Para proyectos grandes (100k+ líneas), conviene pasarle un diseño
# de arquitectura detallado como input, no esperar que el LLM lo resuelva solo.
```

> ⚠️ **Para codebases grandes**: En sistemas maduros y complejos, diseñar la arquitectura de una feature de tamaño medio suele llevar 1-2 días de trabajo de un ingeniero senior. No esperes que el LLM haga trade-offs de diseño a nivel de sistema — eso sigue siendo responsabilidad humana.

#### Pasos 6-8: Checklist, Tareas, Análisis e Implementación

```bash
# Generar checklist de calidad
/speckit.checklist

# Generar tareas — el agente las organiza respetando dependencias
# y marca con [P] las que pueden correr en paralelo
/speckit.tasks

# Verificar consistencia entre spec, plan y tareas
/speckit.analyze

# Implementar — en brownfield conviene ir tarea por tarea
/speckit.implement Ejecutar las tareas una por una.
Antes de crear clases o métodos nuevos, buscar primero si existen
componentes reutilizables en el codebase.
Detenerse después de cada tarea para revisión.
```

> 💡 **Lección de la comunidad**: En brownfield, mantener las features chicas y bien acotadas da mejores resultados. Si una implementación toca más de 30 archivos, probablemente conviene dividirla en features más pequeñas. Así es más fácil revisar el código en cada paso, que es donde realmente se cuida la calidad.

#### Principios clave para brownfield

1. **Tech stack bloqueado**: El stack del proyecto existente es un hecho, la constitución tiene que documentarlo, no cambiarlo.
2. **Compatibilidad ante todo**: Todo código generado debe ser consistente con el estilo que ya existe.
3. **No duplicar código**: Antes de crear algo nuevo, siempre buscar componentes reutilizables en el codebase.
4. **Introducción gradual**: El flujo SDD tiene que integrarse sin fricción en el proceso de desarrollo que ya viene andando.
5. **Research doc primero**: Generar un documento de investigación del codebase antes de arrancar le da al agente un contexto mucho mejor.

#### Extensión Brownfield Bootstrap (opcional)

Si querés algo todavía más automatizado, existe una [extensión comunitaria de brownfield bootstrap](https://github.com/wcpaxx/spec-kit-brownfield-extensions) que analiza tu proyecto automáticamente y genera plantillas personalizadas:

```bash
# Instalar la extensión
specify extension add --from https://github.com/wcpaxx/spec-kit-brownfield-extensions/archive/refs/heads/main.zip

# O manualmente para Claude Code
cp speckit.brownfield-bootstrap.md .claude/commands/

# Ejecutar el bootstrap
/speckit.brownfield-bootstrap.init
```

#### Recursos sobre brownfield

- [Discusión oficial: Recommended approach for brownfield development](https://github.com/github/spec-kit/discussions/331)
- [Discusión: Spec-kit for complex brownfield projects](https://github.com/github/spec-kit/discussions/746)
- [Artículo EPAM: Using Spec Kit for brownfield codebase](https://www.epam.com/insights/ai/blogs/using-spec-kit-for-brownfield-codebase)
- [Brownfield Spec Flow That Actually Ships (Medium)](https://medium.com/@volodymyrostapiuk/brownfield-spec-flow-that-actually-ships-587696ef23fd)
- [Microsoft Learn: Implement SDD with Spec Kit (ejercicio brownfield)](https://learn.microsoft.com/en-us/training/modules/spec-driven-development-github-spec-kit-enterprise-developers/)
- [Brownfield ASP.NET CMS walkthrough](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)

---

## Walkthroughs de la comunidad

- **[Greenfield .NET CLI tool](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Construye una utilidad de Timezone como herramienta .NET single-binary CLI desde un directorio vacío, cubriendo el flujo completo: constitution, specify, plan, tasks e implement multi-pass con GitHub Copilot.

- **[Greenfield Spring Boot + React platform](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Construye una plataforma de analytics de rendimiento de LLM (REST API, gráficos, tracking de iteraciones) desde cero con Spring Boot, React embebido, PostgreSQL y Docker Compose, incluyendo paso de clarify y análisis de consistencia cross-artifact.

- **[Brownfield ASP.NET CMS extension](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)** — Extiende un CMS .NET open-source existente (CarrotCakeCMS-Core) con dos features nuevas, mostrando cómo spec-kit encaja en codebases que ya existen sin necesidad de specs previas.

## Integración con GitHub

### Configuración inicial

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

### Workflow de desarrollo

```bash
# spec-kit crea ramas automáticamente con /speckit.specify
# pero también podés crear ramas manualmente:
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
# (también podés pedirle al agente que lo haga si tenés GitHub CLI instalado)
```

## Ventajas del Spec-Driven Development

### 🎯 Claridad de objetivos
Los requisitos quedan claramente definidos desde el arranque, se reducen las ambigüedades y mejora la comunicación entre los integrantes del equipo.

### 🔄 Iteración controlada
El desarrollo avanza de forma incremental sobre las especificaciones, con validación continua de requisitos y retroalimentación constante.

### 📈 Calidad mejorada
Se consigue código más robusto y mantenible, menos bugs en producción y mejor cobertura de casos de uso.

### ⏱️ Eficiencia
Menos tiempo debuggeando, desarrollo más predecible y mejores estimaciones de tiempos.

### 🔀 Exploración multi-variante
Se pueden generar múltiples variantes de implementación desde la misma spec para comparar trade-offs entre distintos enfoques.

## Mejores prácticas

1. **Especificaciones detalladas**: Incluí casos de uso específicos, definí criterios de aceptación claros y tené en cuenta los casos borde y los errores.

2. **Clarificación temprana**: Usá `/speckit.clarify` antes de planificar para reducir retrabajo. Los *unknown unknowns* son el peor enemigo de una buena spec.

3. **Planificación realista**: Desglosá las tareas en componentes chicos, estimá tiempos de manera conservadora e identificá las dependencias críticas.

4. **Análisis de consistencia**: Ejecutá `/speckit.analyze` antes de implementar para detectar contradicciones entre artefactos.

5. **Implementación iterativa**: Implementá y probá en incrementos chicos, validá contra las especificaciones continuamente y mantené la documentación actualizada.

6. **Human-in-the-Loop**: Recordá que vos dirigís la dirección y las decisiones. El agente es una herramienta muy potente, pero necesita tu supervisión.

## Recursos adicionales

- [Repositorio oficial de GitHub Spec-Kit](https://github.com/github/spec-kit)
- [Releases y plantillas](https://github.com/github/spec-kit/releases)
- [Blog de anuncio — GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [Guía detallada — Microsoft Developer Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
- [Explicación en profundidad — Den Delimarsky](https://den.dev/blog/github-spec-kit/)
- [Curso en LinkedIn Learning](https://github.com/LinkedInLearning/spec-driven-development-with-github-spec-kit-4641001)
- [Sistema de extensiones](https://github.com/github/spec-kit/tree/main/extensions)
- [Template para crear extensiones](https://github.com/github/spec-kit/tree/main/extensions/template)

### Agentes de IA recomendados
- [GitHub Copilot](https://github.com/features/copilot)
- [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
- [Cursor](https://cursor.sh)
- [Windsurf](https://windsurf.ai)
- [Gemini CLI](https://ai.google.dev/gemini-api/docs)

### Proyectos relacionados
- [IaC Spec Kit (IBM)](https://github.com/IBM/iac-spec-kit) — Adaptación de Spec Kit para Infrastructure as Code
- [Autospec](https://github.com/search?q=autospec+spec-driven) — CLI inspirado en Spec Kit para uso con Claude Code

## Contribuciones

¡Las contribuciones son bienvenidas! Si tenés sugerencias, mejoras o encontrás bugs:

1. Hacé fork del repositorio
2. Creá una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commiteá tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Pusheá a la rama (`git push origin feature/AmazingFeature`)
5. Abrí un Pull Request

> ⚠️ **Nota del equipo de Spec Kit**: No manden PRs que reescriban toda la arquitectura. Si tienen cambios grandes, discútanlos primero en un issue.

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

---

**¿Necesitás ayuda?** Abrí un [issue en el repositorio oficial](https://github.com/github/spec-kit/issues) o contactá al equipo de desarrollo.

**¿Te sirvió esta guía?** ¡Dale una estrella al repositorio! ⭐
