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

**Ejemplos de uso**:
```bash
# Aplicación de gestión de tareas
/constitution "Crear una aplicación de gestión de tareas con principios de simplicidad, usabilidad y productividad"

# E-commerce
/constitution "Desarrollar una plataforma de e-commerce enfocada en seguridad, rendimiento y experiencia de usuario"

# Blog personal
/constitution "Crear un blog personal con enfoque en simplicidad, accesibilidad y velocidad de carga"

# API REST
/constitution "Construir una API REST robusta con principios de escalabilidad, mantenibilidad y documentación clara"
```

### 2. 📋 Especificación (`/specify`)
**Objetivo**: Convierte descripciones de características en requisitos detallados

**Resultado**: Genera:
- Archivo `specs/NNN-feature/spec.md`
- Rama de Git correspondiente
- Especificaciones técnicas detalladas

**Ejemplos de uso**:
```bash
# Sistema de autenticación
/specify "Sistema de autenticación de usuarios con login, registro, recuperación de contraseña y autenticación de dos factores"

# Panel de administración
/specify "Panel de administración para gestionar usuarios, productos, pedidos y estadísticas de ventas"

# Sistema de notificaciones
/specify "Sistema de notificaciones en tiempo real con email, SMS y notificaciones push"

# API de pagos
/specify "API de pagos integrada con múltiples procesadores (Stripe, PayPal) y gestión de suscripciones"

# Dashboard de analytics
/specify "Dashboard de analytics con gráficos interactivos, reportes personalizados y exportación de datos"
```

### 3. 🛠️ Stack Tecnológico (`/plan`)
**Objetivo**: Define la arquitectura y tecnologías a utilizar en el proyecto

**Resultado**: Crea archivos como:
- `plan.md` - Plan de implementación con stack tecnológico
- `data-model.md` - Modelo de datos
- `contracts/` - Contratos de API
- `research.md` - Investigación de tecnologías

**Ejemplos de uso**:
```bash
# Aplicación web de gestión de tareas
/plan "Definir stack tecnológico para una aplicación web de gestión de tareas con autenticación y base de datos"

# E-commerce completo
/plan "Establecer arquitectura tecnológica para un e-commerce con carrito, pagos y gestión de inventario"

# API de microservicios
/plan "Diseñar stack tecnológico para una arquitectura de microservicios con autenticación, logging y monitoreo"

# Aplicación móvil
/plan "Definir tecnologías para una aplicación móvil híbrida con sincronización offline"

# Sistema de analytics
/plan "Configurar stack para un sistema de analytics con procesamiento de datos en tiempo real"
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

**Ejemplos de uso**:
```bash
# Sistema de autenticación
/tasks "Crear las tareas para implementar el sistema de autenticación con login, registro y 2FA"

# Dashboard de admin
/tasks "Desglosar en tareas el desarrollo del panel de administración con CRUD completo"

# API REST
/tasks "Definir tareas para construir la API REST con endpoints, validación y documentación"

# Sistema de pagos
/tasks "Crear tareas para implementar el sistema de pagos con integración a Stripe y PayPal"

# Aplicación móvil
/tasks "Desglosar el desarrollo de la aplicación móvil en tareas de UI, lógica y testing"
```

### 5. 🚀 Implementación (`/implement`)
**Objetivo**: Ejecuta la implementación planificada

**Resultado**: 
- Genera artefactos de código
- Actualiza el progreso
- Implementa las funcionalidades especificadas

**Ejemplos de uso**:
```bash
# Implementar sistema de autenticación
/implement "Ejecutar la implementación de las tareas de autenticación con login, registro y 2FA"

# Crear panel de administración
/implement "Desarrollar el panel de administración con todas las funcionalidades CRUD especificadas"

# Construir API REST
/implement "Implementar la API REST completa con endpoints, validación y documentación"

# Sistema de pagos
/implement "Desarrollar el sistema de pagos con integración a múltiples procesadores"

# Aplicación completa
/implement "Ejecutar la implementación completa de la aplicación siguiendo todas las tareas definidas"

# Feature específica
/implement "Implementar únicamente la funcionalidad de notificaciones push"
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

## Ejemplos Prácticos Completos

### Ejemplo 1: Sistema de Blog

#### Fase 1: Constitución
```bash
/constitution "Crear un sistema de blog personal con énfasis en simplicidad, rendimiento y facilidad de uso"
```

#### Fase 2: Especificación
```bash
/specify "Sistema de gestión de posts con editor markdown, categorías, tags y comentarios"
```

#### Fase 3: Stack Tecnológico
```bash
/plan "Definir stack tecnológico para el sistema de blog"

# Stack resultante:
# Frontend: Next.js + React + TypeScript + Tailwind CSS
# Backend: Next.js API Routes
# Base de Datos: PostgreSQL + Prisma ORM
# Autenticación: NextAuth.js
# Despliegue: Vercel
```

#### Fase 4: Tareas
```bash
/tasks "Desglosar implementación en tareas específicas de desarrollo"
```

#### Fase 5: Implementación
```bash
/implement "Ejecutar la implementación del sistema de blog"
```

---

### Ejemplo 2: E-commerce Básico

#### Fase 1: Constitución
```bash
/constitution "Desarrollar una tienda online con enfoque en seguridad, usabilidad y conversión de ventas"
```

#### Fase 2: Especificación
```bash
/specify "Plataforma de e-commerce con catálogo de productos, carrito de compras, checkout y gestión de pedidos"
```

#### Fase 3: Stack Tecnológico
```bash
/plan "Establecer stack tecnológico para el e-commerce con pagos seguros"

# Stack resultante:
# Frontend: React + Next.js + TypeScript + Tailwind CSS
# Backend: Node.js + Express + TypeScript
# Base de Datos: PostgreSQL + Prisma ORM
# Pagos: Stripe + PayPal
# Despliegue: Vercel + Railway
```

#### Fase 4: Tareas
```bash
/tasks "Crear tareas para implementar catálogo, carrito, checkout y sistema de pagos"
```

#### Fase 5: Implementación
```bash
/implement "Desarrollar la tienda online completa con todas las funcionalidades"
```

---

### Ejemplo 3: API de Microservicios

#### Fase 1: Constitución
```bash
/constitution "Construir una arquitectura de microservicios robusta con principios de escalabilidad y mantenibilidad"
```

#### Fase 2: Especificación
```bash
/specify "API de microservicios con autenticación, gestión de usuarios, logging y monitoreo"
```

#### Fase 3: Stack Tecnológico
```bash
/plan "Diseñar stack tecnológico para microservicios con Docker y Kubernetes"

# Stack resultante:
# Backend: Node.js + Express + TypeScript
# Base de Datos: PostgreSQL + Redis
# Contenedores: Docker + Docker Compose
# Orquestación: Kubernetes
# Monitoreo: Prometheus + Grafana
```

#### Fase 4: Tareas
```bash
/tasks "Desglosar en tareas el desarrollo de cada microservicio y su infraestructura"
```

#### Fase 5: Implementación
```bash
/implement "Implementar la arquitectura de microservicios completa"
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
