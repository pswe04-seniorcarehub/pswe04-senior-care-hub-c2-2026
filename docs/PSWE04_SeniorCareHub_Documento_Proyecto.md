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
| **Versión del documento** | 0.2 — Avance 1(S07) |
| **Fecha de última actualización** | 2026-06-22 |

---

## Historial de versiones

| Versión | Fecha | Hito | Cambios principales | Autor(es) |
|---|---|---|---|---|
| 0.1 | 2026-05-26 | Propuesta (S03) | Creación del documento inicial | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |
| 0.2 | 2026-06-22 | Avance 1 (S07) | Desarrollo del contexto del sistema, alcance, usuarios, stakeholders, drivers arquitectónicos, escenarios de calidad y vista de contexto C4. | Roberto Obed Del Cid Winter — P000024239, Lisdiana Mercedes Rodriguez Alvarado — P000030183, Maria Isabel Vallejos Rodriguez — P000020526 |

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

Los adultos mayores que viven solos o requieren supervisión parcial pueden enfrentar situaciones de riesgo como caídas, períodos prolongados de inactividad o desorientación. En muchos casos, la detección tardía de estas situaciones limita la capacidad de respuesta de familiares y cuidadores, incrementando el riesgo de consecuencias graves para la salud y seguridad de la persona.

SeniorCareHub es una plataforma inteligente de monitoreo asistido orientada al cuidado de adultos mayores. Su propósito es apoyar a familiares y cuidadores mediante la identificación oportuna de situaciones de riesgo y la generación de alertas que permitan una respuesta rápida ante posibles emergencias. Cuando ocurre una situación de riesgo, el sistema debe ser capaz de identificar el evento, evaluarlo según las condiciones definidas para cada perfil de monitoreo y comunicar oportunamente la información relevante a familiares o cuidadores. De esta manera, SeniorCareHub busca detectar eventos críticos, clasificarlos y entregar alertas oportunas y confiables, contribuyendo a la seguridad y autonomía de las personas monitoreadas.

La necesidad de una solución de software surge porque la supervisión permanente por parte de familiares o cuidadores no siempre es posible. Un sistema automatizado permite centralizar el monitoreo, detectar eventos relevantes de forma continua y comunicar oportunamente situaciones que requieren atención, reduciendo los tiempos de respuesta y mejorando la capacidad de reacción ante incidentes.

### 1.2 Contexto del negocio o dominio

SeniorCareHub se ubica dentro del dominio de monitoreo asistido para adultos mayores, un área orientada a mejorar la seguridad y calidad de vida de personas que viven solas o requieren supervisión parcial. En este contexto, familiares y cuidadores necesitan mantenerse informados sobre situaciones que puedan representar un riesgo para la integridad o bienestar de la persona monitoreada, como caídas, periodos prolongados de inactividad, desplazamientos fuera de áreas seguras u otros eventos que requieran atención.

En muchos casos, el monitoreo de adultos mayores depende de llamadas periódicas, visitas presenciales o mecanismos de supervisión limitados por parte de familiares y cuidadores. Estos procesos pueden resultar insuficientes para detectar oportunamente situaciones de emergencia, especialmente cuando no existe supervisión continua. El sistema busca apoyar este proceso mediante una plataforma inteligente capaz de recibir, analizar y evaluar eventos asociados al monitoreo de cada adulto mayor. En este contexto, el término inteligente se refiere a la capacidad de interpretar los eventos recibidos, aplicar reglas configurables y determinar cuándo una situación requiere atención. Esto permite adaptar los criterios de detección y clasificación de riesgos según las características y necesidades de cada persona monitoreada.

Para los fines de este proyecto, no se contempla el uso de dispositivos wearable reales. En su lugar, se utilizarán simulaciones que representen los eventos que dichos dispositivos podrían generar en un entorno operativo real. Entre los eventos considerados se incluyen caídas, periodos prolongados de inactividad, desplazamientos fuera de zonas seguras y pérdida de comunicación. Cada evento simulado incluirá información básica como identificación del usuario monitoreado, tipo de evento, fecha y hora de ocurrencia, nivel de criticidad estimado y, cuando corresponda, ubicación aproximada.

