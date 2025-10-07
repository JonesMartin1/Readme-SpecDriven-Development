# Guía de Spec-Driven Development con GitHub Spec-Kit

## ¿Qué es Spec-Driven Development (SDD)?

**Spec-Driven Development** es una metodología de desarrollo que prioriza la creación de especificaciones detalladas antes de la implementación del código. Este enfoque asegura que el desarrollo esté completamente alineado con los requisitos y objetivos del proyecto.

## ¿Qué es GitHub Spec-Kit?

GitHub Spec-Kit es una herramienta que facilita el desarrollo basado en especificaciones, implementando un flujo de trabajo estructurado de cinco fases que transforma ideas en implementaciones ejecutables.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Python 3.11 o superior**
- **Gestor de paquetes `uv`**
- **Git configurado en tu sistema**
- **Un agente de codificación asistido por IA** (como GitHub Copilot, Claude Code o Gemini CLI)

## Instalación

```bash
# Instalar uv si no lo tienes
pip install uv

# Clonar el repositorio spec-kit
git clone https://github.com/github/spec-kit.git
cd spec-kit

# Instalar dependencias
uv sync
```

## Flujo de Trabajo de 5 Fases

### 1. 🏛️ Constitución (`/constitution`)
**Objetivo**: Define los principios de gobernanza del proyecto

**Resultado**: Crea el archivo `memory/constitution.md` con:
- Principios fundamentales del proyecto
- Reglas de decisión
- Estándares de calidad
- Políticas de desarrollo

**Ejemplo de uso**:
```bash
/constitution "Crear una aplicación de gestión de tareas con principios de simplicidad y usabilidad"
```

### 2. 📋 Especificación (`/specify`)
**Objetivo**: Convierte descripciones de características en requisitos detallados

**Resultado**: Genera:
- Archivo `specs/NNN-feature/spec.md`
- Rama de Git correspondiente
- Especificaciones técnicas detalladas

**Ejemplo de uso**:
```bash
/specify "Sistema de autenticación de usuarios con login, registro y recuperación de contraseña"
```

### 3. 🛠️ Stack Tecnológico (`/plan`)
**Objetivo**: Define la arquitectura y tecnologías a utilizar en el proyecto

**Resultado**: Crea archivos como:
- `plan.md` - Plan de implementación con stack tecnológico
- `data-model.md` - Modelo de datos
- `contracts/` - Contratos de API
- `research.md` - Investigación de tecnologías

**Ejemplo de uso**:
```bash
/plan "Definir stack tecnológico para una aplicación web de gestión de tareas"
```

**Ejemplo de Stack Tecnológico**:
```
Frontend: React + TypeScript + Tailwind CSS
Backend: Node.js + Express + TypeScript
Base de Datos: PostgreSQL + Prisma ORM
Autenticación: JWT + bcrypt
Despliegue: Vercel (Frontend) + Railway (Backend)
```

**Otros Ejemplos de Stacks Comunes**:

**Stack Full-Stack JavaScript**:
```
Frontend: Next.js + React + TypeScript
Backend: Next.js API Routes
Base de Datos: MongoDB + Mongoose
Autenticación: NextAuth.js
Despliegue: Vercel
```

**Stack Python**:
```
Frontend: React + Vite
Backend: Python + FastAPI
Base de Datos: PostgreSQL + SQLAlchemy
Autenticación: JWT + PassLib
Despliegue: Netlify (Frontend) + Heroku (Backend)
```

**Stack PHP**:
```
Frontend: Vue.js + Vite
Backend: PHP + Laravel
Base de Datos: MySQL + Eloquent ORM
Autenticación: Laravel Sanctum
Despliegue: Netlify (Frontend) + DigitalOcean (Backend)
```

### 4. ✅ Tareas (`/tasks`)
**Objetivo**: Desglosa el plan técnico en tareas de desarrollo accionables

**Resultado**: Genera `tasks.md` con:
- Lista de tareas específicas
- Priorización
- Estimaciones de tiempo
- Dependencias entre tareas

**Ejemplo de uso**:
```bash
/tasks "Crear las tareas para implementar el sistema de autenticación"
```

### 5. 🚀 Implementación (`/implement`)
**Objetivo**: Ejecuta la implementación planificada

**Resultado**: 
- Genera artefactos de código
- Actualiza el progreso
- Implementa las funcionalidades especificadas

**Ejemplo de uso**:
```bash
/implement "Ejecutar la implementación de las tareas de autenticación"
```

