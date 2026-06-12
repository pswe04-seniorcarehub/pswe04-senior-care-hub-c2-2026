# SeniorCareHub
> Documento de Diseño de Software — PSWE-04  
> Universidad Cenfotec · Maestría Profesional en Ingeniería del Software

---

| Campo | Detalle |
|---|---|
| **Nombre del sistema** | SeniorCareHub – Plataforma Inteligente de Monitoreo y Asistencia para Adultos Mayores |
| **Grupo** | 3 |
| **Integrantes** | Roberto Obed Del Cid Winter — P000024239, Lisdiana Mercedes Rodriguez Alvarado — P000030183, Maria Isabel Vallejos Rodriguez — P000020526|
| **URL del repositorio** | https://github.com/pswe04-seniorcarehub/pswe04-senior-care-hub-c2-2026.git |
| **Docente** | Juan Mauricio Leandro Jimenez |
| **Cuatrimestre** | 2026 — II Cuatrimestre |
| **Versión del documento** | 0.1 — Propuesta inicial |
| **Fecha de última actualización** | 2026-05-26 |

---

## Historial de versiones

| Versión | Fecha | Hito | Cambios principales | Autor(es) |
|---|---|---|---|---|
| 0.1 | 2026-05-26 | Propuesta (S03) | Creación del documento inicial | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |

---

## Tabla de contenidos

1. [Descripción del sistema y alcance](#1-descripción-del-sistema-y-alcance)
2. [Stakeholders](#2-stakeholders)

---

# BLOQUE 1 — CONTEXTO Y PROBLEMA
*Hito: Propuesta (S03)*

---

## 1. Descripción del sistema y alcance

### 1.1 Descripción general

Los adultos mayores que viven solos o requieren supervisión parcial pueden enfrentar situaciones de riesgo como caídas, períodos prolongados de inactividad o desorientación. En muchos casos, la detección tardía de estos eventos limita la capacidad de respuesta de familiares y cuidadores.

La propuesta busca abordar este problema mediante una plataforma distribuida de monitoreo que permita detectar oportunamente eventos críticos y generar alertas automáticas para mejorar la atención y el seguimiento de las personas adultas mayores.

### 1.2 Contexto del negocio o dominio

El envejecimiento de la población representa uno de los principales desafíos sociales y tecnológicos de la actualidad.

Actualmente existen dispositivos wearables capaces de capturar información relacionada con movimiento, actividad física y ubicación. Sin embargo, la utilidad de estos dispositivos depende de la capacidad de procesar los datos recibidos y generar alertas oportunas para familiares o cuidadores.

SeniorCareHub se ubica dentro del dominio de monitoreo asistido para adultos mayores y sistemas IoT orientados al bienestar. El sistema permite centralizar eventos provenientes de dispositivos inteligentes, aplicar reglas configurables para identificar situaciones de riesgo y distribuir notificaciones a través de diferentes canales.

### 1.3 Alcance del sistema

**Dentro del alcance — el sistema HACE:**

- Recepción de eventos desde un wearable simulado.
- Procesamiento y detección de eventos críticos mediante reglas configurables.
- Dashboard de monitoreo para seguimiento de usuarios.
- Generación y envío de alertas multicanal.

**Fuera del alcance — el sistema NO HACE:**

- Diagnóstico médico.
- Desarrollo de hardware físico real.
- Integración con sistemas hospitalarios o expedientes clínicos.

### 1.4 Usuarios y casos de uso principales

| Tipo de usuario | Casos de uso principales |
|---|---|
| Adulto Mayor | Registrar dispositivo wearable, consultar estado general, visualizar historial de actividad |
| Familiar | Recibir alertas, consultar ubicación y estado, revisar historial de eventos |
| Cuidador Profesional | Monitorear múltiples adultos mayores, gestionar incidentes, registrar seguimiento |
| Administrador del Sistema | Configurar reglas, gestionar usuarios, administrar canales de notificación y monitorear la operación del sistema |

---

## 2. Stakeholders

| Stakeholder | Rol | Intereses principales | Preocupaciones o restricciones |
|---|---|---|---|
| Adulto Mayor | Usuario principal | Seguridad, autonomía y monitoreo continuo | Privacidad y falsas alarmas |
| Familiar | Receptor de alertas | Recibir alertas oportunas y confiables | Retrasos o pérdida de notificaciones |
| Cuidador Profesional | Supervisor operativo | Monitoreo eficiente de múltiples usuarios | Sobrecarga de alertas |
| Administrador del Sistema | Operación de la plataforma | Disponibilidad y gestión eficiente | Fallos operativos |

---

*Documento generado bajo el template estándar PSWE-04 — Universidad Cenfotec — Maestría Profesional en Ingeniería del Software*