La naturaleza del dominio exige que los eventos críticos sean identificados y comunicados oportunamente, y que los mecanismos de notificación mantengan altos niveles de disponibilidad y confiabilidad para apoyar la atención de emergencias. Desde la perspectiva del dominio, el sistema debe manejar información personal y sensible relacionada con los usuarios monitoreados, incluyendo datos de actividad, estado general y posibles datos de ubicación. Por esta razón, la privacidad y la protección de la información constituyen consideraciones relevantes para el diseño de la solución. 

### 1.3 Alcance del sistema

**Dentro del alcance — el sistema HACE:**

- Recibir y procesar eventos simulados que representan la información generada por dispositivos wearable asociados a adultos mayores monitoreados.
- Detectar y clasificar situaciones de riesgo, como caídas, periodos prolongados de inactividad, desplazamientos fuera de áreas seguras o pérdida de comunicación con el dispositivo.
- Permitir configurar perfiles individuales de monitoreo, incluyendo reglas de detección, criterios de criticidad y mecanismos de notificación para cada adulto mayor.
- Generar y enviar alertas a familiares y cuidadores, así como notificaciones relevantes al adulto mayor cuando corresponda, a través de múltiples canales de comunicación.
- Aplicar reglas configurables para determinar la criticidad de los eventos y priorizar las alertas generadas.
- Permitir el monitoreo del estado de los adultos mayores mediante paneles de seguimiento e historial de eventos y alertas.
- Mantener un registro histórico de eventos, alertas y acciones relevantes para consulta y seguimiento.
- Gestionar usuarios, roles y permisos diferenciados para adultos mayores, familiares, cuidadores y administradores, garantizando que cada tipo de usuario acceda únicamente a la información y funcionalidades correspondientes a sus responsabilidades.
- Permitir registrar y dar seguimiento al estado de las alertas generadas, incluyendo su atención, confirmación o descarte en casos de posibles falsas alarmas.

**Fuera del alcance — el sistema NO HACE:**

- Realizar diagnósticos médicos, emitir recomendaciones clínicas o sustituir la valoración de profesionales de salud.
- Contactar directamente servicios de emergencia, ambulancias, hospitales o cuerpos de rescate. La responsabilidad del sistema se limita a la detección de eventos y generación de alertas para familiares y cuidadores.
- Desarrollar o administrar hardware físico real; los dispositivos wearable se consideran componentes externos al sistema.
- Integrarse con sistemas hospitalarios, expedientes clínicos electrónicos o plataformas médicas institucionales.
- Monitorear signos vitales médicos especializados ni interpretar información clínica compleja.
- Garantizar la respuesta o intervención de familiares, cuidadores o terceros una vez enviada una alerta. La responsabilidad del sistema se limita a la detección de eventos, clasificación de riesgos y generación de las notificaciones correspondientes.
- Garantizar que toda alerta generada corresponda a una emergencia real. El sistema clasifica y prioriza eventos según reglas configurables, pero la confirmación final depende de la revisión humana.
- Sustituir la supervisión humana o el cuidado presencial de los adultos mayores.

### 1.4 Usuarios y casos de uso principales

| Tipo de usuario | Casos de uso principales |
|---|---|
| Adulto Mayor | Consultar el estado de su monitoreo y del dispositivo wearable, visualizar historial de eventos y alertas asociadas a su perfil, recibir notificaciones relevantes relacionadas con su monitoreo |
| Familiar | Recibir alertas de emergencia en tiempo real, consultar el estado actual del adulto mayor y del dispositivo wearable, visualizar historial de eventos y alertas, configurar preferencias de notificación, confirmar o dar seguimiento a alertas recibidas |
| Cuidador Profesional | Monitorear múltiples adultos mayores, recibir, visualizar y priorizar alertas activas, consultar historial de eventos, registrar seguimiento de incidentes y posibles falsas alarmas, ajustar perfiles de monitoreo de adultos mayores bajo su supervisión |
| Administrador del Sistema | Administrar usuarios, roles y permisos, configurar reglas y parámetros generales de monitoreo, administrar canales de notificación, supervisar la operación del sistema, consultar métricas y eventos de la plataforma |

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