## Comandos Principales

| Comando | Descripción | Archivos Generados |
|---------|-------------|-------------------|
| `/constitution` | Establece principios de gobernanza | `memory/constitution.md` |
| `/specify` | Crea especificaciones detalladas | `specs/NNN-feature/spec.md` |
| `/plan` | Define stack tecnológico y arquitectura | `plan.md`, `data-model.md`, `contracts/` |
| `/tasks` | Desglosa en tareas específicas | `tasks.md` |
| `/implement` | Ejecuta la implementación | Código y artefactos |

## Estructura de Archivos del Proyecto

```
tu-proyecto/
├── memory/
│   └── constitution.md          # Principios de gobernanza
├── specs/
│   └── 001-autenticacion/
│       └── spec.md             # Especificación de característica
├── plan.md                     # Plan de implementación
├── data-model.md              # Modelo de datos
├── contracts/                 # Contratos de API
├── tasks.md                   # Lista de tareas
├── research.md               # Investigación técnica
└── README.md                 # Este archivo
```

## Ventajas del Spec-Driven Development

### 🎯 **Claridad de Objetivos**
- Requisitos claramente definidos desde el inicio
- Reducción de ambigüedades
- Mejor comunicación entre equipos

### 🔄 **Iteración Controlada**
- Desarrollo incremental basado en especificaciones
- Validación continua de requisitos
- Facilita la retroalimentación

### 📈 **Calidad Mejorada**
- Código más robusto y mantenible
- Menos bugs en producción
- Mejor cobertura de casos de uso

### ⏱️ **Eficiencia**
- Menos tiempo en debugging
- Desarrollo más predecible
- Mejor estimación de tiempos

## Mejores Prácticas

### 1. **Especificaciones Detalladas**
- Incluye casos de uso específicos
- Define criterios de aceptación claros
- Considera casos edge y errores

### 2. **Planificación Realista**
- Desglosa tareas en componentes pequeños
- Estima tiempos de manera conservadora
- Identifica dependencias críticas

### 3. **Implementación Iterativa**
- Implementa y prueba en pequeños incrementos
- Valida contra especificaciones continuamente
- Mantén documentación actualizada

### 4. **Revisión Continua**
- Revisa especificaciones regularmente
- Actualiza planes según feedback
- Ajusta tareas según progreso

## Ejemplo Práctico: Sistema de Blog

### Fase 1: Constitución
```bash
/constitution "Crear un sistema de blog personal con énfasis en simplicidad, rendimiento y facilidad de uso"
```

### Fase 2: Especificación
```bash
/specify "Sistema de gestión de posts con editor markdown, categorías, tags y comentarios"
```

### Fase 3: Stack Tecnológico
```bash
/plan "Definir stack tecnológico para el sistema de blog"

# Stack resultante:
# Frontend: Next.js + React + TypeScript + Tailwind CSS
# Backend: Next.js API Routes
# Base de Datos: PostgreSQL + Prisma ORM
# Autenticación: NextAuth.js
# Despliegue: Vercel
```

### Fase 4: Tareas
```bash
/tasks "Desglosar implementación en tareas específicas de desarrollo"
```

### Fase 5: Implementación
```bash
/implement "Ejecutar la implementación del sistema de blog"
```

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
# Crear nueva rama para feature
git checkout -b feature/nueva-funcionalidad

# Trabajar con spec-kit
/specify "Nueva funcionalidad"
/plan "Implementar nueva funcionalidad"
/tasks "Crear tareas para nueva funcionalidad"
/implement "Ejecutar implementación"

# Commit y push
git add .
git commit -m "Implementar nueva funcionalidad"
git push origin feature/nueva-funcionalidad

# Crear Pull Request en GitHub
```

## Recursos Adicionales

- [Repositorio oficial de GitHub Spec-Kit](https://github.com/github/spec-kit)
- [Documentación de Spec-Driven Development](https://en.wikipedia.org/wiki/Specification_by_example)
- [GitHub Copilot](https://github.com/features/copilot)
- [Claude Code](https://claude.ai)

## Contribuciones

¡Las contribuciones son bienvenidas! Si tienes sugerencias, mejoras o encuentras bugs:

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

---

**¿Necesitas ayuda?** Abre un issue en el repositorio o contacta al equipo de desarrollo.

**¿Te gustó esta guía?** ¡Dale una estrella al repositorio! ⭐
