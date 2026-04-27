# Guía para empezar con GitHub Spec-Kit

> 📦 **Repositorio oficial**: [github/spec-kit](https://github.com/github/spec-kit)
> 📖 **Blog oficial**: [Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
> 📝 **Guía detallada**: [Diving Into SDD With GitHub Spec Kit](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
> 🎥 **Video overview**: [GitHub Spec Kit en acción](https://github.com/github/spec-kit#want-to-see-spec-kit-in-action)

---

## ¿Qué es esto y para qué sirve?

**Spec-Kit** es una herramienta que te ayuda a trabajar con un agente de IA (como Claude Code o GitHub Copilot) de forma más ordenada y predecible. En vez de darle prompts sueltos y esperar que adivine lo que querés, primero escribís una especificación detallada de lo que querés construir — y el agente la usa como guía en cada paso.

La idea central es simple: **especificá primero, codificá después.** Así el agente sabe exactamente qué tiene que hacer, y vos podés revisar y corregir antes de que genere código.

---

## Paso 0: Lo que necesitás antes de empezar

Instalá estas herramientas antes de continuar. Hacé clic en cada link para ir a la página de descarga:

| Herramienta | Para qué sirve | Link |
|-------------|---------------|------|
| **Python 3.11+** | Spec-Kit está hecho en Python | [Descargar Python](https://www.python.org/downloads/) |
| **uv** | Manejador de paquetes Python (instala Spec-Kit) | [Instalar uv](https://docs.astral.sh/uv/getting-started/installation/) |
| **Git** | Control de versiones (Spec-Kit lo usa para crear ramas) | [Descargar Git](https://git-scm.com/downloads) |
| **PowerShell 7+** | Terminal recomendada en Windows | [Instalar PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.4) |
| **Un agente de IA** | El que va a ejecutar los comandos | [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) · [GitHub Copilot](https://github.com/features/copilot) · [Cursor](https://cursor.sh) · [Gemini CLI](https://ai.google.dev/gemini-api/docs) |

---

## Paso 1: Instalar Specify

`specify` es el CLI de Spec-Kit. Tenés dos formas de usarlo:

### Opción A: Instalación global (recomendado)

Instalalo una vez y podés usarlo en cualquier proyecto sin volver a instalarlo.

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

Verificá que quedó bien instalado:

```bash
specify check
```

Si no hay errores, estás listo. Pasá al Paso 2.

### Opción B: Sin instalar (con `uvx`)

Si no querés instalarlo globalmente o solo querés probarlo, podés ejecutarlo directamente. El contra es que cada comando tarda más porque descarga todo desde cero.

```bash
uvx --from git+https://github.com/github/spec-kit.git specify check
```

> En la Opción B, cada comando que en esta guía dice `specify ...` lo tenés que escribir como `uvx --from git+https://github.com/github/spec-kit.git specify ...`

---

## Paso 2: Crear tu primer proyecto

Abrí una terminal en la carpeta donde querés crear el proyecto y ejecutá:

```bash
specify init mi-proyecto --integration claude
```

Reemplazá `mi-proyecto` con el nombre que quieras y `claude` con el agente que uses:

| Agente | Comando de inicialización |
|--------|--------------------------|
| Claude Code | `specify init mi-proyecto --integration claude` |
| GitHub Copilot | `specify init mi-proyecto --integration copilot` |
| Cursor | `specify init mi-proyecto --integration cursor-agent` |
| Gemini CLI | `specify init mi-proyecto --integration gemini` |
| Windsurf | `specify init mi-proyecto --integration windsurf` |
| Codex CLI | `specify init mi-proyecto --integration codex` |
| Kiro CLI | `specify init mi-proyecto --integration kiro-cli` |
| Goose | `specify init mi-proyecto --integration goose` |
| Tabnine | `specify init mi-proyecto --integration tabnine` |
| Qwen Code | `specify init mi-proyecto --integration qwen` |
| opencode | `specify init mi-proyecto --integration opencode` |
| Kilo Code | `specify init mi-proyecto --integration kilocode` |
| Auggie CLI | `specify init mi-proyecto --integration auggie` |
| Roo Code | `specify init mi-proyecto --integration roo` |
| Forge | `specify init mi-proyecto --integration forge` |
| Qoder CLI | `specify init mi-proyecto --integration qodercli` |
| Google Antigravity | `specify init mi-proyecto --integration agy` |
| Pi | `specify init mi-proyecto --integration pi` |
| Kimi | `specify init mi-proyecto --integration kimi` |
| Vibe | `specify init mi-proyecto --integration vibe` |
| Trae | `specify init mi-proyecto --integration trae` |
| Shai | `specify init mi-proyecto --integration shai` |
| Junie | `specify init mi-proyecto --integration junie` |
| Genérico | `specify init mi-proyecto --integration generic` |

**¿Querés inicializar en la carpeta donde ya estás parado?**

```bash
specify init --here --integration claude
```

**¿Tu carpeta ya tiene archivos y te da error?** Usá `--force`:

```bash
specify init --here --integration claude --force
```

Cuando termine, Spec-Kit crea una carpeta `.specify/` con las plantillas y los comandos del agente. A partir de acá, todo se hace desde el agente de IA.

---

## Paso 3: Construir tu feature, paso a paso

Una vez que el proyecto está inicializado, abrís tu agente de IA en esa carpeta y usás estos comandos en orden. Los comandos que dicen *opcional* los podés saltear si estás apurado.

### 3.1 Definí las reglas del proyecto — `/speckit.constitution`

**¿Qué hace?** Le decís al agente cuáles son los principios que tiene que respetar en todo momento: qué tecnologías usar, qué estilo de código, qué prioridades. Esto queda guardado y todos los comandos siguientes lo leen antes de actuar.

**¿Cuándo lo hacés?** Una sola vez por proyecto, al principio.

```bash
/speckit.constitution Crear una app de gestión de tareas. Priorizar simplicidad y usabilidad.
```

```bash
/speckit.constitution Construir una API REST. Stack: Node.js + TypeScript. Priorizar escalabilidad y documentación clara.
```

---

### 3.2 Describí qué querés construir — `/speckit.specify`

**¿Qué hace?** Convierte tu descripción en una especificación formal con casos de uso, requisitos y criterios de éxito. También crea una rama de Git automáticamente.

**¿Cuándo lo hacés?** Una vez por feature o funcionalidad que querés agregar.

```bash
/speckit.specify Sistema de login con usuario y contraseña, registro de nuevos usuarios y recuperación de contraseña por email
```

```bash
/speckit.specify Panel de administración para ver usuarios registrados, cambiar su rol y desactivar cuentas
```

> No tomes el primer resultado como definitivo. Leé la spec generada y preguntale al agente si algo no quedó claro o falta algo.

---

### 3.3 Aclará dudas antes de seguir — `/speckit.clarify` *(opcional)*

**¿Qué hace?** El agente revisa la spec que generaste y te pregunta todo lo que puede ser ambiguo antes de que empiece a planificar. Mucho mejor descubrirlo acá que cuando ya generó código.

**¿Cuándo lo usás?** Siempre que la spec haya quedado incompleta o cuando no estés 100% seguro de los detalles.

```bash
/speckit.clarify
```

Respondé las preguntas del agente. Al terminar, la spec se actualiza con tus respuestas.

---

### 3.4 Definí cómo se va a construir — `/speckit.plan`

**¿Qué hace?** El agente diseña la arquitectura técnica: qué tecnologías usar, cómo se estructura la base de datos, cómo se comunican los componentes. Genera archivos de plan, modelo de datos y contratos de API.

**¿Cuándo lo hacés?** Después de tener la spec finalizada.

```bash
/speckit.plan Stack: Next.js + TypeScript + Tailwind. Base de datos: PostgreSQL con Prisma. Deploy en Vercel.
```

```bash
/speckit.plan Backend en .NET 8 con Entity Framework. Frontend en Blazor. Base de datos: SQL Server.
```

> Después de que el agente genere el plan, revisalo. Si algo parece sobrediseñado para lo que necesitás, decíselo y pedile que lo simplifique.

---

### 3.5 Verificá la calidad del plan — `/speckit.checklist` *(opcional)*

**¿Qué hace?** Genera checklists de calidad para validar que la spec y el plan estén completos. Funciona como una auditoría antes de codificar: revisa UX, seguridad, accesibilidad, rendimiento, etc.

```bash
/speckit.checklist
```

---

### 3.6 Dividí el trabajo en tareas — `/speckit.tasks`

**¿Qué hace?** Toma el plan y lo desglosa en tareas concretas y pequeñas, con estimaciones de tiempo y dependencias entre ellas. Genera un archivo `tasks.md`.

**¿Cuándo lo hacés?** Después de tener el plan aprobado.

```bash
/speckit.tasks
```

---

### 3.7 Convertí las tareas en issues de GitHub — `/speckit.taskstoissues` *(opcional)*

**¿Qué hace?** Crea un issue de GitHub por cada tarea del `tasks.md`. Útil si trabajás en equipo y querés trackear el progreso en el tablero del repositorio.

**Requisito:** Tener [GitHub CLI](https://cli.github.com/) instalado y autenticado.

```bash
/speckit.taskstoissues
```

---

### 3.8 Verificá que todo sea consistente — `/speckit.analyze` *(opcional)*

**¿Qué hace?** Revisa que la spec, el plan y las tareas no se contradigan entre sí. Detecta huecos de cobertura, ambigüedades y duplicaciones. Solo lee, no modifica nada.

**¿Cuándo lo usás?** Después de generar las tareas y antes de empezar a implementar.

```bash
/speckit.analyze
```

---

### 3.9 Implementá — `/speckit.implement`

**¿Qué hace?** Ejecuta todas las tareas: genera el código, los tests, la documentación y todo lo que haga falta para que la feature funcione.

```bash
/speckit.implement
```

Si querés más control sobre el proceso:

```bash
# El agente te pregunta antes de avanzar a la siguiente tarea
/speckit.implement Ejecutar las tareas una por una. Preguntar antes de avanzar.

# Implementar solo una parte
/speckit.implement Implementar únicamente el sistema de autenticación
```

> Vos seguís siendo el que toma las decisiones. Revisá el código generado en cada tarea antes de aprobar la siguiente.

---

## Resumen del flujo completo

```
┌─────────────────────────────────────────────┐
│   Definición                                │
│                                             │
│  1. /speckit.constitution                   │
│  2. /speckit.specify                        │
│  3. /speckit.clarify        (opcional)      │
├─────────────────────────────────────────────┤
│   Planificación                             │
│                                             │
│  4. /speckit.plan                           │
│  5. /speckit.checklist      (opcional)      │
│  6. /speckit.tasks                          │
│  7. /speckit.taskstoissues  (opcional)      │
│  8. /speckit.analyze        (opcional)      │
├─────────────────────────────────────────────┤
│   Implementación                            │
│                                             │
│  9. /speckit.implement                      │
└─────────────────────────────────────────────┘
```

### Referencia rápida de comandos

| Comando | Tipo | Qué hace |
|---------|------|----------|
| `/speckit.constitution` | Core | Define las reglas y principios del proyecto |
| `/speckit.specify` | Core | Convierte tu idea en una especificación formal |
| `/speckit.clarify` | Opcional | Aclara ambigüedades antes de planificar |
| `/speckit.plan` | Core | Diseña la arquitectura y el stack técnico |
| `/speckit.checklist` | Opcional | Auditoría de calidad de la spec y el plan |
| `/speckit.tasks` | Core | Divide el plan en tareas concretas |
| `/speckit.taskstoissues` | Opcional | Crea issues de GitHub por cada tarea |
| `/speckit.analyze` | Opcional | Verifica consistencia entre spec, plan y tareas |
| `/speckit.implement` | Core | Genera el código según todo lo anterior |

---

## Ejemplo completo: sistema de blog

Este es el flujo completo de una feature de principio a fin:

```bash
# 1. Definir principios del proyecto
/speckit.constitution Blog personal con énfasis en simplicidad y rendimiento

# 2. Describir la feature
/speckit.specify Sistema de posts con editor markdown, categorías, tags y comentarios

# 3. Clarificar dudas (recomendado)
/speckit.clarify

# 4. Diseñar la arquitectura
/speckit.plan Stack: Next.js + TypeScript + Tailwind. BD: PostgreSQL + Prisma. Auth: NextAuth.js. Deploy: Vercel.

# 5. Generar tareas
/speckit.tasks

# 6. Verificar consistencia (recomendado)
/speckit.analyze

# 7. Implementar
/speckit.implement
```

---

## ¿Tenés un proyecto que ya existe? (Brownfield)

Si ya tenés código y querés agregar una feature nueva con Spec-Kit, el proceso es casi igual. La diferencia es que el agente tiene que entender primero lo que ya existe.

### Paso 0: Instalar Spec-Kit en tu proyecto existente

```bash
cd mi-proyecto-existente
specify init --here --integration claude --force
```

El flag `--force` hace que Spec-Kit se instale sin pisar los archivos que ya tenés.

### Paso 1: Que el agente entienda tu codebase

Antes de correr cualquier comando de Spec-Kit, pedile al agente que estudie el código:

```bash
Analizá todo el codebase actual. Documentá la arquitectura, los módulos principales,
los flujos de datos y las convenciones existentes. Guardá todo en docs/codebase-research.md
```

### Paso 2: Constitución basada en lo que ya existe

```bash
/speckit.constitution
```

El agente va a generar los principios analizando tu código real — el stack que ya usás, las convenciones de naming, los patrones de arquitectura. Revisá el resultado y corregí lo que no coincida.

### Pasos 3 en adelante: igual que en greenfield

Seguí el mismo flujo de `/speckit.specify`, `/speckit.clarify`, `/speckit.plan`, etc. La diferencia es que en el plan le aclarás que tiene que usar los componentes y servicios que ya existen:

```bash
/speckit.plan Usar los componentes existentes del proyecto.
Referir a docs/codebase-research.md para contexto.
No crear nuevas abstracciones si ya existe algo que sirva.
```

> En brownfield, conviene que cada feature toque la menor cantidad de archivos posible. Si una implementación modifica más de 30 archivos, dividila en features más chicas.

#### Recursos sobre brownfield

- [Discusión oficial: Recommended approach for brownfield development](https://github.com/github/spec-kit/discussions/331)
- [Discusión: Spec-kit for complex brownfield projects](https://github.com/github/spec-kit/discussions/746)
- [Microsoft Learn: Implement SDD with Spec Kit](https://learn.microsoft.com/en-us/training/modules/spec-driven-development-github-spec-kit-enterprise-developers/)

---

## Agentes de IA soportados

Spec-Kit funciona con más de 30 agentes. Estos son los principales:

| Agente | Link | Formato |
|--------|------|---------|
| [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) | ✅ | `.md` |
| [GitHub Copilot](https://github.com/features/copilot) | ✅ | `.agent.md` |
| [Gemini CLI](https://ai.google.dev/gemini-api/docs) | ✅ | `.toml` |
| [Google Antigravity](https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform/) | ✅ | `.md` |
| [Cursor](https://cursor.sh) | ✅ | `.md` |
| [Windsurf](https://windsurf.ai) | ✅ | `.md` |
| [Codex CLI](https://openai.com/blog/openai-codex) | ✅ | `.md` |
| [Kiro CLI](https://kiro.dev) | ✅ | `.md` |
| [Goose](https://block.github.io/goose/) | ✅ | `.md` |
| [Tabnine](https://www.tabnine.com) | ✅ | `.md` |
| [Mistral](https://mistral.ai) | ✅ | `.md` |
| [Qwen Code](https://qwenlm.github.io/) | ✅ | `.toml` |
| [opencode](https://opencode.com) | ✅ | `.md` |
| [Kilo Code](https://kilocode.ai) | ✅ | `.md` |
| [Auggie CLI](https://auggie.ai) | ✅ | `.md` |
| [Roo Code](https://roocode.ai) | ✅ | `.md` |
| Forge | ✅ | `.md` |
| Qoder CLI | ✅ | `.md` |
| Genérico (`--integration generic`) | ✅ | `.md` |

> Los alias cortos también funcionan: `--integration kiro` se resuelve a `kiro-cli`, etc.

### Google Antigravity

[Antigravity](https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform/) es el IDE agente de Google, disponible en preview gratuita. Usa Gemini 3 Pro por defecto, pero también soporta Claude Sonnet y GPT. A diferencia de los otros agentes, Antigravity tiene sus propios comandos equivalentes al flujo de Spec-Kit:

| Comando Spec-Kit | Equivalente en Antigravity | Qué hace |
|-----------------|---------------------------|----------|
| `/speckit.specify` | `/define` | Crea la spec (acepta screenshots y bocetos) |
| `/speckit.plan` | `/architect` | Genera el plan técnico y grafo de tareas |
| `/speckit.implement` | `/execute` | Lanza agentes para construir en paralelo |
| `/speckit.analyze` | `/verify` | Pruebas de regresión visual y lógica |
| `/speckit.constitution` | `/refactor` | Audita y refactoriza según la constitución |

Para integrar Spec-Kit con Antigravity existe el proyecto [spec-kit-antigravity](https://github.com/waveupHQ/spec-kit-antigravity), que adapta las plantillas al formato nativo de Antigravity.

> 💡 El flag `--ai-skills` instala las skills base comunes a todos los agentes. Podés usarlo con cualquier integración, no es exclusivo de Antigravity:
> ```bash
> specify init mi-proyecto --integration agy --ai-skills  # con skills base
> specify init mi-proyecto --integration agy               # sin skills base
> ```

#### Extension Hooks en Antigravity

Antigravity ejecuta hooks automáticos antes de procesar cada comando. El más importante es el **pre-hook de git**, que se dispara automáticamente al comenzar cualquier flujo:

```
Extension Hooks
Automatic Pre-Hook: git
Executing: /speckit.git.initialize
EXECUTE_COMMAND: speckit.git.initialize
```

Este hook inicializa y configura el repositorio Git antes de que el agente genere el outline del proyecto. No hace falta invocarlo manualmente — Antigravity lo ejecuta solo. Hay que esperar a que termine antes de continuar con el siguiente paso.

---

## Estructura de archivos que genera Spec-Kit

Después de inicializar y correr algunos comandos, tu proyecto va a tener esta estructura:

```
tu-proyecto/
├── .specify/
│   ├── memory/
│   │   └── constitution.md       ← Las reglas del proyecto
│   └── templates/                ← Plantillas internas (no tocar)
├── .claude/commands/             ← Los comandos del agente (o .github/agents/, .cursor/, etc.)
│   ├── speckit.constitution.md
│   ├── speckit.specify.md
│   ├── speckit.clarify.md
│   ├── speckit.plan.md
│   ├── speckit.tasks.md
│   ├── speckit.taskstoissues.md
│   ├── speckit.implement.md
│   ├── speckit.analyze.md
│   └── speckit.checklist.md
└── specs/
    └── 001-mi-feature/           ← Se crea con /speckit.specify
        ├── spec.md               ← La especificación
        ├── plan.md               ← El plan técnico
        ├── tasks.md              ← Las tareas
        ├── data-model.md         ← El modelo de datos
        └── checklists/           ← Los checklists de calidad
```

---

## Extensiones (funcionalidades extra)

Spec-Kit tiene un sistema de extensiones para agregar integraciones con otras herramientas. Hay más de 150 extensiones comunitarias disponibles.

```bash
# Ver extensiones disponibles
specify extension search

# Instalar una extensión
specify extension add <extension-id>

# Ver extensiones instaladas
specify extension list

# Actualizar todas las extensiones
specify extension update

# Desinstalar una extensión
specify extension remove <extension-id>
```

---

## Recursos adicionales

- [Repositorio oficial](https://github.com/github/spec-kit)
- [Releases y plantillas](https://github.com/github/spec-kit/releases)
- [Blog de anuncio — GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [Guía detallada — Microsoft Developer Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
- [Explicación en profundidad — Den Delimarsky](https://den.dev/blog/github-spec-kit/)
- [Curso en LinkedIn Learning](https://github.com/LinkedInLearning/spec-driven-development-with-github-spec-kit-4641001)
- [IaC Spec Kit (IBM)](https://github.com/IBM/iac-spec-kit) — Adaptación para Infrastructure as Code

---

**¿Necesitás ayuda?** Abrí un [issue en el repositorio oficial](https://github.com/github/spec-kit/issues).

**¿Te sirvió esta guía?** ¡Dale una estrella al repositorio! ⭐
