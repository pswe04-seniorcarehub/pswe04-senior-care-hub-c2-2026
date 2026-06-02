# Senior Care Hub

> Plataforma inteligente distribuida de monitoreo y asistencia para adultos mayores.

[![Estado](https://img.shields.io/badge/estado-en%20desarrollo-yellow)]()
[![Curso](https://img.shields.io/badge/curso-PSWE04-blue)]()
[![Ciclo](https://img.shields.io/badge/ciclo-C2%202026-lightgrey)]()

---

## Tabla de contenidos

- [Descripción](#descripción)
- [Características principales](#características-principales)
- [Arquitectura](#arquitectura)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Stack tecnológico](#stack-tecnológico)
- [Requisitos previos](#requisitos-previos)
- [Instalación y ejecución](#instalación-y-ejecución)
- [Flujo de trabajo con Git](#flujo-de-trabajo-con-git)
- [Documentación](#documentación)
- [Decisiones arquitectónicas (ADR)](#decisiones-arquitectónicas-adr)
- [Equipo](#equipo)
- [Licencia](#licencia)

---

## Descripción

**SeniorCareHub** es una plataforma distribuida orientada al monitoreo y la asistencia de
adultos mayores. Su objetivo es integrar la información proveniente de distintas fuentes
(sensores, dispositivos, cuidadores y familiares) en un sistema centralizado e inteligente
que permita tomar decisiones oportunas sobre el bienestar de la persona.

<!-- [POR COMPLETAR] Amplía aquí el problema que resuelve el proyecto, el contexto
académico y los objetivos generales y específicos. Puedes apoyarte en el documento
docs/s03-propuesta.md. -->

## Características principales

<!-- [POR COMPLETAR] Reemplaza estos ejemplos por las funcionalidades reales del sistema. -->

- Monitoreo en tiempo real del estado y la actividad del adulto mayor.
- Generación de alertas y notificaciones ante eventos relevantes.
- Panel de seguimiento para cuidadores y familiares.
- Arquitectura distribuida que separa responsabilidades en distintos servicios.

## Arquitectura

El sistema sigue un enfoque **distribuido**. Los diagramas que describen la arquitectura
(componentes, despliegue, secuencia, etc.) se encuentran en la carpeta [`diagramas/`](./diagramas).

<!-- [POR COMPLETAR] Inserta aquí un resumen de la arquitectura y, si lo deseas, una
imagen del diagrama principal:

![Diagrama de arquitectura](./diagramas/arquitectura.png)
-->

## Estructura del repositorio

```
.
├── docs/             # Documentación del proyecto (propuesta, especificaciones, etc.)
│   └── s03-propuesta.md
├── decisiones/       # Decisiones arquitectónicas documentadas como ADR
├── diagramas/        # Diagramas del proyecto (arquitectura, despliegue, secuencia)
└── README.md         # Este archivo
```

<!-- [POR COMPLETAR] Actualiza la estructura conforme se agreguen carpetas de código
(por ejemplo: src/, backend/, frontend/, tests/). -->

## Stack tecnológico

<!-- [POR COMPLETAR] Lista las tecnologías reales del proyecto. A continuación, una
plantilla de ejemplo para un sistema distribuido. -->

| Capa            | Tecnología        |
|-----------------|-------------------|
| Backend         | [POR DEFINIR]     |
| Frontend        | [POR DEFINIR]     |
| Base de datos   | [POR DEFINIR]     |
| Mensajería      | [POR DEFINIR]     |
| Infraestructura | [POR DEFINIR]     |

## Requisitos previos

<!-- [POR COMPLETAR] Especifica las versiones reales requeridas. -->

- Git
- [POR DEFINIR] (p. ej. Node.js / .NET / Python / Docker)

## Instalación y ejecución

```bash
# 1. Clonar el repositorio
git clone https://github.com/pswe04-seniorcarehub/pswe04-senior-care-hub-c2-2026.git
cd pswe04-senior-care-hub-c2-2026

# 2. [POR COMPLETAR] Pasos de instalación de dependencias

# 3. [POR COMPLETAR] Pasos de ejecución del proyecto
```

## Flujo de trabajo con Git

El proyecto utiliza un flujo de ramas basado en **Git Flow**:

- **`main`** — rama estable, contiene versiones listas para entrega.
- **`develop`** — rama de integración del trabajo en curso.
- **`feature/*`** — ramas para el desarrollo de funcionalidades específicas
  (p. ej. `feature/propuesta`).

Para contribuir:

```bash
git checkout develop
git checkout -b feature/nombre-de-la-funcionalidad
# ...realizar cambios y commits...
git push origin feature/nombre-de-la-funcionalidad
# Abrir un Pull Request hacia develop
```

## Documentación

La documentación del proyecto se encuentra en la carpeta [`docs/`](./docs):

- [`docs/s03-propuesta.md`](./docs/s03-propuesta.md) — Propuesta inicial del proyecto.

<!-- [POR COMPLETAR] Agrega enlaces a otros documentos a medida que se generen. -->

## Decisiones arquitectónicas (ADR)

Las decisiones arquitectónicas se documentan mediante archivos **ADR** (Architecture
Decision Records) en la carpeta [`decisiones/`](./decisiones). Cada ADR registra el
contexto, la decisión tomada y sus consecuencias.

## Equipo

<!-- [POR COMPLETAR] Lista a los integrantes del equipo. -->

| Nombre              |         Contacto                |
|---------------------|---------------------------------|
| María Vallejos      | <mvallejosr@ucenfotec.ac.cr>    |
| Lisdiana Rodriguez  | <lrodriguezal@ucenfotec.ac.cr>  |
| Roberto del Cid     | <rdelcidw@ucenfotec.ac.cr>      |

## Licencia

[POR DEFINIR] 

---

*Proyecto desarrollado para el curso PSWE04 — Ciclo C2 2026.*
