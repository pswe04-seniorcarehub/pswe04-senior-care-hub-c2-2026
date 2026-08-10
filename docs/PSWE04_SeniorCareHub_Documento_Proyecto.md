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
| **Versión del documento** | 0.3 — Avance 2(S11) |
| **Fecha de última actualización** | 2026-07-26 |

---

## Historial de versiones

| Versión | Fecha | Hito | Cambios principales | Autor(es) |
|---|---|---|---|---|
| 0.1 | 2026-05-26 | Propuesta (S03) | Creación del documento inicial | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |
| 0.2 | 2026-06-24 | Avance 1 (S07) | Desarrollo del contexto del sistema, alcance, usuarios, stakeholders, drivers arquitectónicos, escenarios de calidad y vista de contexto C4. | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |
| 0.3 | 2026-07-26 | Avance 2 (S11) | Incorporación de la vista de contenedores, estilo arquitectónico, análisis de alternativas, trade-offs, ADRs y primer diseño detallado de componentes. | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |

---

## Tabla de contenidos

1. [Descripción del sistema y alcance](#1-descripción-del-sistema-y-alcance)
2. [Stakeholders](#2-stakeholders)
3. [Drivers arquitectónicos](#3-drivers-arquitectónicos)
4. [Requerimientos de calidad — Escenarios](#4-requerimientos-de-calidad--escenarios)
5. [Restricciones](#5-restricciones)
6. [Principios de diseño adoptados](#6-principios-de-diseño-adoptados)
7. [Vistas arquitectónicas](#7-vistas-arquitectónicas)
   - 7.1 [Vista de contexto](#71-vista-de-contexto)
   - 7.2 [Vista de contenedores](#72-vista-de-contenedores)
   - 7.3 [Vista de comportamiento](#73-vista-de-comportamiento)
   - 7.4 [Vista de despliegue](#74-vista-de-despliegue)
   - 7.5 [Vista de concurrencia](#75-vista-de-concurrencia)
   - 7.6 [Evolución de las vistas arquitectónicas](#76-evolución-de-las-vistas-arquitectónicas)
8. [Estilo arquitectónico](#8-estilo-arquitectónico)
9. [Registro de decisiones — ADRs](#9-registro-de-decisiones--adrs)
10. [Diseño detallado de componentes](#10-diseño-detallado-de-componentes)
14. [Asuntos clave de diseño](#14-asuntos-clave-de-diseño)
   - 14.1 [Sistemas distribuidos y computación en la nube](#141-sistemas-distribuidos-y-computación-en-la-nube)
   - 14.2 [Sistemas concurrentes y de tiempo real](#142-sistemas-concurrentes-y-de-tiempo-real)
---

# BLOQUE 1 — CONTEXTO Y PROBLEMA
*Hito: Propuesta (S03) y Avance 1 (S07)*

---

## 1. Descripción del sistema y alcance

### 1.1 Descripción general

Los adultos mayores que viven solos o requieren supervisión parcial pueden enfrentar situaciones de riesgo como caídas, períodos prolongados de inactividad o desorientación. En muchos casos, la detección tardía de estas situaciones limita la capacidad de respuesta de familiares y cuidadores, incrementando el riesgo de consecuencias graves para la salud y seguridad de la persona.

SeniorCareHub es una plataforma inteligente de monitoreo asistido orientada al cuidado de adultos mayores. Su propósito es apoyar a familiares y cuidadores mediante la identificación oportuna de situaciones de riesgo y la generación de alertas que permitan una respuesta rápida ante posibles emergencias. El principal desafío del sistema consiste en recibir de manera continua los eventos generados durante el monitoreo, evaluarlos mediante reglas configurables, distinguir eventos críticos de posibles falsas alarmas, clasificar su nivel de criticidad y generar alertas oportunas, confiables y resilientes, preservando la privacidad y la protección de los datos de las personas monitoreadas. De esta manera, SeniorCareHub contribuye a mejorar la seguridad y autonomía de los adultos mayores, facilitando una respuesta más rápida y efectiva por parte de familiares y cuidadores cuando ocurre una situación de riesgo.

Para cumplir este objetivo, el sistema debe operar de forma continua, procesando los eventos recibidos y generando alertas oportunas incluso durante picos de carga de trabajo o fallos parciales, sin comprometer la privacidad y protección de la información sensible de las personas monitoreadas. Esto permite reducir los tiempos de respuesta ante posibles emergencias y brindar un apoyo efectivo a familiares y cuidadores durante el proceso de monitoreo.


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

El análisis de stakeholders identifica a las personas y entidades que influyen en SeniorCareHub o son afectadas por él. Para cada uno se precisa su tipo, sus intereses y expectativas, sus preocupaciones, y los drivers arquitectónicos que origina (sección 3), de modo que la caracterización sea concreta y trazable hacia las decisiones de diseño.

**Clasificación.** *Primario:* interactúa directamente con el sistema. *Secundario:* interesado o afectado, sin operarlo directamente. *Interno:* parte de la organización que opera o construye el sistema. *Externo:* ajeno a esa organización.

| Stakeholder | Tipo | Intereses y expectativas | Preocupaciones / restricciones | Drivers que origina (§3) |
|---|---|---|---|---|
| Adulto Mayor | Primario · externo (usuario final monitoreado) | Seguridad y atención oportuna ante una emergencia; conservar su autonomía e independencia; monitoreo continuo y poco intrusivo; control sobre su información personal y posibilidad de consultar su estado. | Privacidad de sus datos de ubicación y salud; falsas alarmas que generen molestias o intervenciones innecesarias; sensación de vigilancia constante. | QA-04, QA-01, QA-02, RF-01, RF-03, RF-05 |
| Familiar | Primario · externo (receptor de alertas) | Recibir alertas oportunas y confiables; conocer el estado y el historial del adulto mayor; tranquilidad de saber que será avisado ante un evento crítico. | Retrasos o pérdida de notificaciones; no enterarse a tiempo de una emergencia; recibir información poco clara. | RF-03, RF-04, QA-02, QA-03, QA-01 |
| Cuidador Profesional | Primario · externo (supervisor operativo) | Monitorear eficientemente a varios adultos mayores; gestionar incidentes y dar seguimiento; definir criterios de detección y criticidad por persona mediante perfiles. | Sobrecarga de alertas (fatiga de alarmas) y falsos positivos; perder eventos críticos entre muchos usuarios; dificultad para ajustar reglas. | RF-02, RF-04, RF-05, QA-02, QA-03, QA-05 |
| Administrador del Sistema | Primario · interno (operación de la plataforma) | Operación estable y disponible de la plataforma; gestionar reglas, perfiles, usuarios y canales de notificación; administrar la seguridad y los accesos. | Fallos operativos e indisponibilidad; complejidad para cambiar reglas o canales sin redesplegar; brechas de seguridad o accesos indebidos. | QA-01, QA-05, QA-04, RF-02, RF-05, REST-01 |
| Equipo de desarrollo y mantenimiento | Secundario · interno (construye y mantiene el sistema) | Una arquitectura clara y modificable que permita evolucionar reglas, perfiles y canales sin reescritura; facilidad de prueba y despliegue. | Deuda técnica y acoplamiento; complejidad para incorporar cambios sin afectar lo existente. | QA-05, RF-02, RF-05 |
| Autoridad de protección de datos (PRODHAB) | Secundario · externo (ente regulador) | Cumplimiento de la Ley 8968 sobre el tratamiento de datos personales sensibles de personas vulnerables. | Protección, minimización y trazabilidad de los datos; uso indebido de información sensible. | QA-04, REST-02 |

---

## 3. Drivers arquitectónicos

Los drivers son los factores que más moldean la arquitectura de SeniorCareHub. Se clasifican en tres categorías: requerimientos funcionales con impacto estructural, atributos de calidad prioritarios y restricciones no negociables. Cada driver referencia al stakeholder que lo origina (sección 2). Las justificaciones se centran en *por qué* cada elemento condiciona la arquitectura, sin comprometer todavía una tecnología ni un patrón de solución (eso corresponde a las vistas y decisiones posteriores).

### 3.1 Requerimientos funcionales clave

| ID | Requerimiento | Stakeholder | Por qué es un driver (impacto arquitectónico) |
|---|---|---|---|
| RF-01 | El sistema recibe un flujo continuo de eventos (movimiento, inactividad, ubicación) desde el wearable de cada adulto mayor. | Adulto Mayor, Familiar, Cuidador Profesional | El ingreso continuo y permanente de eventos obliga a que la arquitectura acepte y conserve la entrada con independencia del ritmo al que se procesa, tolerando variaciones de carga y fallos momentáneos sin perder eventos. Esto separa la responsabilidad de recibir los datos de la de analizarlos y convierte la ingesta en una preocupación estructural propia, además de exigir que la arquitectura no quede atada al dispositivo concreto que los emite. |
| RF-02 | El sistema determina si un evento es crítico aplicando reglas configurables (umbrales de inactividad, zonas, etc.) que el administrador modifica sin intervención del equipo de desarrollo. | Administrador del Sistema, Cuidador Profesional | Que las reglas cambien sin tocar el código obliga a que el comportamiento de detección no esté fijo, sino determinado por configuración que la arquitectura debe poder leer y aplicar en tiempo de ejecución. Esto separa la política (qué se considera crítico) del mecanismo (cómo se evalúa) y condiciona dónde reside y cómo se gobierna esa configuración dentro de la estructura. |
| RF-03 | Ante un evento crítico de un adulto mayor, el sistema notifica a sus familiares y cuidadores a través de múltiples canales externos (correo, SMS, mensajería). | Adulto Mayor, Familiar, Cuidador Profesional | Notificar por varios canales que pueden cambiar (agregarse o quitarse) y que dependen de servicios externos no controlados obliga a que la lógica de detección no quede acoplada a un canal concreto y a que la arquitectura aísle esas dependencias para que sus fallos o cambios no se propaguen al núcleo. La entrega de la alerta se vuelve una preocupación estructural, separada de la decisión de cuándo alertar. |
| RF-04 | Cuidadores y familiares consultan estado e historial en el dashboard; un mismo cuidador supervisa a varios adultos mayores. | Cuidador Profesional, Familiar | La relación muchos-a-muchos (un cuidador con varios adultos mayores; un adulto mayor con varios familiares) condiciona el modelo de datos, el acceso a la información histórica y la autorización basada en la relación entre actores (quién puede ver a quién). |
| RF-05 | El sistema permite definir y administrar perfiles de monitoreo individuales para cada adulto mayor, incluyendo criterios de detección, niveles de criticidad y destinatarios de las notificaciones. | Administrador del Sistema, Cuidador Profesional | Que cada adulto mayor tenga su propia configuración (criterios, criticidad y destinatarios) impide que la detección y la notificación se comporten de forma única y global: la arquitectura debe sostener comportamiento parametrizado por sujeto, que los caminos de detección y notificación leen y aplican. Esto condiciona el modelo de datos (relación entre perfiles, adultos mayores y destinatarios) y exige que la configuración sea una preocupación de primer nivel, desacoplada de la lógica para poder cambiar sin reescritura. |

### 3.2 Atributos de calidad prioritarios

| ID | Atributo | Importancia | Stakeholder | Justificación (meta medible y porqué) |
|---|---|---|---|---|
| QA-01 | Disponibilidad | Alta | Adulto Mayor, Familiar, Cuidador, Administrador | **Meta:** pipeline crítico (ingesta → detección → notificación) disponible ≥ 99.5 % mensual (≈ ≤ 3.6 h de caída/mes). Si el sistema está caído, no llegan alertas, no se puede monitorear ni consultar el estado; la seguridad del adulto mayor depende de que el monitoreo no se interrumpa. |
| QA-02 | Rendimiento (latencia de alerta) | Alta | Adulto Mayor, Familiar, Cuidador | **Meta:** desde la recepción de un evento crítico hasta el despacho de la notificación ≤ 5 s en el percentil 95, bajo carga normal. Una alerta tardía pierde su valor; la latencia afecta a todos los implicados en recibir la alerta y actuar a tiempo. |
| QA-03 | Tolerancia a fallos / Resiliencia | Alta | Adulto Mayor, Familiar, Cuidador, Administrador | **Meta:** 0 alertas críticas perdidas; entrega "al menos una vez" con reintentos; ante la caída de un canal externo o de un componente no crítico, el sistema sigue operando y notificando (degradación sin pérdida). Perder una alerta crítica es el peor fallo posible. |
| QA-04 | Seguridad y privacidad | Alta | Adulto Mayor, Familiar, Cuidador, Administrador | **Meta:** 100 % de los datos sensibles (ubicación, identidad) cifrados en tránsito y en reposo; acceso por rol y por relación; alineado a la Ley 8968 (CR). Se maneja ubicación y estado de personas vulnerables: todos los actores tienen interés en su protección y existe marco regulatorio aplicable. |
| QA-05 | Modificabilidad | Alta | Administrador, Cuidador | **Meta:** cambiar un umbral, un perfil de monitoreo o un canal de notificación no requiere recompilar ni redesplegar el núcleo; el comportamiento de detección y notificación se ajusta mediante configuración. Las reglas, los perfiles (RF-02, RF-05) y los canales son lo que más cambiará en el tiempo; el diseño debe absorber ese cambio sin reescritura. |

**Selección — por qué estos cinco:** se priorizan los atributos críticos para la operación del sistema y presentes en el alcance: disponibilidad, rendimiento (latencia de alerta), tolerancia a fallos/resiliencia, seguridad/privacidad y modificabilidad (clave por las reglas y perfiles configurables, ligada a RF-02 y RF-05). La **exactitud de la detección / control de falsas alarmas** no se trata como atributo prioritario independiente, sino como parte del diseño de las reglas de detección, y se refleja como una tensión con el rendimiento (ver abajo). Se excluye la **escalabilidad** del top-5 porque el proyecto es de diseño con una base de usuarios acotada; queda como punto de extensión habilitado por la independencia entre ingesta y procesamiento (RF-01), sin meta de escala fija. Se excluye la **usabilidad** porque, aunque es relevante para el dashboard, no impone decisiones estructurales mayores.

**Tensiones entre atributos** (insumo directo para los escenarios de calidad de la sección 4):

- **Rendimiento ↔ Tolerancia a fallos:** entregar la alerta lo más rápido posible vs. garantizar que no se pierda; las confirmaciones y los reintentos que dan robustez agregan latencia.
- **Exactitud de la detección (control de falsas alarmas) ↔ Rendimiento:** confirmar un evento antes de alertar reduce los falsos positivos pero agrega retraso; es el equilibrio entre no generar falsas alarmas y avisar a tiempo.
- **Modificabilidad ↔ Rendimiento:** la flexibilidad de evaluar reglas configurables y parametrizadas en tiempo de ejecución compite con la velocidad de procesamiento frente a una lógica fija.
- **Disponibilidad ↔ Seguridad:** operar de forma continua vs. los controles, validaciones y ventanas de mantenimiento que añaden latencia o indisponibilidad.

> La tensión Disponibilidad ↔ Consistencia (CAP) se retoma más adelante, en la etapa de diseño del almacenamiento distribuido, donde corresponde tomar esa decisión.

### 3.3 Restricciones que actúan como drivers

| ID | Restricción | Tipo | Impacto en el diseño |
|---|---|---|---|
| REST-01 | La notificación depende de servicios de terceros (correo, SMS, mensajería) fuera del control del equipo. | Técnica | La arquitectura debe aislar estas dependencias externas para que su indisponibilidad, latencia o cambios no se propaguen al núcleo, y mantener la entrega de alertas resiliente pese a que esos servicios no son gobernables por el sistema. Es la cara de "restricción" de RF-03 y sustenta la meta de QA-03. |
| REST-02 | Cumplimiento de la Ley 8968 de protección de datos personales (Costa Rica), por manejar ubicación e identidad de personas vulnerables. | Regulatoria | Obliga a tratar la seguridad y la privacidad como una preocupación transversal: protección de los datos sensibles en tránsito y en reposo, minimización, acceso por rol y por relación, y trazabilidad de los accesos. Sustenta la meta de QA-04. |

*Referencia normativa (para la sección de Referencias del documento):* Asamblea Legislativa de la República de Costa Rica, "Ley N.° 8968: Protección de la Persona frente al Tratamiento de sus Datos Personales," *La Gaceta*, n.° 170, San José, Costa Rica, 5 de setiembre de 2011.

---

# BLOQUE 2 — REQUERIMIENTOS DE CALIDAD
*Hito: Avance 1 (S07)*

---

## 4. Requerimientos de calidad — Escenarios

Un escenario de calidad es una descripción concreta y medible de cómo el sistema debe responder ante un estímulo específico. El formato ISO/IEEE de 6 elementos es muy utilizado para representar estos escenarios ya que muestra como se desenvuelve el sistema ante diferentes situaciones.

Para los fines de este proyecto se identificaron 5 atributos de calidad y sus respectivas tensiones cuando corresponden.

### Escenario QS-01 — Disponibilidad

| Elemento | Descripción |
|-----------|-------------|
| **Fuente del estímulo** | Familiar o cuidador |
| **Estímulo** | Eventos continuos de monitoreo y generación de alertas|
| **Entorno** | Operación continua durante un mes de funcionamiento normal |
| **Artefacto** | Plataforma SeniorCareHub (pipeline de monitoreo y dashboard)|
| **Respuesta** | El sistema permite acceder al estado y al historial sin interrupciones significativas|
| **Medida de respuesta** | El pipeline crítico de monitoreo y consulta mantiene una disponibilidad mensual ≥ 99.5 %, equivalente a un tiempo máximo de indisponibilidad de 3.6 horas por mes |

**Tensión con:** QS-04 (Seguridad y privacidad), porque mecanismos de autenticación, auditoría y mantenimiento pueden introducir indisponibilidad temporal.

### Escenario QS-02 — Rendimiento

| Elemento | Descripción |
|-----------|-------------|
| **Fuente del estímulo** | Wearable simulado asociado a un adulto mayor |
| **Estímulo** | Emite un evento de caída o inactividad que cumple los criterios configurados para ser evaluado como crítico |
| **Entorno** | Operación normal con hasta 1 000 adultos mayores monitoreados, una carga sostenida de 20 eventos por segundo y picos de hasta 100 eventos por segundo durante 5 minutos, los proveedores externos se encuentran disponibles |
| **Artefacto** | Servicio de Ingesta, Bus de Mensajería, Motor de Reglas y Servicio de Notificaciones |
| **Respuesta** | El sistema valida y conserva el evento, evalúa las reglas del perfil, genera la alerta y despacha la primera notificación al canal de mayor prioridad |
| **Medida de respuesta** | El tiempo entre la aceptación durable del evento y la aceptación de la solicitud por el primer proveedor de notificación es ≤ 5 segundos en el percentil 95 y ≤ 8 segundos en el percentil 99 |

**Tensión con:** QS-03 (Resiliencia), debido a que reintentos y mecanismos de recuperación incrementan la latencia. También tensiona con QS-05 (Modificabilidad), porque reglas más flexibles pueden aumentar el tiempo de procesamiento y QS-05, porque la evaluación dinámica de reglas puede requerir más procesamiento que una lógica fija.

### Escenario QS-03 — Tolerancia a fallos / Resiliencia

| Elemento | Descripción |
|-----------|-------------|
| **Fuente del estímulo** | Proveedor externo de notificaciones o fallo de una instancia interna |
| **Estímulo** | El proveedor principal de SMS deja de responder o una instancia del Servicio de Notificaciones se reinicia durante el procesamiento |
| **Entorno** | Operación con eventos críticos en tránsito, el perfil tiene al menos un canal alternativo configurado y el sistema procesa la carga de referencia |
| **Artefacto** | Bus de Mensajería, Motor de Reglas, Servicio de Notificaciones y almacenes durables |
| **Respuesta** | El sistema conserva la alerta, reintenta el procesamiento de manera idempotente, utiliza un canal alternativo cuando esté disponible y registra el fallo y el resultado de cada intento |
| **Medida de respuesta** | Durante una prueba de inyección de fallos con al menos 1000 eventos críticos aceptados, el 100% debe quedar asociado a un estado durable y trazable: Delivered, PendingRetry, FallbackInProgress o DeadLetter. Deben existir 0 alertas sin correspondencia entre eventId, alertId y sus intentos de notificación. El primer intento por un canal alternativo debe comenzar en menos de 10 segundos en el percentil 95 |

**Tensión con:** QS-02 (Rendimiento), porque los mecanismos de recuperación y reintentos agregan tiempo adicional.

### Escenario QS-04 — Seguridad y privacidad

| Elemento | Descripción |
|-----------|-------------|
| **Fuente del estímulo** | Usuario autenticado sin relación autorizada con el adulto mayor, usuario con un rol insuficiente o cliente con credenciales inválidas |
| **Estímulo** | Intenta consultar o modificar ubicación, estado, historial, perfil de monitoreo o alertas de un adulto mayor para el cual no posee autorización |
| **Entorno** | Operación normal |
| **Artefacto** | API de Aplicación, subsistema de autenticación y autorización y bitácora de auditoría |
| **Respuesta** | El sistema bloquea el acceso, se registra el intento y mantiene protegidos los datos sensibles |
| **Medida de respuesta** | El 100 % de los casos incluidos en la suite de pruebas de autorización recibe una respuesta HTTP 401 o 403, según corresponda. Cada intento genera un registro consultable en menos de 5 segundos que contiene fecha y hora, identificador del actor, recurso objetivo, acción solicitada, decisión de autorización, motivo, dirección de origen y correlationId, sin almacenar el contenido sensible consultado |

**Tensión con:** QS-01 (Disponibilidad) y QS-02 (Rendimiento), porque mecanismos de seguridad, auditoría y mantenimiento pueden impactar la continuidad del servicio.

### Escenario QS-05 — Modificabilidad

| Elemento | Descripción |
|-----------|-------------|
| **Fuente del estímulo** | Administrador del sistema o cuidador autorizado |
| **Estímulo** | Modifica los parámetros de una regla previamente soportada, como el tiempo máximo de inactividad, el radio de una zona segura, el nivel de criticidad, la ventana de confirmación o los canales y destinatarios habilitados |
| **Entorno** | Operación normal con usuarios y dispositivos simulados activos |
| **Artefacto** | API de Aplicación, configuración de perfiles, BD Operativa y Motor de Reglas |
| **Respuesta** | El sistema valida la configuración, crea una nueva versión del perfil, registra quién realizó el cambio y hace que los eventos posteriores sean evaluados utilizando la versión actualizada |
| **Medida de respuesta** | La nueva configuración está disponible para evaluación en menos de 10 minutos, sin recompilar ni redesplegar el Motor de Reglas y sin interrumpir la recepción de eventos. El siguiente evento procesado para ese perfil permite verificar mediante auditoría qué versión de la regla fue aplicada |

**Tensión con:** QS-02 (Rendimiento), debido a que una mayor flexibilidad y configurabilidad puede incrementar el tiempo requerido para evaluar eventos y determinar su criticidad y QS-04 (Seguridad y privacidad), porque el versionado y la auditoría agregan almacenamiento y controles.

---

## 5. Restricciones

| ID | Restricción | Tipo | Origen | Impacto en el diseño |
|---|---|---|---|---|
| REST-01 | La entrega de alertas depende de servicios externos de notificación (correo electrónico, SMS y mensajería) que están fuera del control del sistema. | Técnica            | Dominio del problema y proveedores externos | La arquitectura debe aislar estas dependencias y tolerar fallos, indisponibilidad o cambios en los proveedores sin comprometer la generación y gestión de alertas. |
| REST-02 | El sistema debe cumplir con la Ley N.° 8968 de Protección de la Persona frente al Tratamiento de sus Datos Personales de Costa Rica.                | Regulatoria        | Marco legal costarricense                   | Obliga a proteger datos sensibles como identidad, ubicación y estado de monitoreo mediante mecanismos de control de acceso, auditoría y protección de datos.       |
| REST-03 | Para los fines del proyecto se utilizarán eventos simulados en lugar de dispositivos wearable reales.                                               | Proyecto           | Alcance definido para el proyecto académico | El diseño debe desacoplar la plataforma de dispositivos específicos y permitir que la fuente de eventos sea simulada sin afectar el resto de la arquitectura.      |
| REST-04 | El alcance del proyecto está orientado al diseño arquitectónico de la solución y no a la implementación completa de un sistema productivo.          | Negocio / Proyecto | Curso PSWE-04                               | Las decisiones se enfocan en arquitectura, atributos de calidad, componentes e integración, sin requerir el desarrollo completo de todas las funcionalidades.      |

---

## 6. Principios de diseño adoptados

| Principio | Justificación para este sistema |
|------------|--------------------------------|
| Separación de responsabilidades (Separation of Concerns) | La ingestión de eventos, el motor de reglas, las notificaciones y el dashboard representan responsabilidades distintas que deben evolucionar y fallar de manera independiente. Esto facilita la mantenibilidad y limita el impacto de cambios futuros. |
| Diseño para el cambio (Open-Closed Principle) | Las reglas de detección, perfiles de monitoreo y canales de notificación pueden cambiar con el tiempo. El sistema debe permitir incorporar nuevas reglas o proveedores sin modificar el núcleo del procesamiento. |
| Defensa en profundidad (Defense in Depth) | SeniorCareHub maneja información sensible relacionada con ubicación y estado de personas adultas mayores. Por ello, la seguridad debe implementarse mediante múltiples capas de protección, incluyendo autenticación, autorización, cifrado y auditoría. |
| Diseño para resiliencia (Fail Gracefully) | La indisponibilidad de un proveedor externo de notificaciones no debe impedir el funcionamiento del sistema. El diseño debe tolerar fallos parciales y continuar operando mediante degradación controlada y recuperación ante errores. |
| KISS (Keep It Simple) | El sistema debe mantener una arquitectura comprensible y evitar complejidad innecesaria. Las decisiones arquitectónicas se orientan a satisfacer los atributos de calidad prioritarios sin introducir mecanismos que no respondan a necesidades reales del dominio. |
| Principio de menor privilegio (Principle of Least Privilege) | Cada actor debe acceder únicamente a la información y funcionalidades necesarias para desempeñar sus responsabilidades. Esto reduce riesgos de exposición de datos y fortalece la privacidad de los usuarios monitoreados. |
| Inversión / Aislamiento de dependencias externas (Dependency Inversion) | El núcleo de detección y notificación no depende de implementaciones concretas de sistemas externos. El wearable y los proveedores de notificación (correo, SMS, mensajería) se ubican detrás de abstracciones, de modo que un dispositivo o proveedor pueda sustituirse sin modificar la lógica del sistema. |

---

# BLOQUE 3 — VISTAS ARQUITECTÓNICAS

## 7. Vistas arquitectónicas

### 7.1 Vista de contexto

La vista de contexto (C4 · Nivel 1) ubica a SeniorCareHub como un único sistema dentro de su entorno, mostrando los actores que interactúan con él y los sistemas externos de los que depende, junto con las relaciones etiquetadas entre ellos.

```mermaid
flowchart LR
    adulto["**Adulto Mayor**<br/>[Persona]<br/>Monitoreado mediante wearable;<br/>consulta su estado e historial"]
    admin["**Administrador del Sistema**<br/>[Persona]<br/>Configura reglas,<br/>usuarios y canales"]
    familiar["**Familiar**<br/>[Persona]<br/>Recibe alertas y consulta<br/>estado e historial"]
    cuidador["**Cuidador Profesional**<br/>[Persona]<br/>Monitorea a varios adultos<br/>mayores y gestiona incidentes"]

    sch["**SeniorCareHub**<br/>[Sistema de software]<br/>Recibe eventos del wearable,<br/>detecta situaciones críticas con<br/>reglas configurables y notifica<br/>por múltiples canales"]

    wearable["**Wearable (simulado)**<br/>[Sistema externo]<br/>Emite eventos de movimiento,<br/>inactividad y ubicación"]
    notif["**Servicios de notificación externos**<br/>[Sistema externo]<br/>Correo, SMS y mensajería<br/>que entregan las alertas"]

    adulto -->|"consulta estado de monitoreo y su perfil"| sch
    wearable -->|"emite eventos (asíncrono)"| sch
    admin -->|"configura reglas y canales"| sch
    familiar -->|"consulta estado e historial"| sch
    cuidador -->|"monitorea y gestiona incidentes"| sch
    sch -->|"envía alertas (correo/SMS/mensajería)"| notif
    notif -->|"entrega la alerta"| familiar
    notif -->|"entrega la alerta"| cuidador

    subgraph leyenda ["Leyenda"]
        direction LR
        lp["Persona (actor)"]:::person
        ls["Sistema en alcance"]:::system
        le["Sistema externo"]:::ext
    end

    classDef person fill:#08427B,stroke:#052E56,color:#ffffff;
    classDef system fill:#1168BD,stroke:#0B4884,color:#ffffff;
    classDef ext fill:#8A8A8A,stroke:#5F5F5F,color:#ffffff;

    class adulto,admin,familiar,cuidador person;
    class sch system;
    class wearable,notif ext;
```

*Figura 1 — Vista de contexto (C4 · Nivel 1) de SeniorCareHub*

**Actores:**

- **Adulto Mayor** — persona monitoreada mediante un wearable; consulta su estado de monitoreo, su perfil e historial.
- **Familiar** — recibe alertas y consulta el estado e historial del adulto mayor.
- **Cuidador Profesional** — monitorea a varios adultos mayores y gestiona incidentes.
- **Administrador del Sistema** — configura reglas, perfiles, usuarios y canales de notificación.

**Sistemas externos:**

- **Wearable (simulado)** — emite de forma continua los eventos de movimiento, inactividad y ubicación del adulto mayor hacia el sistema.
- **Servicios de notificación externos** — proveedores de correo, SMS y mensajería que entregan las alertas a familiares y cuidadores.

**Flujo principal:** el wearable emite eventos hacia SeniorCareHub; el sistema los procesa, detecta situaciones críticas según las reglas configuradas y despacha las alertas a través de los servicios de notificación externos, que las entregan a familiares y cuidadores. En paralelo, el adulto mayor, el familiar y el cuidador consultan el estado e historial directamente en el sistema, y el administrador gestiona reglas, perfiles y canales.

> El diagrama anterior lo renderiza GitHub a partir del bloque Mermaid embebido. La versión formal en notación e iconografía C4 (C4-PlantUML) está disponible en `diagramas/c4-contexto.puml`.

### 7.2 Vista de contenedores

Esta vista abre la caja negra de SeniorCareHub presentada en §7.1 y muestra las unidades desplegables que la componen, la tecnología de cada una y los protocolos de comunicación entre ellas. Los actores y los sistemas externos son exactamente los mismos de la vista de contexto: no se introduce ningún elemento externo nuevo.

```mermaid
flowchart TB
    %% ---------- Actores ----------
    AM["Adulto Mayor"]
    FA["Familiar"]
    CU["Cuidador Profesional"]
    AD["Administrador"]

    %% ---------- Sistemas externos ----------
    WE["Wearable simulado<br/><i>sistema externo</i>"]
    NO["Servicios de notificacion<br/><i>sistema externo</i>"]

    %% ---------- Contenedores ----------
    subgraph SCH["SeniorCareHub"]
        direction TB
        C1["App Web<br/>React + TypeScript"]
        C2["API de Aplicacion<br/>ASP.NET Core"]
        C3["Servicio de Ingesta<br/>ASP.NET Core"]
        C4["Bus de Mensajeria<br/>Azure Service Bus"]
        C5["Motor de Reglas<br/>.NET Worker"]
        C6["Servicio de Notificaciones<br/>.NET Worker"]
        C7[("BD Operativa<br/>PostgreSQL")]
        C8[("Almacen de Eventos<br/>PostgreSQL")]
    end

    %% ---------- Relaciones ----------
    AM -.->|"porta"| WE
    AM -->|"HTTPS"| C1
    FA -->|"HTTPS"| C1
    CU -->|"HTTPS"| C1
    AD -->|"HTTPS"| C1
    WE -->|"HTTPS/TLS"| C3

    C1 -->|"HTTPS/JSON"| C2
    C3 -->|"AMQP 1.0"| C4
    C4 -->|"AMQP 1.0"| C5
    C5 -->|"AMQP 1.0"| C4
    C4 -->|"AMQP 1.0"| C6
    C6 -->|"HTTPS y SMTP"| NO
    NO -->|"entrega la alerta"| FA
    NO -->|"entrega la alerta"| CU

    C2 -->|"SQL/TLS"| C7
    C5 -->|"SQL/TLS"| C7
    C6 -->|"SQL/TLS"| C7
    C3 -->|"SQL/TLS"| C8
    C2 -->|"SQL/TLS"| C8

    %% ---------- Estilos ----------
    classDef actor fill:#08427B,stroke:#052E56,color:#FFFFFF
    classDef ext fill:#999999,stroke:#6B6B6B,color:#FFFFFF
    classDef cont fill:#438DD5,stroke:#2E6295,color:#FFFFFF
    class AM,FA,CU,AD actor
    class WE,NO ext
    class C1,C2,C3,C4,C5,C6,C7,C8 cont
```

*Figura 2 — Vista de contenedores (C4 · Nivel 2) de SeniorCareHub*

> **Leyenda.** Azul oscuro: actores. Gris: sistemas externos fuera del alcance del equipo.
> Azul claro: contenedores de SeniorCareHub; los cilindros son almacenes de datos.
> Cada relación indica el protocolo; el detalle de qué transporta cada una se encuentra
> en la tabla 7.2.2.
>
> Fuente editable en notación e iconografía C4 formal: `diagramas/c4-contenedores.puml`.

#### 7.2.1 Contenedores

| # | Contenedor | Tecnología | Responsabilidad |
|---|---|---|---|
| 1 | App Web (dashboard) | SPA React + TypeScript sobre Azure Static Web Apps | Presentar el estado y el historial del adulto mayor, y ofrecer la gestión de reglas de detección, perfiles de monitoreo y canales de notificación. |
| 2 | API de Aplicación | ASP.NET Core Web API sobre Azure App Service | Autenticar al usuario, aplicar autorización por rol, exponer las consultas de estado e historial y las operaciones de configuración. Única puerta de entrada de los usuarios al sistema. |
| 3 | Servicio de Ingesta | ASP.NET Core sobre Azure App Service | Autenticar el dispositivo emisor, validar la estructura del evento, persistirlo en el almacén de eventos y publicarlo en el bus. No evalúa reglas. |
| 4 | Bus de Mensajería | Azure Service Bus, tier Standard (topics + subscriptions) | Transportar y persistir de forma durable los eventos crudos y las alertas confirmadas. Provee reintentos, cola de mensajes muertos, detección de duplicados y sesiones ordenadas por adulto mayor. |
| 5 | Motor de Reglas | .NET Worker Service sobre Azure Container Apps | Consumir eventos crudos, evaluar las reglas de detección configurables, aplicar la ventana de confirmación y la deduplicación, y publicar la alerta confirmada. |
| 6 | Servicio de Notificaciones | .NET Worker Service sobre Azure Container Apps | Consumir alertas confirmadas, resolver destinatarios y canales según el perfil, invocar a los proveedores externos mediante adaptadores y registrar el acuse de entrega. |
| 7 | BD Operativa | Azure Database for PostgreSQL Flexible Server | Almacenar usuarios, perfiles de monitoreo, reglas, alertas y acuses. Es la fuente de verdad del sistema y soporta la bitácora de auditoría exigida por REST-02. |
| 8 | Almacén de Eventos | Azure Database for PostgreSQL, particionado por tiempo | Conservar el historial de eventos recibidos del wearable para consulta, análisis posterior y evidencia ante disputas sobre una alerta. |

#### 7.2.2 Relaciones y protocolos

| Origen | Destino | Protocolo | Descripción |
|---|---|---|---|
| Wearable simulado | Servicio de Ingesta | HTTPS/JSON sobre TLS | Emite eventos de movimiento, inactividad y ubicación. |
| Adulto Mayor | App Web | HTTPS | Consulta su propio estado e historial desde el portal. |
| Familiar / Cuidador / Administrador | App Web | HTTPS | Acceden al dashboard desde el navegador. |
| App Web | API de Aplicación | HTTPS/JSON | Consultas y operaciones de configuración. |
| Servicio de Ingesta | Bus de Mensajería | AMQP 1.0 sobre TLS | Publica el evento crudo en el tópico correspondiente. |
| Bus de Mensajería | Motor de Reglas | AMQP 1.0 sobre TLS | Entrega el evento crudo con sesión por adulto mayor. |
| Motor de Reglas | Bus de Mensajería | AMQP 1.0 sobre TLS | Publica la alerta confirmada. |
| Bus de Mensajería | Servicio de Notificaciones | AMQP 1.0 sobre TLS | Entrega la alerta confirmada para su despacho. |
| Servicio de Notificaciones | Servicios de notificación externos | HTTPS/REST y SMTP | Despacha la notificación por el canal correspondiente. |
| Servicios de notificación externos | Familiar / Cuidador Profesional | SMS, correo y mensajería | Entregan la alerta al destinatario final, tal como se representó en §7.1. |
| API de Aplicación / Motor de Reglas / Servicio de Notificaciones | BD Operativa | PostgreSQL wire protocol sobre TLS | Lectura y escritura de configuración, alertas y acuses. |
| Servicio de Ingesta / API de Aplicación | Almacén de Eventos | PostgreSQL wire protocol sobre TLS | Persistencia y consulta del historial de eventos. |

#### 7.2.3 Consistencia con la vista de contexto

Los cuatro actores (Adulto Mayor, Familiar, Cuidador Profesional y Administrador) y los dos sistemas externos (Wearable simulado y Servicios de notificación externos) son los mismos declarados en §7.1, sin altas ni bajas. Las relaciones que en la vista de contexto entraban o salían de la caja única de SeniorCareHub se refinan aquí hacia el contenedor específico que las atiende: la emisión de eventos del wearable aterriza en el Servicio de Ingesta, el acceso de los cuatro actores humanos entra por la App Web, y la salida hacia los proveedores de notificación parte del Servicio de Notificaciones, que a su vez entregan la alerta al Familiar y al Cuidador Profesional tal como se representó en §7.1. El Adulto Mayor conserva la doble relación con el sistema definida en §7.1: una indirecta, mediada por el wearable que genera los eventos, y una directa con la App Web cuando consulta su propio estado e historial.

### 7.3 Vista de comportamiento

Esta vista describe cómo colaboran en el tiempo los contenedores definidos en §7.2 para producir los comportamientos que el sistema debe garantizar. Se documentan tres flujos representativos, seleccionados porque cada uno ejercita un escenario de calidad distinto de §4 y porque juntos recorren el camino crítico completo, su modo de fallo principal y su capacidad de cambio en operación.

Los diagramas de esta sección operan a nivel de contenedores: muestran qué unidad desplegable participa y en qué orden, sin detallar la estructura interna de cada una. El comportamiento interno de los componentes se documenta en §10.

#### 7.3.1 Flujo 1 — Detección y notificación de un evento crítico

Corresponde al camino principal del sistema y ejercita el escenario QS-02, cuya meta es que la primera notificación se emita en cinco segundos o menos en el percentil 95.

```mermaid
sequenceDiagram
    autonumber
    participant WE as Wearable simulado
    participant ING as Servicio de Ingesta
    participant BDE as Almacen de Eventos
    participant BUS as Bus de Mensajeria
    participant MR as Motor de Reglas
    participant BDO as BD Operativa
    participant SN as Servicio de Notificaciones
    participant PR as Proveedores externos
    participant FA as Familiar

    WE->>ING: Emitir evento de caida (HTTPS/TLS)
    ING->>ING: Autenticar dispositivo y validar estructura
    ING->>BDE: Persistir el evento
    ING->>BUS: Publicar evento crudo (SessionId = adulto mayor)
    ING-->>WE: Aceptado

    Note over ING,BUS: La ingesta responde sin esperar la evaluacion.<br/>El desacople temporal protege QS-01.

    BUS->>MR: Entregar evento crudo
    MR->>BDO: Leer reglas vigentes y estado de confirmacion
    BDO-->>MR: Version de reglas y estado
    MR->>MR: Evaluar. Criticidad inmediata: no requiere ventana
    MR->>BDO: Registrar alerta con version de reglas y eventos origen
    MR->>BUS: Publicar alerta confirmada
    MR->>BUS: Completar el evento crudo

    BUS->>SN: Entregar alerta confirmada
    SN->>BDO: Leer perfil de notificacion y canales por prioridad
    BDO-->>SN: Destinatarios y orden de canales
    SN->>PR: Despachar por el canal de mayor prioridad
    PR-->>SN: Aceptado
    PR->>FA: Entregar la alerta
    SN->>BDO: Registrar acuse de entrega
    SN->>BUS: Completar la alerta

    Note over WE,FA: Presupuesto extremo a extremo: 5 s en el percentil 95 (QS-02).
```

*Figura 12 — Secuencia de sistema: detección y notificación de un evento crítico*

#### 7.3.2 Flujo 2 — Fallo del proveedor de notificación

Ejercita el escenario QS-03: ante la indisponibilidad de un proveedor, ninguna alerta crítica se pierde y la entrega se completa por un canal alterno en menos de diez segundos. Es el flujo que justifica el estilo adoptado en §8.

```mermaid
sequenceDiagram
    autonumber
    participant BUS as Bus de Mensajeria
    participant SN as Servicio de Notificaciones
    participant BDO as BD Operativa
    participant P1 as Proveedor SMS
    participant P2 as Proveedor correo
    participant CU as Cuidador

    BUS->>SN: Entregar alerta confirmada
    SN->>BDO: Leer canales ordenados por prioridad
    BDO-->>SN: SMS, luego correo

    SN->>P1: Despachar por SMS
    P1--xSN: Sin respuesta dentro del tiempo limite
    SN->>BDO: Registrar intento fallido

    Note over SN,BUS: La alerta sigue bloqueada en el bus, no completada.<br/>Mientras no se confirme, no puede perderse.

    SN->>P2: Despachar por el siguiente canal
    P2-->>SN: Aceptado
    P2->>CU: Entregar la alerta
    SN->>BDO: Registrar acuse con canal efectivo
    SN->>BUS: Completar la alerta

    alt Todos los canales fallan
        SN->>BUS: No completar la alerta
        BUS->>SN: Reentregar tras el intervalo de reintento
        Note over BUS: Agotados los reintentos, la alerta pasa a la<br/>cola de mensajes muertos para intervencion manual.<br/>En ningun caso se descarta.
    end
```

*Figura 13 — Secuencia de sistema: fallo del proveedor y entrega por canal alterno*

#### 7.3.3 Flujo 3 — Cambio de una regla de detección en operación

Ejercita el escenario QS-05: una regla nueva o modificada entra en vigencia en menos de diez minutos y sin detener el sistema. Demuestra la modificabilidad declarada como QA-05 y materializa la decisión de ADR-002.

```mermaid
sequenceDiagram
    autonumber
    participant AD as Administrador
    participant WEB as App Web
    participant API as API de Aplicacion
    participant BDO as BD Operativa
    participant BUS as Bus de Mensajeria
    participant MR as Motor de Reglas

    AD->>WEB: Modificar umbral de inactividad de un perfil
    WEB->>API: Enviar la configuracion (HTTPS/JSON)
    API->>API: Autorizar por rol y validar la configuracion
    API->>BDO: Guardar como version nueva de la regla
    BDO-->>API: Version registrada
    API-->>WEB: Confirmacion
    WEB-->>AD: Regla vigente

    Note over API,BDO: La regla anterior se conserva.<br/>El versionado permite auditar que version<br/>evaluo cada alerta (ADR-002).

    BUS->>MR: Entregar el siguiente evento del mismo adulto mayor
    MR->>BDO: Leer reglas vigentes
    BDO-->>MR: Version nueva
    MR->>MR: Evaluar con la version nueva
    MR->>BDO: Registrar la evaluacion con la version utilizada

    Note over MR: Sin recompilar, sin redesplegar y sin reiniciar<br/>ningun contenedor del sistema.
```

*Figura 14 — Secuencia de sistema: cambio de una regla de detección sin interrupción*

#### 7.3.4 Trazabilidad hacia los escenarios de calidad

| Flujo | Escenario | Qué demuestra | Contenedores involucrados |
|---|---|---|---|
| Detección y notificación de evento crítico | QS-02 | El camino completo se recorre dentro del presupuesto de latencia, con respuesta inmediata de la ingesta | Ingesta, Bus, Motor de Reglas, Notificaciones, ambas bases |
| Fallo del proveedor de notificación | QS-03 | La alerta permanece bajo custodia del bus hasta confirmarse la entrega; el fallo de un canal no la destruye | Bus, Notificaciones, BD Operativa |
| Cambio de regla en operación | QS-05 | La configuración se modifica en caliente y la evaluación siguiente ya utiliza la versión nueva | App Web, API, BD Operativa, Motor de Reglas |

Los tres flujos comparten una propiedad que conviene hacer explícita: en ningún momento un contenedor invoca sincrónicamente a otro dentro del camino crítico. Toda transición entre etapas ocurre a través del bus, lo que constituye la evidencia de comportamiento del estilo adoptado en §8 y de la decisión registrada en ADR-001.

#### 7.3.5 Consistencia con las vistas anteriores

Los participantes de los tres diagramas son exclusivamente contenedores declarados en §7.2 y actores o sistemas externos declarados en §7.1. No se introduce ningún elemento nuevo. Las relaciones ejercitadas —ingesta hacia el bus, bus hacia el motor, motor hacia el bus, bus hacia notificaciones, y notificaciones hacia los proveedores externos— son las mismas de la tabla de relaciones de §7.2.2, recorridas ahora en orden temporal.

### 7.4 Vista de despliegue

Esta vista muestra la infraestructura sobre la que se ejecutan los contenedores definidos en §7.2, la correspondencia entre cada contenedor y su nodo de ejecución, y las decisiones de dimensionamiento que responden a los atributos de calidad del sistema.

Todos los recursos gestionados se agrupan en un único grupo de recursos, `rg-seniorcarehub`, desplegado en la región **East US 2** por ser la de menor latencia de red hacia Costa Rica entre las regiones con disponibilidad completa de los servicios utilizados. La agrupación en un solo grupo de recursos permite además aprovisionar y eliminar el entorno completo como una unidad, lo que resulta conveniente para un despliegue de duración acotada.

```mermaid
flowchart TB
    subgraph CLI["Fuera de la nube"]
        direction TB
        NAV["Dispositivo del usuario<br/>Navegador web"]
        SIM["Estacion de simulacion<br/>Contenedor Docker · .NET"]
    end

    subgraph AZ["Azure · East US 2 · rg-seniorcarehub"]
        direction TB
        SWA["Azure Static Web Apps<br/><i>Free</i>"]
        subgraph ASP["App Service Plan Linux B1"]
            direction TB
            API["API de Aplicacion"]
            ING["Servicio de Ingesta"]
        end
        subgraph ACA["Entorno de Azure Container Apps"]
            direction TB
            REG["Motor de Reglas<br/><i>min 1 replica</i>"]
            NOT["Servicio de Notificaciones<br/><i>min 1 replica</i>"]
        end
        SB["Azure Service Bus<br/><i>Standard</i>"]
        subgraph PG["PostgreSQL Flexible Server B1ms"]
            direction TB
            BDO[("BD Operativa")]
            BDE[("Almacen de Eventos")]
        end
        KV["Azure Key Vault"]
        AI["Application Insights<br/>+ Log Analytics"]
    end

    EXT["Servicios de notificacion<br/><i>proveedores externos</i>"]

    NAV -->|"HTTPS"| SWA
    SWA -->|"HTTPS/JSON"| API
    SIM -->|"HTTPS/TLS"| ING

    ING -->|"AMQP 1.0"| SB
    SB -->|"AMQP 1.0"| REG
    REG -->|"AMQP 1.0"| SB
    SB -->|"AMQP 1.0"| NOT
    NOT -->|"HTTPS y SMTP"| EXT

    API -->|"SQL/TLS"| BDO
    REG -->|"SQL/TLS"| BDO
    NOT -->|"SQL/TLS"| BDO
    ING -->|"SQL/TLS"| BDE
    API -->|"SQL/TLS"| BDE

    API -.->|"telemetria"| AI
    ING -.->|"telemetria"| AI
    REG -.->|"telemetria"| AI
    NOT -.->|"telemetria"| AI

    API -.->|"secretos"| KV
    ING -.->|"secretos"| KV
    REG -.->|"secretos"| KV
    NOT -.->|"secretos"| KV

    classDef nube fill:#438DD5,stroke:#2E6295,color:#FFFFFF
    classDef fuera fill:#08427B,stroke:#052E56,color:#FFFFFF
    classDef ext fill:#999999,stroke:#6B6B6B,color:#FFFFFF
    class SWA,API,ING,REG,NOT,SB,BDO,BDE,KV,AI nube
    class NAV,SIM fuera
    class EXT ext
```

*Figura 15 — Vista de despliegue de SeniorCareHub sobre Microsoft Azure*

> **Leyenda.** Azul oscuro: nodos fuera de la nube. Azul claro: recursos gestionados de Azure. Gris: proveedores externos. Las líneas continuas representan el flujo funcional; las punteadas, dependencias transversales de telemetría y gestión de secretos.
>
> Fuente editable en notación e iconografía C4 formal: `diagramas/c4-despliegue.puml`.

#### 7.4.1 Nodos de despliegue

| Nodo | Servicio y dimensionamiento | Contenedores alojados (§7.2) | Justificación |
|---|---|---|---|
| Dispositivo del usuario | Navegador web | App Web (ejecución) | La SPA se descarga y ejecuta en el cliente; no requiere cómputo en la nube. |
| Estación de simulación | Contenedor Docker con aplicación .NET | Simulador de wearable (sistema externo) | Sustituye al dispositivo físico. Se ejecuta fuera de la nube para representar fielmente a un emisor externo al sistema. |
| Azure Static Web Apps | Nivel Free | App Web (distribución) | Distribuye los archivos estáticos con certificado TLS gestionado. El nivel gratuito es suficiente porque no ejecuta lógica de servidor. |
| App Service Plan Linux | B1 (1 vCPU, 1.75 GB) | API de Aplicación, Servicio de Ingesta | Ambos son servicios web con carga moderada y exposición HTTP directa. Comparten plan para contener el costo. |
| Entorno de Azure Container Apps | Consumo, mínimo 1 réplica por aplicación | Motor de Reglas, Servicio de Notificaciones | Procesos consumidores de larga duración, sin exposición pública, con escalado independiente por profundidad de cola. |
| Azure Service Bus | Namespace tier Standard | Bus de Mensajería | El tier Standard es el mínimo que ofrece tópicos con suscripciones múltiples, sesiones ordenadas y detección de duplicados. |
| PostgreSQL Flexible Server | B1ms, almacenamiento 32 GB | BD Operativa, Almacén de Eventos | Un mismo servidor aloja ambas bases lógicas. El volumen previsto no justifica dos servidores independientes. |
| Azure Key Vault | Estándar | — | Custodia cadenas de conexión y credenciales de proveedores externos. |
| Application Insights y Log Analytics | Pago por ingesta | — | Recibe trazas correlacionadas de los cuatro servicios y sostiene la depuración distribuida exigida por el estilo (§8.4). |

#### 7.4.2 Decisiones de despliegue y sus trade-offs

**Réplica mínima permanente en Container Apps.** Azure Container Apps permite reducir a cero réplicas y no facturar cómputo en reposo. Se descarta esa configuración para el Motor de Reglas y el Servicio de Notificaciones porque el arranque en frío introduce una demora de varios segundos que consumiría el presupuesto completo de QS-02, cuya meta es de 5 segundos en el percentil 95. Se acepta el costo de mantener una réplica activa de forma permanente a cambio de latencia predecible. El escalado hacia arriba sí es automático, gobernado por la profundidad de la suscripción del bus.

**Plan de App Service compartido entre API e Ingesta.** Ambos servicios web comparten un mismo plan B1, lo que reduce el costo respecto de dos planes independientes. La contrapartida es que comparten CPU y memoria: una ráfaga sostenida de eventos entrantes podría degradar el tiempo de respuesta de las consultas del dashboard. Se acepta el riesgo porque los perfiles de carga previstos son moderados, y la mitigación está identificada: separar la ingesta a un plan propio, cambio que no afecta a ningún otro contenedor por tratarse de servicios ya desacoplados.

**Identidades administradas en lugar de credenciales en configuración.** El acceso de los servicios a Service Bus, PostgreSQL y Key Vault se realiza mediante identidades administradas de Microsoft Entra ID, de modo que ninguna credencial persiste en archivos de configuración ni en variables de entorno del despliegue. Esta decisión responde directamente a QA-04 y a la obligación de resguardo de datos personales de REST-02, y reduce la superficie expuesta ante una filtración de configuración.

**Instancia única sin redundancia de zona.** Los servicios se despliegan en una sola zona de disponibilidad. La redundancia de zona multiplicaría el costo de PostgreSQL y del namespace de mensajería, y la meta comprometida en QS-01 es de 99.5 % mensual, equivalente a poco menos de 3.6 horas de indisponibilidad, alcanzable con la configuración descrita. El dimensionamiento se realizó contra la meta declarada y no por encima de ella; elevar el objetivo de disponibilidad exigiría revisar esta decisión antes que cualquier otra.

#### 7.4.3 Consistencia con la vista de contenedores

Los ocho contenedores de §7.2 tienen exactamente un nodo de ejecución asignado y no se introduce ningún contenedor nuevo. La App Web aparece dos veces con roles distintos —distribuida desde Static Web Apps y ejecutada en el navegador del usuario—, lo que corresponde a la naturaleza de una aplicación de página única. El Bus de Mensajería se materializa en el namespace de Service Bus, y las dos bases de datos comparten servidor sin dejar de ser almacenes lógicamente separados, tal como se representó en §7.2. Los proveedores externos de notificación conservan su condición de sistemas fuera del alcance del equipo, coherente con §7.1. Key Vault y Application Insights no corresponden a contenedores del nivel 2: son servicios de plataforma transversales que sostienen decisiones ya documentadas en §8.4 sobre observabilidad y gestión de secretos.

### 7.5 Vista de concurrencia

Esta vista describe qué unidades de ejecución operan simultáneamente en SeniorCareHub, qué recursos comparten, dónde aparece contención y qué mecanismos garantizan que el procesamiento concurrente no comprometa la corrección de las alertas.

El sistema es concurrente por naturaleza: múltiples adultos mayores emiten eventos al mismo tiempo, y cada etapa del pipeline definido en §8 escala de forma independiente. El desafío central no es el volumen, sino el orden: la etapa de confirmación descrita en ADR-003 mantiene estado temporal por persona monitoreada, y evaluar dos eventos de un mismo adulto mayor en paralelo produciría alertas duplicadas o, peor, la pérdida de una confirmación válida.

```mermaid
flowchart TB
    ING["Servicio de Ingesta<br/><i>sin estado · N instancias</i>"]

    subgraph BUS["Bus de Mensajeria"]
        direction TB
        TE["Topico: eventos crudos<br/><i>con sesiones</i>"]
        TA["Topico: alertas confirmadas<br/><i>sin sesiones</i>"]
    end

    subgraph SES["Sesiones activas · una por adulto mayor"]
        direction LR
        S1["sesion AM-001"]
        S2["sesion AM-002"]
        S3["sesion AM-003"]
    end

    subgraph MR["Motor de Reglas · replicas"]
        direction LR
        R1["replica 1"]
        R2["replica 2"]
    end

    subgraph SN["Servicio de Notificaciones · replicas"]
        direction LR
        N1["replica 1"]
        N2["replica 2"]
    end

    EST[("BD Operativa<br/>estado de confirmacion<br/>+ reglas + alertas")]

    ING -->|"publica con<br/>SessionId = id adulto mayor"| TE
    TE --> S1
    TE --> S2
    TE --> S3

    S1 -->|"lock exclusivo"| R1
    S2 -->|"lock exclusivo"| R1
    S3 -->|"lock exclusivo"| R2

    R1 -->|"lee y persiste estado"| EST
    R2 -->|"lee y persiste estado"| EST

    R1 -->|"publica alerta"| TA
    R2 -->|"publica alerta"| TA

    TA -->|"consumidores<br/>en competencia"| N1
    TA -->|"consumidores<br/>en competencia"| N2

    classDef proc fill:#438DD5,stroke:#2E6295,color:#FFFFFF
    classDef ses fill:#0E7C86,stroke:#0A5A61,color:#FFFFFF
    classDef db fill:#08427B,stroke:#052E56,color:#FFFFFF
    class ING,R1,R2,N1,N2,TE,TA proc
    class S1,S2,S3 ses
    class EST db
```

*Figura 16 — Vista de concurrencia: particionamiento por sesión y consumidores en competencia*

> **Leyenda.** Azul claro: unidades de ejecución concurrentes. Verde azulado: sesiones lógicas del bus, una por adulto mayor. Azul oscuro: estado compartido persistente.

#### 7.5.1 Unidades de concurrencia

| Unidad | Multiplicidad | Disparador | Estado que mantiene | Aislamiento |
|---|---|---|---|---|
| Servicio de Ingesta | N instancias tras el balanceador del App Service | Petición HTTP del wearable | Ninguno | No requiere: es un servicio sin estado. |
| Motor de Reglas | N réplicas, escaladas por profundidad de la suscripción | Mensaje disponible en la suscripción con sesión | Ventana de confirmación por adulto mayor | Sesión con bloqueo exclusivo: una sesión es atendida por una sola réplica a la vez. |
| Servicio de Notificaciones | N réplicas | Alerta confirmada disponible | Ninguno entre mensajes | Consumidores en competencia sin orden garantizado, porque el despacho de dos alertas distintas es independiente. |
| API de Aplicación | N instancias | Petición HTTP del usuario | Ninguno | Transacciones de base de datos para lecturas y escrituras de configuración. |

#### 7.5.2 Recursos compartidos y puntos de contención

| Recurso compartido | Quién lo accede concurrentemente | Riesgo | Mecanismo de control |
|---|---|---|---|
| Estado de la ventana de confirmación | Réplicas del Motor de Reglas | Dos eventos del mismo adulto mayor evaluados en paralelo generan alertas duplicadas o pierden una confirmación | Particionamiento por sesión: el bloqueo exclusivo de sesión garantiza que solo una réplica evalúe a esa persona en un momento dado |
| Configuración de reglas y perfiles | Motor de Reglas (lectura), API (escritura) | Una evaluación en curso podría usar una regla a medio actualizar | Versionado de reglas según ADR-002: cada evaluación fija la versión al inicio y la registra en la alerta |
| BD Operativa | Motor de Reglas, Servicio de Notificaciones, API | Agotamiento del pool de conexiones al escalar réplicas | Dimensionamiento del pool por réplica contra el límite de conexiones del servidor B1ms |
| Registro de acuses de entrega | Réplicas del Servicio de Notificaciones | Doble envío ante reentrega del mismo mensaje | Clave de idempotencia por alerta y canal: el segundo intento detecta el acuse ya registrado y no reenvía |

#### 7.5.3 Decisiones de sincronización

**Particionamiento por sesión en lugar de bloqueos explícitos.** La ordenación se resuelve en la infraestructura de mensajería y no en el código: el Servicio de Ingesta publica cada evento con el identificador del adulto mayor como identificador de sesión, y el bus garantiza que los mensajes de una misma sesión se entreguen en orden a un único consumidor con bloqueo exclusivo. El Motor de Reglas nunca necesita bloqueos ni semáforos sobre el estado de confirmación, porque no existe acceso concurrente a la partición que atiende. El trade-off es que el paralelismo máximo efectivo queda acotado por el número de adultos mayores con eventos activos: agregar réplicas más allá de esa cifra no incrementa el rendimiento. Para el volumen previsto es una restricción holgada.

**El estado de confirmación se persiste, no reside solo en memoria.** ADR-003 dejó abierta la recuperación del estado temporal tras un reinicio. Se decide persistir el estado de cada ventana de confirmación en la BD Operativa, con el identificador del adulto mayor como clave, y actualizarlo en la misma transacción en que se registra la evaluación. Al tomar una sesión, la réplica lee el estado vigente antes de evaluar. De este modo, si una réplica termina de forma abrupta, el bus libera el bloqueo y otra réplica retoma la sesión desde el último estado confirmado, en lugar de comenzar la ventana desde cero y arriesgar la pérdida de una alerta —lo que comprometería QS-03. El costo es una escritura adicional por evaluación, admisible dentro del presupuesto de latencia de QS-02.

**Duración del bloqueo dimensionada por encima del tiempo de evaluación.** El bus mantiene el mensaje bloqueado durante un intervalo acotado mientras el consumidor lo procesa. Si la evaluación excediera ese intervalo, el mensaje se reentregaría y produciría una evaluación duplicada. Se establece que la duración del bloqueo debe superar con margen el tiempo de evaluación observado en el percentil 99, y que el consumidor renueve el bloqueo en operaciones prolongadas. Esta es una condición de configuración que debe verificarse durante las pruebas y no una propiedad garantizada por el diseño.

**Idempotencia como requisito transversal.** Por la semántica de entrega al menos una vez documentada en §8.4, todo consumidor debe tolerar recibir el mismo mensaje más de una vez. En el Motor de Reglas esto se resuelve con la deduplicación por adulto mayor, tipo de evento y período establecida en ADR-003; en el Servicio de Notificaciones, con la clave de idempotencia por alerta y canal. Ningún consumidor puede asumir procesamiento exactamente una vez.

**Ausencia de orden en el despacho de notificaciones.** El tópico de alertas confirmadas no utiliza sesiones. Dos alertas de personas distintas, o incluso dos alertas sucesivas de la misma persona, pueden despacharse en paralelo y en cualquier orden, porque cada notificación es independiente y lleva su propia referencia a los eventos que la originaron. Renunciar al orden en esta etapa maximiza el paralelismo justo donde la latencia importa, sin afectar la corrección.

#### 7.5.4 Consistencia con las vistas anteriores

Las unidades de concurrencia descritas corresponden exactamente a los contenedores de §7.2 y a los nodos de ejecución de §7.4: las réplicas del Motor de Reglas y del Servicio de Notificaciones son las instancias del entorno de Container Apps, y las instancias de la API y la Ingesta son las del App Service Plan compartido. Las sesiones no constituyen un contenedor adicional, sino una característica del Bus de Mensajería ya declarada en la tabla de contenedores de §7.2.1 y justificada en §8.

### 7.6 Evolución de las vistas arquitectónicas

Las vistas de este capítulo no se produjeron de una sola vez: se construyeron y corrigieron a lo largo de los tres hitos del proyecto. Esta sección documenta qué cambió en cada una, por qué cambió y dónde queda la evidencia en el historial del repositorio, de modo que la arquitectura presentada pueda leerse como el resultado de un proceso de revisión y no como una versión final sin trazabilidad.

#### 7.6.1 Resumen por hito

| Hito | Vistas incorporadas | Vistas modificadas |
|---|---|---|
| Avance 1 (S07) | 7.1 Vista de contexto | — |
| Avance 2 (S11) | 7.2 Vista de contenedores | 7.2 (tres correcciones posteriores a la revisión interna) |
| Entrega final (S14) | 7.3 Comportamiento, 7.4 Despliegue, 7.5 Concurrencia | — |

#### 7.6.2 Vista de contexto: estabilidad deliberada

La vista de contexto no ha sufrido modificaciones sustantivas desde el Avance 1. Sus cuatro actores —Adulto Mayor, Familiar, Cuidador Profesional y Administrador— y sus dos sistemas externos —el wearable simulado y los servicios de notificación— se mantienen sin altas ni bajas.

Esta estabilidad no es casual y conviene interpretarla correctamente: indica que la frontera del sistema y su relación con el entorno quedaron bien delimitadas desde el primer hito. Los cambios posteriores ocurrieron todos en niveles de mayor detalle, que es donde se esperaba que ocurrieran. Un vaivén en la vista de contexto habría señalado, en cambio, un problema de alcance no resuelto.

#### 7.6.3 Vista de contenedores: tres correcciones de consistencia

La vista de contenedores se incorporó en el Avance 2 y recibió tres modificaciones antes de la entrega final. Las tres se originaron en revisiones internas del equipo, no en observaciones del docente, y las tres corrigen inconsistencias con el nivel superior.

| Cambio | Motivo | Evidencia |
|---|---|---|
| Se agregó la relación directa entre el Adulto Mayor y la App Web, y se rediseñó el diagrama para mejorar su legibilidad | La revisión de una integrante detectó que la vista de contexto incluía al Adulto Mayor como usuario del portal de consultas, mientras que la vista de contenedores lo representaba únicamente como portador del wearable | `63fdd2d` |
| Se agregaron las relaciones de entrega de la alerta desde los servicios de notificación hacia el Familiar y el Cuidador Profesional | La vista de contexto mostraba la entrega final al destinatario, pero en la vista de contenedores el flujo terminaba en los proveedores externos y nunca alcanzaba a las personas | `cee2141` |
| Se agregó el pie de la Figura 2 | La numeración de figuras del documento saltaba de la 1 a la 3 porque el diagrama de contenedores carecía de rótulo | `06c6e4b` |

El patrón común de las dos primeras es significativo: en ambos casos el nivel de contenedores había omitido una relación que el nivel de contexto sí declaraba. La revisión cruzada entre niveles resultó ser el mecanismo que las detectó, y por eso cada vista de este capítulo cierra con una subsección explícita de consistencia con las anteriores.

#### 7.6.4 Vistas incorporadas en la entrega final

Las tres vistas agregadas en este hito no modifican las anteriores: las complementan en dimensiones que hasta el Avance 2 no estaban documentadas.

La **vista de comportamiento** (§7.3) recorre en el tiempo las mismas relaciones ya declaradas en la tabla de §7.2.2, sin introducir participantes nuevos. La **vista de despliegue** (§7.4) asigna un nodo de ejecución a cada contenedor existente, sin crear contenedores adicionales. La **vista de concurrencia** (§7.5) describe cómo se multiplican en ejecución esos mismos contenedores y cómo se coordinan al compartir estado.

Una de ellas, además, cerró una decisión que había quedado abierta: ADR-003 señalaba como consecuencia negativa que era necesario definir cómo recuperar el estado temporal de la ventana de confirmación tras un reinicio. La §7.5.3 resuelve ese punto al establecer que el estado se persiste en la BD Operativa dentro de la misma transacción de la evaluación, lo que permite que otra réplica retome una sesión interrumpida sin reiniciar la ventana.

#### 7.6.5 Cambios evaluados y descartados

Documentar lo que no cambió, y por qué, forma parte del registro de evolución.

**Incorporar Microsoft Entra ID como sistema externo.** Al elaborar la vista de despliegue se evaluó representar al proveedor de identidad como un elemento explícito de las vistas. Se descartó porque habría introducido en un nivel inferior un sistema externo ausente de la vista de contexto del Avance 1, generando precisamente el tipo de inconsistencia entre niveles que las revisiones anteriores habían corregido. El mecanismo de identidades administradas se documenta en el texto de §7.4.2, donde su justificación es visible sin alterar la frontera del sistema.

**Desagregar los proveedores de notificación por canal.** Se consideró representar por separado los proveedores de correo, SMS y mensajería. Se mantuvo la agrupación en un único sistema externo por coherencia con la vista de contexto, y porque la diferenciación por canal es una cuestión de configuración resuelta mediante adaptadores según ADR-004, no una distinción de frontera arquitectónica.

**Materializar un contenedor de Gestión de Alertas.** ADR-001 identificaba inicialmente la gestión de alertas como una responsabilidad separada dentro del pipeline. Se optó por alojarla en el Motor de Reglas en lugar de crear un contenedor propio, dado que su ciclo de vida y sus datos están estrechamente ligados a la evaluación que origina la alerta. La redacción del ADR se ajustó en consecuencia para mantener la coherencia con esta vista.

---

# BLOQUE 4 — DECISIONES ARQUITECTÓNICAS
*Hito: Avance 2 (S11)*

---

## 8. Estilo arquitectónico

### 8.1 Estilo seleccionado

El estilo arquitectónico principal de SeniorCareHub es **orientado a eventos, en su variante publicación-suscripción con intermediario durable** (*publish-subscribe with durable broker*). Este estilo gobierna el camino crítico del sistema: ingesta del evento del wearable, detección de la situación mediante reglas y despacho de la notificación.

El estilo principal se complementa con dos estilos secundarios de menor alcance:

- **Por capas**, aplicado dentro de cada contenedor, para separar presentación, lógica de aplicación, dominio y acceso a datos.
- **Puertos y adaptadores**, aplicado en la frontera con los proveedores externos de notificación, para aislar al sistema de la variabilidad de sus interfaces y de su disponibilidad (REST-01).

### 8.2 Justificación frente a los drivers y escenarios de calidad

El estilo responde de forma directa a los atributos de calidad definidos en §3.2 y a los escenarios formalizados en §4.

| Driver | Cómo lo atiende el estilo | Escenario |
|---|---|---|
| **QA-03 · Confiabilidad: cero alertas perdidas** | El intermediario persiste el evento antes de que exista un consumidor listo, y el consumidor confirma su recepción (*acknowledgement*) solo después de completar el procesamiento. Si el Motor de Reglas falla a mitad de una evaluación, el evento no se descarta: se reentrega. La no pérdida deja de depender de que un proceso permanezca vivo y pasa a ser una propiedad garantizada por la infraestructura de mensajería. | QS-03 |
| **QA-01 · Disponibilidad ≥ 99.5 %** | El desacople temporal entre productores y consumidores permite que el Servicio de Ingesta siga aceptando eventos aunque el Motor de Reglas o el Servicio de Notificaciones estén caídos o saturados. La disponibilidad del sistema deja de ser el producto de la disponibilidad de todos los componentes encadenados. | QS-01 |
| **QA-02 · Latencia de alerta ≤ 5 s (p95)** | La propagación es por empuje (*push*) y no por sondeo periódico, de modo que el evento avanza en cuanto está disponible. El salto adicional por el intermediario introduce un costo del orden de decenas de milisegundos, holgadamente contenido dentro del presupuesto de 5 segundos. | QS-02 |
| **QA-04 · Seguridad y privacidad (Ley N.° 8968)** | El estilo *tensiona* este driver más que favorecerlo; ver §8.4. Se mitiga con minimización del contenido de los eventos, segregación de tópicos por sensibilidad y cifrado en tránsito y en reposo. | QS-04 |
| **QA-05 · Modificabilidad** | Las reglas de detección se tratan como datos residentes en la BD Operativa y no como código distribuido entre componentes. Además, incorporar un consumidor nuevo —por ejemplo, un servicio de analítica o un registro de auditoría independiente— consiste en suscribirlo a un tópico existente, sin modificar al productor ni redesplegarlo. | QS-05 |
| **REST-01 · Dependencia de proveedores externos** | La indisponibilidad de un proveedor de notificación queda contenida en el Servicio de Notificaciones: la alerta permanece en el tópico y se reintenta, en lugar de propagar el fallo hacia atrás hasta la ingesta. | — |

Adicionalmente, el estilo da lugar natural a la resolución de la tensión **Exactitud ↔ Latencia** identificada en §3: la ventana de confirmación y la deduplicación que reducen las falsas alarmas se implementan como un consumidor con estado ubicado entre la detección y la notificación, sin acoplar esa lógica ni al emisor del evento ni al despachador de la alerta.

### 8.3 Alternativas evaluadas y rechazadas

#### Alternativa A — Monolito por capas con procesamiento sincrónico

Un único contenedor desplegable donde la recepción del evento, la evaluación de reglas y el envío de la notificación ocurren dentro de la misma llamada.

**A favor:** costo operativo sensiblemente menor, un solo artefacto que desplegar y monitorear; depuración directa mediante una traza de ejecución única; y una sola frontera de seguridad, lo que resulta más favorable para QA-04 que la solución adoptada. En condiciones normales presenta además la menor latencia posible, al no existir saltos intermedios.

**Por qué se rechaza:** el fallo de cualquier eslabón se propaga hacia atrás hasta la ingesta. Si el proveedor externo de notificación no responde (REST-01) o el evaluador de reglas lanza una excepción, el evento se pierde sin que exista un lugar donde reintentarlo, lo que incumple **QA-03**. La disponibilidad total, además, queda acotada por la del componente más débil de la cadena, lo que compromete **QA-01**. El trade-off aceptado al descartarla es explícito: se sacrifica simplicidad operativa y unidad de la frontera de seguridad a cambio de garantía de no pérdida y de disponibilidad.

#### Alternativa B — Servicios independientes con comunicación REST sincrónica punto a punto

La misma descomposición en servicios de la solución adoptada, pero comunicados mediante llamadas HTTP directas entre sí, sin intermediario.

**A favor:** conserva la desplegabilidad independiente y buena parte de la modificabilidad de **QA-05**; el flujo de una alerta es más fácil de seguir porque la traza es una cadena de llamadas identificable; y no introduce un componente de infraestructura adicional que operar.

**Por qué se rechaza:** encadenar llamadas sincrónicas multiplica las probabilidades individuales de disponibilidad, de modo que el conjunto es menos disponible que cualquiera de sus partes, en contra de **QA-01**. Más grave para este dominio: exige que el receptor esté disponible en el instante exacto en que ocurre la alerta, y los reintentos residen en la memoria del proceso llamador, por lo que un reinicio durante el reintento pierde la alerta de forma definitiva —el mismo incumplimiento de **QA-03** que en la Alternativa A, ahora con mayor complejidad de despliegue.

### 8.4 Consecuencias asumidas

**Positivas.** Garantía de no pérdida sostenida por la infraestructura y no por el código de aplicación; aislamiento de fallos entre etapas del camino crítico; capacidad de absorber ráfagas de eventos sin degradar la ingesta; y extensión del sistema por suscripción de consumidores nuevos sin modificar a los existentes.

**Negativas.** Se documentan de forma explícita porque condicionan el diseño detallado posterior:

1. **Mayor complejidad operativa y de despliegue.** Se incorpora un componente de infraestructura adicional que debe aprovisionarse, configurarse y monitorearse.
2. **Consistencia eventual.** Existe una ventana durante la cual un evento ya ocurrió pero todavía no se refleja en lo que el dashboard muestra al familiar o al cuidador.
3. **Depuración distribuida.** Seguir el recorrido de una alerta requiere correlacionar registros de varios contenedores, lo que obliga a propagar un identificador de correlación a lo largo de todo el flujo.
4. **Entrega *at-least-once*.** El intermediario garantiza que el mensaje se entrega al menos una vez, no exactamente una vez. Los consumidores deben ser idempotentes y la deduplicación es obligatoria, no opcional.
5. **Tensión con QA-04.** La circulación del dato personal a través de tópicos amplía la superficie donde ese dato reside respecto de un diseño monolítico. Se mitiga mediante minimización —el evento transporta identificadores y no datos clínicos—, segregación de tópicos según sensibilidad, y cifrado en tránsito y en reposo.

### 8.5 Trazabilidad hacia las decisiones registradas

La elección del estilo arquitectónico se registra como decisión formal en ADR-001 (sección 9), con el detalle de contexto, alternativas evaluadas y consecuencias. La selección del producto concreto que materializa ese estilo es una decisión de tecnología subordinada al estilo, y no al revés: el diseño se sostiene sobre cualquier intermediario durable con entrega garantizada, cola de mensajes muertos y suscripción múltiple. La tecnología elegida y sus características se detallan en la vista de contenedores (sección 7.2.1).

---

## 9. Registro de decisiones — ADRs
Las siguientes decisiones documentan los aspectos arquitectónicos que tienen mayor impacto sobre el cumplimiento de los drivers funcionales, atributos de calidad y restricciones identificados en las secciones 3 y 4.

Cada ADR indica explícitamente los drivers que origina la decisión y los escenarios de calidad que permiten validarla. De esta forma, las decisiones arquitectónicas no se presentan como elecciones tecnológicas aisladas, sino como respuestas concretas a los requerimientos prioritarios de SeniorCareHub.

### Resumen de decisiones
Se documentaron cuatro decisiones arquitectónicas significativas que afectan la estructura del pipeline crítico de SeniorCareHub, la configurabilidad del motor de reglas, el manejo de falsas alarmas, la confiabilidad de entrega de eventos y el desacoplamiento del envío de notificaciones. Cada ADR detalla el contexto, la decisión tomada, las alternativas evaluadas y sus consecuencias.

| ADR | Título | Estado | Drivers atendidos |
|---|---|---|---|
| [ADR-001](/decisiones/ADR-001-separar-ingesta-evaluacion-alertas-notificaciones.md) | Separar ingesta, evaluación, alertas y notificaciones mediante eventos | Propuesta | RF-01, RF-03, QA-01, QA-02, QA-03, REST-01 |
| [ADR-002](/decisiones/ADR-002-motor-reglas-configurable-perfiles-versionados.md) | Implementar un motor de reglas configurable y perfiles versionados | Propuesta | RF-02, RF-05, QA-02, QA-05 |
| [ADR-003](/decisiones/ADR-003-gestion-falsas-alarmas-correlacion-confirmacion.md) | Gestionar falsas alarmas mediante correlación, confirmación y deduplicación | Propuesta | RF-02, RF-05, QA-02, QA-03 |
| [ADR-004](/decisiones/ADR-004-notificaciones-canales-configurables-adaptadores.md) | Desacoplar las notificaciones mediante canales configurables y adaptadores | Propuesta | RF-03, RF-05, QA-02, QA-05, REST-01 |

---

### ADR-001: Separar ingesta, evaluación, alertas y notificaciones mediante eventos

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |
| **Drivers atendidos** | RF-01, RF-03, QA-01, QA-02, QA-03, REST-01 |
| **Escenarios relacionados** | QS-01, QS-02, QS-03 |

#### Contexto

SeniorCareHub debe recibir continuamente eventos simulados de monitoreo, evaluarlos mediante reglas configurables, generar alertas cuando se confirma una situación crítica y despacharlas mediante proveedores externos.

Estas actividades presentan características y ritmos distintos. La recepción de eventos debe continuar aunque el motor de reglas se encuentre temporalmente saturado, y la evaluación de eventos no debe detenerse por la indisponibilidad de un proveedor de SMS o correo electrónico.

Una cadena de llamadas sincrónicas entre recepción, evaluación y notificación provocaría que el fallo de un componente se propagara al resto del pipeline. Además, obligaría a que todos los componentes estuvieran disponibles al mismo tiempo para poder procesar un evento.

#### Decisión

Se decide dividir el pipeline crítico en cuatro responsabilidades principales:

- **Servicio de Ingesta**, responsable de validar y aceptar eventos.
- **Motor de Reglas**, responsable de evaluar los eventos y determinar si deben generar una alerta, responsable de registrar la alerta y controlar su estado.
- **Servicio de Notificaciones**, responsable de seleccionar canales, invocar proveedores y registrar los intentos de entrega.

La comunicación entre estas etapas se realizará de manera asíncrona mediante un intermediario de mensajería durable basado en publicación y suscripción.

El productor no dependerá de que el consumidor se encuentre disponible en el instante en que se publica el evento. Los mensajes permanecerán almacenados hasta que puedan ser procesados o trasladados a una cola de mensajes fallidos.

#### Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Monolito con procesamiento sincrónico | Menor complejidad operativa y menor latencia interna | Un fallo en reglas o notificaciones afecta la recepción; los reintentos dependen del proceso | No satisface adecuadamente QA-01 y QA-03 |
| Servicios separados mediante REST sincrónico | Permite desplegar servicios independientes | Acoplamiento temporal, propagación de fallos y necesidad de que todos los servicios estén disponibles | Mantiene los principales riesgos de pérdida e indisponibilidad |
| Procesamiento orientado a eventos | Desacoplamiento temporal, aislamiento de fallos y almacenamiento durable | Mayor complejidad de despliegue, consistencia eventual y depuración distribuida | **Alternativa seleccionada** |

#### Consecuencias

**Positivas**
- La ingesta puede continuar aunque el Motor de Reglas o el Servicio de Notificaciones estén temporalmente indisponibles.
- Los eventos pueden conservarse y reprocesarse.
- Los componentes pueden desplegarse y escalarse de forma independiente.
- La integración con nuevos consumidores no requiere modificar al productor original.
- Los fallos de proveedores externos quedan aislados del procesamiento central.

**Negativas**
- Se introduce infraestructura adicional de mensajería.
- El sistema opera con consistencia eventual.
- Es necesario propagar identificadores de correlación entre componentes.
- La observabilidad y depuración requieren correlacionar registros distribuidos.
- Los consumidores deben manejar mensajes duplicados.

#### Evidencia y validación

- **Vista:** sección 7.2, vista de contenedores.
- **Flujo:** procesamiento de un evento crítico en la sección 10.1.4.
- **Prueba prevista:** detener temporalmente el Motor de Reglas mientras el Servicio de Ingesta continúa recibiendo eventos.
- **Resultado esperado:** los eventos permanecen disponibles en el intermediario y son procesados cuando el consumidor se recupera.

#### Revisión requerida si

- La latencia adicional del intermediario impide cumplir QS-02.
- La operación del sistema no puede asumir la complejidad de una plataforma distribuida.
- El volumen real de eventos resulta suficientemente pequeño y tolerante a fallos como para justificar una arquitectura más simple.

---

### ADR-002: Implementar un motor de reglas configurable y perfiles versionados

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-02, RF-05, QA-02, QA-05 |
| **Escenarios relacionados** | QS-02, QS-05 |

#### Contexto

Los criterios que determinan si un evento representa una situación crítica varían entre adultos mayores. Por ejemplo, el tiempo máximo de inactividad, las zonas consideradas seguras, la criticidad de una caída y los destinatarios de una alerta pueden depender del perfil individual.

Estas reglas deben poder modificarse durante la operación normal sin recompilar ni redesplegar el Motor de Reglas. Sin embargo, permitir reglas completamente arbitrarias aumentaría considerablemente la complejidad, los riesgos de seguridad y la dificultad para garantizar la latencia de procesamiento.

#### Decisión

Se decide implementar un Motor de Reglas que combine:

- Un conjunto controlado de tipos de reglas soportadas.
- Parámetros configurables almacenados en la BD Operativa.
- Perfiles de monitoreo individuales.
- Versionado de reglas y perfiles.
- Validación previa de las configuraciones.
- Evaluadores especializados detrás de una interfaz común.
- Registro de la versión utilizada en cada evaluación.

Los cambios sin redespliegue se limitarán a parámetros y combinaciones de reglas conocidas por el motor, tales como:

- Tiempo máximo de inactividad.
- Radio de una zona segura.
- Ventana de confirmación.
- Nivel de criticidad.
- Número de eventos requeridos para confirmar una condición.
- Canales y destinatarios asociados.

La incorporación de un nuevo tipo de regla o algoritmo requerirá implementación, pruebas y despliegue de un nuevo evaluador.

#### Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Reglas codificadas directamente | Simplicidad y alto rendimiento | Cada cambio requiere modificar código y redesplegar | Incumple QA-05 |
| Motor de reglas completamente dinámico o DSL arbitrario | Máxima flexibilidad | Mayor complejidad, riesgos de seguridad y dificultad de validación | Sobredimensionado para el alcance |
| Tipos de reglas controlados con parámetros configurables | Equilibrio entre modificabilidad, control y rendimiento | Los nuevos algoritmos requieren despliegue | **Alternativa seleccionada** |

#### Consecuencias

**Positivas**
- Los administradores pueden modificar parámetros sin intervención del equipo de desarrollo.
- Cada adulto mayor puede tener un perfil distinto.
- Las evaluaciones quedan asociadas a una versión concreta.
- La validación de configuraciones reduce errores operativos.
- Los evaluadores pueden extenderse sin modificar la ingesta ni las notificaciones.

**Negativas**
- Debe mantenerse compatibilidad con versiones anteriores de reglas.
- El Motor de Reglas necesita mecanismos de caché o actualización de configuración.
- Las reglas inválidas deben rechazarse antes de entrar en operación.
- La flexibilidad introduce un costo adicional de evaluación.

#### Evidencia y validación

- **Componente detallado principal:** Motor de Reglas y Generación de Alertas.
- **Patrón previsto:** Strategy para seleccionar el evaluador correspondiente al tipo de regla.
- **Prueba prevista:** modificar el umbral de inactividad durante la operación y enviar eventos antes y después del cambio.
- **Resultado esperado:** los eventos posteriores utilizan la nueva versión sin redesplegar el servicio.

#### Revisión requerida si

- Los usuarios necesitan expresar reglas arbitrarias no cubiertas por los evaluadores disponibles.
- El número de tipos de regla crece hasta hacer difícil mantener evaluadores independientes.
- La evaluación dinámica impide cumplir la latencia de QS-02.

---
### ADR-003: Gestionar falsas alarmas mediante correlación, confirmación y deduplicación

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-02, RF-05, QA-02, QA-03 |
| **Escenarios relacionados** | QS-02, QS-03, QS-05 |

#### Contexto

Generar una alerta por cada evento recibido podría producir notificaciones duplicadas o falsas alarmas. Por ejemplo, múltiples eventos de movimiento pueden representar una misma caída, y una pérdida temporal de comunicación puede recuperarse antes de requerir intervención.

No obstante, esperar demasiado tiempo para confirmar una condición reduce las falsas alarmas, pero incrementa la latencia de las alertas verdaderamente críticas.

#### Decisión

Se decide incorporar en el Motor de Reglas una etapa de confirmación configurable que considere:

- Correlación de eventos relacionados.
- Ventanas temporales de confirmación.
- Deduplicación por adulto mayor, tipo de evento y período.
- Estado temporal de evaluación por persona monitoreada.
- Niveles de confianza o criticidad.
- Políticas diferenciadas según el tipo de evento.

Los eventos de criticidad inmediata podrán generar una alerta sin esperar una ventana adicional cuando la regla configurada así lo determine. Los eventos ambiguos podrán requerir confirmación mediante eventos posteriores o el cumplimiento de una duración mínima.

Una alerta confirmada deberá incluir una referencia a los eventos que la originaron y a la versión de reglas utilizada.

#### Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Alertar por cada evento individual | Mínima latencia y lógica sencilla | Fatiga de alarmas, duplicados y falsas alertas | No atiende adecuadamente el problema central |
| Confirmación manual antes de notificar | Reduce alertas incorrectas | Requiere supervisión humana permanente y puede retrasar emergencias | No satisface QS-02 |
| Correlación y confirmación configurable | Equilibra latencia y reducción de falsas alarmas | Requiere estado temporal y aumenta complejidad | **Alternativa seleccionada** |

#### Consecuencias

**Positivas**
- Disminuye la generación de notificaciones duplicadas.
- Permite adaptar la confirmación a la criticidad del evento.
- Mejora la trazabilidad entre eventos y alertas.
- Reduce el riesgo de fatiga de alarmas para cuidadores y familiares.

**Negativas**
- Mantener estado temporal incrementa la complejidad del Motor de Reglas.
- Una ventana de confirmación excesiva puede retrasar alertas reales.
- Es necesario definir cómo recuperar el estado después de un reinicio.
- Se requieren métricas para evaluar falsos positivos y falsos negativos.

#### Evidencia y validación

- **Componente:** Motor de Reglas y Generación de Alertas.
- **Flujo de comportamiento:** correlación de eventos y confirmación de alerta.
- **Prueba prevista:** simular eventos duplicados, pérdida breve de comunicación y una caída confirmada.
- **Resultado esperado:** los duplicados producen una única alerta; el evento transitorio no genera una alerta crítica; la caída confirmada respeta la meta de latencia.

#### Revisión requerida si

- Las ventanas de confirmación provocan incumplimientos repetidos de QS-02.
- El dominio requiere modelos probabilísticos o aprendizaje automático para reducir falsas alarmas.
- Las métricas muestran que las reglas configurables no alcanzan la exactitud necesaria.

---
### ADR-004: Desacoplar las notificaciones mediante canales configurables y adaptadores

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-03, RF-05, QA-02, QA-05, REST-01 |
| **Escenarios relacionados** | QS-02, QS-05 |

#### Contexto

SeniorCareHub debe notificar a familiares y cuidadores mediante distintos canales, como SMS, correo electrónico o mensajería. Los proveedores pueden cambiar, utilizar contratos diferentes o no estar disponibles en todos los entornos.

Acoplar el Motor de Reglas directamente a un proveedor dificultaría incorporar nuevos canales y haría que los cambios de integración afectaran la lógica de detección.

#### Decisión

Se decide implementar un Servicio de Notificaciones independiente que:

- Reciba alertas confirmadas.
- Consulte el perfil de notificación.
- Resuelva destinatarios y canales.
- Ordene los canales según prioridad.
- Seleccione un adaptador compatible con cada canal.
- Registre cada intento y resultado de entrega.
- Permita incorporar nuevos adaptadores sin modificar el Motor de Reglas.

Cada proveedor externo se ubicará detrás de una interfaz interna estable. La selección de canales y proveedores se determinará mediante configuración.

#### Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Integrar proveedores dentro del Motor de Reglas | Menos componentes | Alto acoplamiento y propagación de fallos | Contradice RF-03 y QA-05 |
| Un servicio independiente por proveedor | Máximo aislamiento | Mayor costo operativo y duplicación de lógica | Complejidad innecesaria para el alcance |
| Servicio multicanal con adaptadores | Aislamiento, reutilización y extensibilidad | El servicio concentra coordinación de varios canales | **Alternativa seleccionada** |

#### Consecuencias

**Positivas**
- Los cambios de proveedor no afectan la detección de eventos.
- Pueden agregarse nuevos canales mediante adaptadores.
- Las preferencias se administran por perfil.
- La lógica común de seguimiento y auditoría se mantiene centralizada.

**Negativas**
- El Servicio de Notificaciones puede convertirse en un componente complejo.
- Deben normalizarse respuestas diferentes de proveedores.
- Los proveedores pueden tener límites y semánticas de entrega distintas.
- La configuración de prioridad debe validarse.

#### Evidencia y validación

- **Patrones previstos:** Adapter para proveedores y Strategy para selección de canal.
- **Prueba prevista:** incorporar un proveedor simulado nuevo sin modificar el Motor de Reglas.
- **Resultado esperado:** el nuevo adaptador puede seleccionarse mediante configuración.

#### Revisión requerida si

- La cantidad de canales o el volumen de notificaciones requiere separar cada canal en un servicio independiente.
- Un proveedor exige un modelo de integración incompatible con la interfaz común.
- Se requiere enviar simultáneamente por todos los canales en lugar de utilizar prioridad.

---

# BLOQUE 5 — DISEÑO DETALLADO
*Hito: Entrega final (S14)*

---

## 10. Diseño detallado de componentes

### Componente 1 — Motor de reglas

**Responsabilidad:** Analizar los eventos recibidos desde el Bus de Mensajería, evaluarlos de acuerdo con las reglas configurables y el perfil de monitoreo del adulto mayor asociado, determinar si representan una situación de riesgo, establecer su nivel de criticidad y generar una alerta cuando corresponda, aplicando mecanismos de confirmación, deduplicación e idempotencia para evitar falsas alarmas y el procesamiento repetido de un mismo evento.

**Trazabilidad:** Soporta los casos de uso definidos en la Sección 1.4 relacionados con Monitorear múltiples adultos mayores, Recibir alertas de emergencia en tiempo real, Ajustar perfiles de monitoreo y Configurar reglas y parámetros generales de monitoreo. Asimismo, implementa los requerimientos funcionales RF-01 (Recepción y procesamiento de eventos), RF-02 (Evaluación mediante reglas configurables) y RF-05 (Aplicación de perfiles de monitoreo individuales). En la Vista de estructura interna presentada en la Sección 7.2, corresponde al Contenedor 5 – Motor de Reglas, responsable de consumir eventos desde el Bus de Mensajería, evaluarlos según las reglas y perfiles configurados, generar alertas y publicarlas nuevamente en el Bus de Mensajería.

#### 10.1.1 Diagrama de clases de diseño

El siguiente diagrama presenta el diseño interno del componente Motor de Reglas. Se muestran las principales clases, interfaces y relaciones de colaboración que lo conforman, así como la aplicación de los patrones de diseño Strategy, Factory, Repository y Adapter para desacoplar las responsabilidades del componente y facilitar su extensibilidad y mantenimiento.

El diseño se organiza alrededor de `RuleProcessingService`, responsable de coordinar el flujo completo de procesamiento. El procesamiento inicia en `EventMessageConsumer`, que recibe los eventos desde el Bus de Mensajería y delega su procesamiento mediante la interfaz `IRuleProcessingService`. A partir de este punto, `RuleProcessingService` coordina la validación del evento, la recuperación del perfil de monitoreo y de las reglas activas correspondientes a la versión del perfil, la evaluación mediante estrategias especializadas, la aplicación de los mecanismos de confirmación y deduplicación, la persistencia del estado temporal cuando la regla lo requiere, la generación y almacenamiento de la alerta, y finalmente su publicación mediante el mecanismo de mensajería configurado.

![Diagrama de clases — Componente 1](../diagramas/clases-componente1.png)
*Figura 6 — Diagrama de clases de diseño: Motor de Reglas*
> **Imagen en tamaño completo:** [`clases-componente1.png`](../diagramas/clases-componente1.png) · **Fuente editable:** [`clases-componente1.mmd`](../diagramas/clases-componente1.mmd)

#### 10.1.2 Contratos de interfaz

El punto de entrada técnico del componente corresponde a EventMessageConsumer.HandleAsync(message): Task, invocado por el Bus de Mensajería para iniciar el procesamiento de cada evento recibido. Este consumidor delega el procesamiento al servicio principal del componente mediante ProcessAsync(event: MonitoringEvent): Task<ProcessingResult>. Adicionalmente, se documentan los contratos de las principales interfaces internas de colaboración del componente, ya que definen las responsabilidades, precondiciones, garantías y condiciones de error entre los elementos que participan en el flujo de procesamiento. Estas operaciones no representan endpoints externos del sistema, sino contratos internos entre los colaboradores del Motor de Reglas.

| Método / Endpoint | Precondición | Postcondición | Excepciones |
|---|---|---|---|
| `EventMessageConsumer.`<br>`HandleAsync`<br>`(message): Task` | El parámetro `message` no debe ser nulo. | El mensaje ha sido recibido, transformado en un `MonitoringEvent` y su procesamiento ha sido delegado mediante `IRuleProcessingService`. Si el procesamiento concluye satisfactoriamente, el mensaje se confirma al Bus. En caso de una falla técnica que impida completar el procesamiento de forma segura, el mensaje no se confirma y queda disponible para su reentrega conforme a la política de mensajería. | Puede producir una excepción cuando el mensaje no puede ser interpretado o transformado en un `MonitoringEvent`, cuando se cancela la operación o cuando ocurre una falla técnica inesperada durante el procesamiento o la comunicación con el Bus. |
| `IRuleProcessingService.`<br>`ProcessAsync`<br>`(event: MonitoringEvent): `<br>`Task<ProcessingResult>`| El parámetro `event` no debe ser nulo. | El evento ha sido procesado, incluyendo las etapas aplicables de validación, recuperación del perfil asociado a `event.OlderAdultId` y `event.ProfileVersion` y las reglas activas aplicables. Las reglas se evalúan mediante la estrategia correspondiente, se aplican los mecanismos de confirmación y deduplicación y, cuando la situación de riesgo resulta confirmada y no duplicada, se genera, persiste y publica una `Alert`. Se devuelve un `ProcessingResult` que representa el estado final del procesamiento. Las condiciones esperadas, como evento inválido, perfil inexistente, ausencia de reglas aplicables, condición no confirmada o duplicado detectado, se representan mediante el resultado y no mediante excepciones. | Puede producir una excepción ante una falla técnica al consultar o persistir información, durante la publicación de la alerta, ante la cancelación de la operación o cuando una inconsistencia interna impida completar el procesamiento de forma segura. |
| `IEventValidator.`<br>`ValidateAsync`<br>`(event: MonitoringEvent):`<br>`Task<EventValidationResult>` | El parámetro `event` no debe ser nulo. | El evento ha sido evaluado para verificar que contiene la información mínima y válida requerida para continuar con el procesamiento. Se devuelve un `EventValidationResult` que indica si el evento es válido y, cuando corresponde, los errores de validación encontrados. | Puede producir una excepción ante una falla técnica inesperada durante la validación o ante la cancelación de la operación. Los eventos con información incompleta o inválida se representan mediante `EventValidationResult` y no mediante excepciones. |
| `IMonitoringProfileRepository.`<br>`GetByOlderAdultIdAsync`<br>`(olderAdultId: Guid,`<br>` profileVersion: int):`<br>`Task<MonitoringProfile?>` | El parámetro `olderAdultId` no debe ser nulo y `profileVersion` debe representar una versión válida. | Se consulta la fuente de datos y se devuelve el `MonitoringProfile` correspondiente al adulto mayor y a la versión especificada. Si no existe un perfil asociado, se devuelve null. | Puede producir una excepción ante una falla técnica durante el acceso al repositorio, la cancelación de la operación o una falla inesperada. La ausencia de la versión solicitada del perfil se representa mediante el valor de retorno y no se considera una condición excepcional. |
| `IMonitoringRuleRepository.`<br>`GetActiveRulesAsync`<br>`(profileId: Guid, `<br>`profileVersion: int,`<br>`eventType: EventType):`<br>` Task<MonitoringRuleCollection>` | Los parámetros `profileId` y `eventType` no deben ser nulos, y `profileVersion` debe representar una versión válida. | Se consulta la fuente de datos y se devuelve la colección de `MonitoringRule` activas y aplicables asociadas al perfil, la versión y el tipo de evento especificados. Si no existen reglas aplicables, se devuelve una colección vacía. | Puede producir una excepción ante una falla técnica durante el acceso al repositorio, la cancelación de la operación o una falla inesperada. La ausencia de reglas aplicables se representa mediante una colección vacía y no se considera una condición excepcional. |
| `IRuleEvaluatorFactory.`<br>`Create`<br>`(ruleType: RuleType):`<br>`IRuleEvaluator` | El parámetro `ruleType` debe corresponder a un valor válido de `RuleType`. | Se selecciona y devuelve una implementación de `IRuleEvaluator` compatible con el tipo de regla especificado. | Puede producir una excepción cuando no existe un evaluador compatible con el tipo de regla solicitado, cuando la configuración resulta ambigua porque más de un evaluador atiende el mismo tipo de regla o cuando ocurre una falla inesperada durante la selección.|
| `IRuleEvaluator.Supports`<br>`(ruleType: RuleType):bool` | El parámetro `ruleType` debe corresponder a un valor válido de `RuleType`. | Se devuelve `true` cuando el evaluador es compatible con el tipo de regla especificado; de lo contrario, se devuelve `false`. | Puede producir una excepción ante una falla inesperada durante la determinación de compatibilidad. |
| `IRuleEvaluator.Evaluate`<br>`(event: MonitoringEvent, `<br>`rule: MonitoringRule,`<br>`profile: MonitoringProfile):`<br>`RuleEvaluationResult` | Los parámetros `event`, `rule` y `profile` no deben ser nulos. | El evento ha sido evaluado mediante la estrategia correspondiente, aplicando la regla y el perfil de monitoreo proporcionados. Se devuelve un `RuleEvaluationResult` que indica si la condición definida por la regla se cumple y la razón del resultado. | Puede producir una excepción ante una falla técnica inesperada durante la evaluación. Cuando la condición definida por la regla no se cumple, el resultado de la evaluación se representa mediante `RuleEvaluationResult` y no mediante una excepción.|
| `IConfirmationService.`<br>`ConfirmAsync`<br>`(event: MonitoringEvent, `<br>`rule: MonitoringRule, `<br>`evaluation: RuleEvaluationResult):`<br>`Task<ConfirmationResult>` | Los parámetros `event`, `rule` y `evaluation` no deben ser nulos. | Se aplican los criterios de confirmación definidos por la regla y, cuando corresponde, se consulta y actualiza el estado temporal asociado al adulto mayor, la regla y la versión del perfil evaluada. Se devuelve un `ConfirmationResult` que indica si la situación de riesgo ha sido confirmada y, cuando corresponde, incluye la razón del resultado y las referencias a los eventos que participaron en la confirmación. | Puede producir una excepción ante una falla técnica durante la consulta o actualización del estado temporal, ante una falla inesperada durante el proceso de confirmación o ante la cancelación de la operación. Una situación de riesgo no confirmada, una ventana aún pendiente o una ventana vencida se representan mediante `ConfirmationResult` y no mediante excepciones. |
| `IConfirmationStateRepository.`<br>`GetAsync`<br>`(olderAdultId: Guid, ruleId: Guid, `<br>`profileVersion: int):`<br>`Task<ConfirmationState?>` | Los parámetros `olderAdultId` y `ruleId` deben ser identificadores válidos, y `profileVersion` debe representar una versión válida.| Se consulta la fuente de datos y se devuelve el `ConfirmationState` asociado al adulto mayor, la regla y la versión del perfil especificados. Si no existe un estado temporal pendiente, se devuelve `null`. | Puede producir una excepción ante una falla técnica durante el acceso al repositorio, ante la cancelación de la operación o ante una falla inesperada. La ausencia de un estado temporal pendiente se representa mediante `null` y no se considera una condición excepcional. |
| `IConfirmationStateRepository.`<br>`SaveAsync`<br>`(state: ConfirmationState):`<br>`Task` | El parámetro `state` no debe ser nulo. | El `ConfirmationState` se almacena o actualiza de forma durable y queda disponible para continuar la evaluación en eventos posteriores o después de un reinicio del componente. | Puede producir una excepción ante una falla técnica durante la persistencia del estado, ante la cancelación de la operación o ante una falla inesperada. |
| `IDeduplicationService.`<br>`IsDuplicateAsync`<br>`(eventId: Guid,`<br>`olderAdultId: Guid,`<br>`alertType: AlertType): Task<bool>` | Los parámetros `eventId`, `olderAdultId` deben ser identificadores válidos y `alertType` debe corresponder a un valor válido de `AlertType`. | Se verifica si el evento ya fue procesado o si existe una alerta activa equivalente para el mismo adulto mayor y tipo de alerta. Se devuelve `true` cuando se detecta un duplicado; en caso contrario, se devuelve `false`. | Puede producir una excepción ante una falla técnica durante la consulta de la información necesaria para determinar la existencia de un duplicado, ante la cancelación de la operación o ante una falla inesperada. La detección o ausencia de un duplicado se representa mediante el valor de retorno y no mediante excepciones. |
| `IAlertRepository.`<br>`ExistsBySourceEventIdAsync`<br>`(eventId: Guid): Task<bool>` | El parámetro `eventId` debe ser un identificador válido. | Se devuelve `true` cuando existe una `Alert` que incluye el identificador del evento especificado entre sus eventos de origen; de lo contrario, se devuelve `false`. | Puede producir una excepción ante una falla técnica durante la consulta al repositorio, ante la cancelación de la operación o ante una falla inesperada. La ausencia de una alerta asociada se representa mediante el valor `false` y no mediante una excepción. |
| `IAlertRepository.`<br>`ExistsActiveEquivalentAsync`<br>`(olderAdultId: Guid, `<br>`alertType: AlertType):`<br>`Task<bool>` | El parámetro `olderAdultId` debe ser un identificador válido y `alertType` debe corresponder a un valor válido de `AlertType`. | Se devuelve `true` cuando existe una `Alert` activa equivalente para el mismo adulto mayor y tipo de alerta, dentro del período de deduplicación aplicable; de lo contrario, se devuelve `false`. | Puede producir una excepción ante una falla técnica durante la consulta al repositorio, ante la cancelación de la operación o ante una falla inesperada. La ausencia de una alerta activa equivalente se representa mediante el valor `false` y no mediante una excepción. |
| `IAlertRepository.SaveAsync`<br>`(alert: Alert): Task` | El parámetro `alert` no debe ser nulo. | La `Alert` se almacena de forma durable y queda disponible para su consulta, deduplicación y publicación. | Puede producir una excepción ante una falla técnica durante la persistencia de la alerta, ante la cancelación de la operación o ante una falla inesperada.|
| `IAlertPublisher.PublishAsync`<br>`(alert: Alert): Task` | El parámetro `alert` no debe ser nulo. | La alerta se entrega al mecanismo de publicación configurado y se solicita su publicación hacia el Bus de Mensajería. | Puede producir una excepción ante una falla técnica durante la preparación o publicación de la alerta, ante la cancelación de la operación o ante una falla inesperada. |

#### 10.1.3 Análisis de robustez

El análisis de robustez permite verificar que el diseño del Motor de Reglas contempla los objetos necesarios para recibir información desde elementos externos, coordinar el procesamiento del evento y representar los datos utilizados durante la evaluación. La siguiente tabla clasifica los principales objetos que intervienen en el flujo de procesamiento de eventos como Boundary, Control o Entity. En este contexto, los repositorios se consideran objetos Boundary, ya que representan la interacción entre la lógica del componente y los mecanismos externos de persistencia.

| Objeto | Tipo | Responsabilidad |
|---|---|---|
| EventMessageConsumer          | Boundary | Recibir los mensajes provenientes del Bus de Mensajería, transformarlos en objetos `MonitoringEvent` y delegar su procesamiento mediante `IRuleProcessingService`.       |
| IAlertPublisher               | Boundary | Definir el contrato para publicar las alertas confirmadas mediante el mecanismo de mensajería configurado, independientemente de la tecnología concreta utilizada. |
| IMonitoringProfileRepository  | Boundary | Proporcionar acceso a la configuración del perfil de monitoreo correspondiente al adulto mayor y a la versión asociada al evento. |
| IMonitoringRuleRepository     | Boundary | Proporcionar acceso a las reglas activas aplicables al perfil, su versión y el tipo de evento recibido |
| IAlertRepository              | Boundary | Proporcionar acceso a las alertas existentes para apoyar la deduplicación y almacenar las alertas generadas por el componente. |
| IConfirmationStateRepository  | Boundary | Proporcionar acceso persistente al estado temporal utilizado para correlacionar eventos, mantener y actualizar las ventanas de confirmación y recuperar evaluaciones pendientes después de un reinicio. |
| RuleProcessingService         | Control  | Coordinar el flujo completo de procesamiento, incluyendo validación, recuperación del perfil y las reglas aplicables, evaluación, confirmación, deduplicación, generación, persistencia y publicación de alertas. |
| EventValidator                | Control  | Verificar que el evento recibido contenga la información mínima y válida requerida para continuar con el procesamiento. |
| RuleEvaluatorFactory          | Control  | Seleccionar la estrategia de evaluación correspondiente según el tipo de regla configurada. |
| FallRuleEvaluator             | Control  | Evaluar las reglas relacionadas con posibles caídas. |
| InactivityRuleEvaluator       | Control  | Evaluar las reglas asociadas con periodos de inactividad. |
| SafeZoneRuleEvaluator         | Control  | Evaluar si el adulto mayor se encuentra fuera de la zona segura configurada. |
| ConfirmationService           | Control  | Aplicar los criterios de confirmación definidos por la regla, consultar y actualizar el estado temporal cuando corresponda y determinar si la condición de riesgo ha sido confirmada. |
| DeduplicationService          | Control  | Verificar si el evento ya fue procesado o si existe una alerta equivalente para el mismo adulto mayor y tipo de alerta dentro del período de deduplicación aplicable, evitando alertas duplicadas. |
| IRuleEvaluator                | Control  | Definir el contrato común para evaluar un evento según una regla y devolver el resultado normalizado de la evaluación. |
| MonitoringEvent               | Entity   | Representar el evento de monitoreo recibido, incluyendo su identificador, tipo, fecha de ocurrencia, versión del perfil y adulto mayor asociado. |
| MonitoringProfile             | Entity   | Representar la configuración de monitoreo correspondiente al adulto mayor y a una versión específica del perfil. |
| MonitoringRule                | Entity   | Representar las reglas configurables y las condiciones utilizadas para evaluar los eventos de monitoreo. |
| Alert                         | Entity   | Representar una alerta generada por el componente, incluyendo la regla y la versión del perfil aplicadas, los eventos que originaron su confirmación y la información necesaria para su almacenamiento y publicación. |
| ConfirmationState             | Entity   | Representar el estado temporal persistente de una evaluación para un adulto mayor, una regla y una versión específica del perfil, incluyendo la ventana de confirmación, el último evento relacionado, los identificadores y la cantidad de eventos coincidentes, y el estado actual del proceso.  |

#### 10.1.4 Diagrama de secuencia — flujo principal

El siguiente diagrama de secuencia representa el flujo principal de la interacción entre los principales objetos del componente Motor de Reglas, desde la recepción del evento hasta la generación, almacenamiento y publicación de una alerta cuando se detecta una situación de riesgo. El componente obtiene la versión correspondiente del perfil de monitoreo y las reglas activas aplicables, evalúa el evento mediante la estrategia correspondiente y aplica los criterios de confirmación definidos. Cuando la regla lo requiere, consulta y actualiza el estado temporal de confirmación para correlacionar eventos relacionados y determinar si la condición de riesgo ha sido confirmada. Posteriormente, verifica que no exista un procesamiento duplicado y, cuando corresponde, genera la alerta a partir de la regla confirmada y de los eventos que participaron en la confirmación, la almacena y la publica en el Bus de Mensajería.

![Secuencia — Componente 1, flujo principal](../diagramas/secuencia-comp1-principal.png)
*Figura 4 — Secuencia: Procesamiento de un evento con generación de alerta*
> **Imagen en tamaño completo:** [`secuencia-comp1-principal.png`](../diagramas/secuencia-comp1-principal.png) · **Fuente editable:** [`secuencia-comp1-principal.mmd`](../diagramas/secuencia-comp1-principal.mmd)

El siguiente diagrama de secuencia representa un camino de error significativo en el que ocurre una falla técnica durante la persistencia del estado temporal de confirmación. El evento ha superado la validación, se han recuperado la versión correspondiente del perfil y las reglas aplicables, y la evaluación ha determinado que la condición requiere confirmación. Sin embargo, al intentar almacenar o actualizar el ConfirmationState, el repositorio produce una excepción. El procesamiento se detiene antes de verificar duplicados, generar, almacenar o publicar una alerta. Como el consumidor no confirma el mensaje al Bus de Mensajería, este permanece disponible para su reentrega mediante el mecanismo de reintentos de la infraestructura de mensajería.

![Secuencia — Componente 1, camino de error](../diagramas/secuencia-comp1-error.png)
*Figura 5 — Secuencia: Falla al persistir el estado temporal de confirmación*
> **Imagen en tamaño completo:** [`secuencia-comp1-error.png`](../diagramas/secuencia-comp1-error.png) · **Fuente editable:** [`secuencia-comp1-error.mmd`](../diagramas/secuencia-comp1-error.mmd)

---

### Componente 2 — Servicio de Notificaciones

**Responsabilidad:** Garantizar que las alertas confirmadas lleguen oportunamente a los familiares y cuidadores mediante los canales de comunicación configurados, gestionando la selección de destinatarios, la determinación de la política de entrega, la ejecución de la estrategia de notificación correspondiente, la comunicación con los proveedores externos y el registro del resultado de cada intento de notificación.

**Trazabilidad:** Soporta los casos de uso definidos en la Sección 1.4 relacionados con Recibir alertas de emergencia en tiempo real y Configurar preferencias de notificación. Asimismo, implementa los requerimientos funcionales RF-03 (Notificación de situaciones de riesgo) y RF-05 (Configuración y entrega de notificaciones). En la Vista de estructura interna presentada en la Sección 7.2, corresponde al Contenedor 6 – Servicio de Notificaciones, responsable de consumir las alertas confirmadas desde el Bus de Mensajería, resolver los destinatarios y canales de comunicación configurados, determinar la política de entrega aplicable según la severidad de la alerta y la configuración del perfil, ejecutar la estrategia de notificación correspondiente, enviarlas mediante los proveedores externos adecuados y registrar el resultado de cada intento de entrega.

#### 10.2.1 Diagrama de clases de diseño

El siguiente diagrama presenta el diseño interno del componente Servicio de Notificaciones. Se muestran las principales clases, interfaces y relaciones de colaboración que lo conforman, así como la aplicación de los patrones de diseño Strategy, Factory, Repository y Adapter para desacoplar las responsabilidades del componente, permitir la aplicación de diferentes políticas de entrega según el contexto de la alerta, facilitar la incorporación de nuevos canales y proveedores de notificación, y favorecer su mantenibilidad y extensibilidad.

El diseño se organiza alrededor de `NotificationProcessingService`, responsable de coordinar el flujo completo de procesamiento de una alerta confirmada. El procesamiento inicia en `AlertMessageConsumer`, que recibe las alertas desde el Bus de Mensajería y delega su procesamiento mediante la interfaz `INotificationProcessingService`. A partir de este punto, `NotificationProcessingService` coordina la recuperación de la configuración de notificación correspondiente a la versión del perfil asociada a la alerta, verifica el estado previo del procesamiento para garantizar la idempotencia, determina los destinatarios y canales aplicables mediante `ChannelSelectionService`, resuelve la estrategia de entrega más adecuada considerando la severidad de la alerta y la configuración del perfil mediante `DeliveryPolicyResolver`, delega la ejecución de los envíos a la estrategia correspondiente (`IChannelDeliveryStrategy`), la cual utiliza `NotificationAdapterFactory` para seleccionar los adaptadores apropiados y comunicarse con los proveedores externos. Finalmente, registra de forma durable el estado de cada notificación y sus intentos de entrega, y devuelve el resultado global del procesamiento al consumidor.

![Diagrama de clases — Componente 2](../diagramas/clases-componente2.png)
*Figura 6 — Diagrama de clases de diseño: Servicio de Notificaciones*
> **Imagen en tamaño completo:** [`clases-componente2.png`](../diagramas/clases-componente2.png) · **Fuente editable:** [`clases-componente2.mmd`](../diagramas/clases-componente2.mmd)

#### 10.2.2 Contratos de interfaz

| Método / Endpoint | Precondición | Postcondición | Excepciones |
|---|---|---|---|
| `AlertMessageConsumer.`<br>`HandleAsync`<br>`(message): Task` | `message` no debe ser nulo y debe contener una alerta confirmada serializada en el formato esperado, incluyendo los identificadores necesarios para su trazabilidad. El consumidor debe disponer de una implementación válida de `INotificationProcessingService`. | El mensaje se transforma en un objeto `Alert` y se delega su procesamiento mediante `ProcessAsync(alert)` en `INotificationProcessingService`. Si el resultado indica **ShouldAcknowledgeMessage = true**, el mensaje se confirma al Bus. Si indica **false**, el mensaje no se confirma, permitiendo su reentrega conforme a la política configurada. | Puede producir una excepción cuando el mensaje no puede deserializarse o mapearse a una alerta válida, cuando se cancela la operación o cuando ocurre una falla técnica inesperada durante el procesamiento o la comunicación con el Bus. |
| `INotificationProcessingService.`<br>`ProcessAsync`<br>`(alert: Alert): Task`<br>`<NotificationProcessingResult>` | `alert` no debe ser nula y debe contener identificadores válidos de alerta y adulto mayor, una versión de perfil positiva, un tipo y severidad reconocidos y la información mínima necesaria para construir la notificación. | Se consulta la configuración correspondiente a `alert.OlderAdultId` y `alert.ProfileVersion`; se procesa cada destinatario configurado; se determinan los canales aplicables mediante `ChannelSelectionService`; se resuelve la estrategia de entrega correspondiente considerando la severidad de la alerta y la configuración del perfil mediante `DeliveryPolicyResolver`; se ejecutan los envíos utilizando dicha estrategia; se evita repetir una entrega ya completada; se registran de forma durable las entidades `Notification` y `NotificationAttempt` que correspondan; y se devuelve un `NotificationProcessingResult` que indica el estado global de la ejecución, si el mensaje debe confirmarse al Bus y, cuando corresponda, la razón del resultado. Las condiciones esperadas, como perfil inexistente, configuración inválida, alerta ya procesada o necesidad de reintento, se representan mediante el resultado y no mediante excepciones. | Puede producir una excepción ante una falla técnica inesperada al consultar o persistir información, cuando no es posible garantizar el registro durable del resultado, cuando se cancela la operación o ante una inconsistencia interna que impida completar el procesamiento de forma segura. |
| `INotificationProfileRepository.`<br>`GetByOlderAdultIdAsync(`<br>`olderAdultId: Guid, `<br>`profileVersion: int):`<br>`Task<NotificationProfile?>` | `olderAdultId` debe ser un identificador válido y `profileVersion` debe ser mayor que cero. | Se consulta la fuente de datos y se devuelve la configuración de notificación asociada al adulto mayor y a la versión del perfil indicados, incluyendo los destinatarios, canales, prioridades, proveedores y la política de entrega configurada para el perfil. Si no existe una configuración correspondiente, se devuelve null; esta condición no se representa mediante una excepción. | Puede producir una excepción ante una falla técnica al consultar la fuente de datos, una inconsistencia al reconstruir la configuración almacenada o la cancelación de la operación. |
| `INotificationTrackingRepository.`<br>`GetByAlertAndRecipientAsync(`<br>`alertId: Guid,recipientId: Guid):`<br>`Task<Notification?>`| `alertId` y `recipientId` deben ser identificadores válidos. | Se consulta la fuente de datos y se devuelve la Notification previamente registrada para la combinación de alerta y destinatario, incluyendo su estado actual y la información necesaria para determinar si el procesamiento debe continuar. Si no existe un registro previo, se devuelve null; esta condición no se representa mediante una excepción. | Puede producir una excepción ante una falla técnica al consultar la fuente de datos, una inconsistencia al reconstruir la información persistida o la cancelación de la operación. |
| `ChannelSelectionService.`<br>`SelectApplicableChannels(`<br>`alert: Alert,`<br>`profile: NotificationProfile,`<br>`recipient: NotificationRecipient)`<br>`: NotificationChannelCollection` | `alert`, `profile` y `recipient` no deben ser nulos. El perfil debe corresponder al adulto mayor y a la versión asociada a la alerta. El destinatario debe corresponder a uno de los destinatarios definidos en `profile`. | Se devuelve una colección ordenada de configuraciones de canal aplicables para el destinatario, considerando el tipo y severidad de la alerta, la prioridad y el estado de los canales, los proveedores configurados, los datos de contacto disponibles y las restricciones definidas. La colección puede incluir múltiples configuraciones para un mismo tipo de canal cuando correspondan a distintos proveedores o niveles de prioridad. | Puede producir una excepción ante una configuración internamente inconsistente que impida evaluar los canales o ante una falla técnica inesperada durante la operación. |
| `DeliveryPolicyResolver.`<br>`ResolveStrategy(`<br>`alert: Alert,`<br>`profile: NotificationProfile)`<br>`: IChannelDeliveryStrategy` | `alert` y `profile` no deben ser nulos. El perfil debe corresponder al adulto mayor y a la versión indicada en la alerta. | Se devuelve una implementación de `IChannelDeliveryStrategy` compatible con la severidad de la alerta y la política de entrega configurada para el perfil. | Puede producir una excepción únicamente cuando existe una inconsistencia de configuración que impide determinar una estrategia válida o ante una falla técnica inesperada durante la resolución. |
| `NotificationAdapterFactory.`<br>`Create(`<br>`channelType: NotificationChannelType,`<br>`providerId: string)`<br>`: INotificationAdapter` | `channelType` debe corresponder a un valor válido de `NotificationChannelType` y `providerId` no debe ser nulo ni vacío. Debe existir un adaptador registrado capaz de atender la combinación indicada. | Se devuelve una implementación de `INotificationAdapter` compatible con el tipo de canal y el proveedor especificados. | Puede producir una excepción de configuración cuando no existe un adaptador compatible o cuando la configuración resulta ambigua porque más de un adaptador atiende la misma combinación. También puede producir una excepción ante una falla técnica inesperada durante la resolución de dependencias. |
| `IChannelDeliveryStrategy.`<br>`ExecuteAsync(`<br>`alert: Alert,`<br>`recipient: NotificationRecipient,`<br>`channels: NotificationChannelCollection)`<br>`: Task`<br>`<ChannelDeliveryResultCollection>` | `alert`, `recipient` y `channels` no deben ser nulos. La colección de canales debe contener únicamente configuraciones aplicables al destinatario y a la alerta. | Se ejecuta la política de entrega implementada por la estrategia utilizando los canales proporcionados y se devuelve un `ChannelDeliveryResultCollection` con el resultado normalizado de los intentos de entrega realizados por la estrategia. Los resultados esperados de los proveedores se representan mediante `ChannelDeliveryResultCollection` y no mediante excepciones. | Puede producir una excepción únicamente cuando ocurre una falla técnica inesperada que impide ejecutar la estrategia o completar el procesamiento. |
| `INotificationAdapter.`<br>`Supports(`<br>`channelType: NotificationChannelType,`<br>`providerId: string)`<br>`: bool` | `channelType` debe corresponder a un valor válido de `NotificationChannelType` y `providerId` no debe ser nulo ni vacío. | Se devuelve `true` cuando el adaptador puede atender la combinación de tipo de canal y proveedor especificados; en caso contrario, se devuelve `false`. | Puede producir una excepción únicamente ante una falla técnica inesperada al evaluar la configuración interna del adaptador. |
| `INotificationAdapter.`<br>`SendAsync(`<br>`alert: Alert,`<br>`recipient: NotificationRecipient,`<br>`channel: NotificationChannel)`<br>`: Task<AdapterSendResult>` | `alert`, `recipient` y `channel` no deben ser nulos. El adaptador debe ser compatible con el tipo de canal y el proveedor especificados en `channel`. El destinatario debe disponer de la información de contacto requerida para el canal seleccionado. | Se realiza el intento de envío mediante el proveedor externo y se devuelve un `AdapterSendResult` normalizado, incluyendo el estado de entrega y, cuando corresponda, el identificador externo y la información de la falla. Los resultados esperados del proveedor se representan mediante `AdapterSendResult` y no mediante excepciones. | Puede producir una excepción únicamente cuando ocurre una falla técnica inesperada que impide completar la comunicación con el proveedor o la ejecución del adaptador. |
| `INotificationTrackingRepository.`<br>`SaveAttemptAsync(`<br>`attempt: NotificationAttempt)`<br>`: Task` | `attempt` no debe ser nulo y debe estar asociado a una `Notification` y a un canal válidos. Debe incluir la fecha del intento, el proveedor utilizado, el estado de entrega y la información de correlación requerida. | Se registra de forma durable un nuevo `NotificationAttempt` asociado a la notificación, preservando el historial completo de intentos de entrega sin reemplazar los registros anteriores, independientemente de que el envío haya sido exitoso o fallido. | Puede producir una excepción ante una falla técnica durante la persistencia, una violación de integridad referencial, datos incompatibles con el modelo almacenado, la imposibilidad de garantizar la escritura durable o la cancelación de la operación. |
| `INotificationTrackingRepository.`<br>`SaveNotificationAsync(`<br>`notification: Notification):`<br>`Task` | `notification` no debe ser nula y debe contener una asociación válida con una alerta y un destinatario. Su estado debe representar un valor válido de `NotificationStatus`. | La entidad `Notification` se crea o actualiza de forma durable en la fuente de datos, preservando el estado actual del proceso de notificación, sus fechas relevantes y la información necesaria para su seguimiento. | Puede producir una excepción ante una falla técnica durante la persistencia, una violación de restricciones de integridad de los datos, la imposibilidad de garantizar la escritura durable o la cancelación de la operación. |

#### 10.2.3 Análisis de robustez

| Objeto | Tipo | Responsabilidad |
|---|---|---|
| `AlertMessageConsumer`            | Boundary | Recibir desde el Bus de Mensajería las alertas confirmadas, transformarlas en objetos `Alert` y delegar su procesamiento mediante `INotificationProcessingService`. |
| `INotificationProfileRepository`  | Boundary | Proporcionar acceso a la configuración de notificación asociada al perfil del adulto mayor, incluyendo destinatarios, datos de contacto, canales habilitados, prioridades, proveedores y la política de entrega configurada. |
| `INotificationTrackingRepository` | Boundary | Proporcionar acceso persistente al estado global de las notificaciones y a sus intentos de entrega, permitiendo consultar procesamientos previos, almacenar o actualizar una `Notification` y registrar cada `NotificationAttempt`. |
| `INotificationAdapter`            | Boundary | Definir la interfaz común para enviar una notificación mediante un proveedor externo y devolver un resultado normalizado, independientemente del contrato particular del proveedor. |
| `NotificationProcessingService`   | Control  | Coordinar el procesamiento completo de una alerta confirmada, verificando su idempotencia, obteniendo la configuración de notificación, resolviendo los destinatarios y canales aplicables, determinando la estrategia de entrega correspondiente según la severidad de la alerta y la configuración del perfil, delegando la ejecución de los envíos a dicha estrategia y registrando de forma durable el estado de las notificaciones y sus intentos de entrega. |
| `ChannelSelectionService`         | Control  | Determinar los canales de comunicación aplicables según el tipo y severidad de la alerta, las preferencias y prioridades definidas, los proveedores configurados, los datos de contacto disponibles y las restricciones de cada canal. |
| `DeliveryPolicyResolver` | Control | Determinar la estrategia de entrega aplicable considerando la severidad de la alerta y la política de entrega configurada para el perfil del adulto mayor. |
| `PriorityFallbackDeliveryStrategy` | Control | Ejecutar la entrega de la notificación utilizando los canales aplicables en orden de prioridad, recurriendo al siguiente canal únicamente cuando el intento anterior no resulte exitoso. |
| `ParallelDeliveryStrategy` | Control | Ejecutar la entrega de la notificación utilizando simultáneamente todos los canales aplicables definidos para el destinatario. |
| `NotificationAdapterFactory`      | Control  | Resolver una implementación de `INotificationAdapter` compatible con el canal y proveedor especificados para ser utilizada por la estrategia de entrega. |
| `Alert`                           | Entity   | Representar la alerta confirmada recibida desde el Motor de Reglas, incluyendo su identificador, adulto mayor asociado, tipo, severidad y la información necesaria para generar la notificación. |
| `NotificationProfile`             | Entity   | Representar la configuración de notificación asociada al adulto mayor, incluyendo destinatarios, canales disponibles, prioridades, proveedores habilitados y la política de entrega configurada. |
| `Notification`                    | Entity   | Representar una notificación asociada a una alerta y un destinatario, incluyendo su estado actual, las fechas relevantes y la información necesaria para realizar el seguimiento completo de su ciclo de vida. |
| `NotificationAttempt`             | Entity   | Representar un intento individual de entrega mediante un canal y proveedor determinados, incluyendo la fecha, el resultado, la clasificación de la falla y la información necesaria para su trazabilidad. |

#### 10.2.4 Diagrama de secuencia — flujo principal

El siguiente diagrama de secuencia representa el flujo principal de interacción entre los objetos del componente Servicio de Notificaciones para el procesamiento de una alerta crítica, desde la recepción de una alerta confirmada hasta la confirmación del mensaje en el Bus de Mensajería. El componente recupera la configuración de notificación correspondiente al perfil del adulto mayor, verifica si la notificación ya fue procesada para evitar duplicados, determina los canales aplicables para cada destinatario y resuelve la estrategia de entrega correspondiente. En este escenario se utiliza una estrategia de envío paralelo, registrando posteriormente los intentos de entrega, actualizando el estado de la notificación y devolviendo el resultado del procesamiento al consumidor.

![Secuencia — Componente 2, flujo principal](../diagramas/secuencia-comp2-principal.png)
*Figura 7 — Secuencia: Procesamiento de una alerta con entrega de notificación*
> **Imagen en tamaño completo:** [`secuencia-comp2-principal.png`](../diagramas/secuencia-comp2-principal.png) · **Fuente editable:** [`secuencia-comp2-principal.mmd`](../diagramas/secuencia-comp2-principal.mmd)

El siguiente diagrama de secuencia representa un camino de error en el que ocurre una falla transitoria durante el envío de una notificación. El componente determina la estrategia de entrega aplicable, ejecuta el envío mediante el proveedor correspondiente y registra de forma durable el intento realizado. Al no completarse exitosamente la entrega, la notificación se actualiza al estado PendingRetry y el resultado del procesamiento indica que el mensaje no debe confirmarse al Bus de Mensajería, permitiendo su posterior reentrega conforme a la política de mensajería.

![Secuencia — Componente 2, camino de error](../diagramas/secuencia-comp2-error.png)
*Figura 8 — Secuencia: Falla transitoria en el envío mediante el único canal disponible*
> **Imagen en tamaño completo:** [`secuencia-comp2-error.png`](../diagramas/secuencia-comp2-error.png) · **Fuente editable:** [`secuencia-comp2-error.mmd`](../diagramas/secuencia-comp2-error.mmd)

---

### Componente 3 – Servicio de Ingesta de Eventos

**Responsabilidad:** Garantizar la recepción confiable de los eventos provenientes de fuentes simuladas o de dispositivos wearable, mediante la autenticación del emisor, la validación técnica del evento, la detección de duplicados, su aceptación durable mediante persistencia y la preparación para su publicación asíncrona hacia el Bus de Mensajería, preservando la integridad y disponibilidad de la información para su posterior procesamiento.

**Trazabilidad:** Soporta los casos de uso definidos en la Sección 1.4 relacionados con la recepción de eventos provenientes de las fuentes de monitoreo del adulto mayor. Asimismo, implementa el requerimiento funcional RF-01 (Recepción y conservación de eventos) y contribuye al cumplimiento de los atributos de calidad relacionados con la disponibilidad, resiliencia y procesamiento asíncrono de eventos. En la Vista de estructura interna presentada en la Sección 7.2, corresponde al Contenedor 3 – Servicio de Ingesta de Eventos, responsable de autenticar el emisor, validar técnicamente los eventos recibidos, garantizar su aceptación durable y publicarlos de forma asíncrona hacia el Bus de Mensajería para su posterior procesamiento.
 
#### 10.3.1 Diagrama de clases de diseño

El siguiente diagrama presenta el diseño interno del componente Servicio de Ingesta de Eventos y las principales clases, interfaces y relaciones que permiten autenticar y validar los eventos recibidos, garantizar su aceptación durable e idempotente y desacoplar su persistencia de la publicación al Bus de Mensajería mediante el patrón Transactional Outbox.

El diseño se organiza alrededor de `EventIngestionService`, que coordina la validación y persistencia atómica del `MonitoringEvent` y su `OutboxMessage`. La publicación se realiza posteriormente mediante `OutboxPublisher`, que procesa los mensajes pendientes y utiliza `IEventPublisher` para enviarlos al Bus de Mensajería, preservando los eventos incluso ante posibles fallos temporales en la publicación.

![Diagrama de clases — Componente 3](../diagramas/clases-componente3.png)
*Figura 9 — Diagrama de clases de diseño: Servicio de Ingesta de Eventos*
> **Imagen en tamaño completo:** [`clases-componente3.png`](../diagramas/clases-componente3.png) · **Fuente editable:** [`clases-componente3.mmd`](../diagramas/clases-componente3.mmd)


#### 10.3.2 Contratos de interfaz

| Método / Endpoint | Precondición | Postcondición | Excepciones |
|---|---|---|---|
| `EventIngestionController.`<br>`ReceiveAsync(request):`<br>`Task<EventIngestionResult>` | `request` no debe ser nulo y debe provenir de una fuente de monitoreo capaz de presentar las credenciales requeridas por el componente. | Se autentica al emisor, se transforma la solicitud en un `MonitoringEvent` y, cuando la autenticación es válida, se delega su aceptación mediante `IEventIngestionService`. Se devuelve un `EventIngestionResult` que indica si el evento fue aceptado, ya había sido aceptado, es inválido o el emisor no está autorizado. | Puede producir una excepción cuando la solicitud no puede interpretarse, ante una falla técnica inesperada durante la autenticación o el procesamiento, o cuando se cancela la operación. |
| `IEventSourceAuthenticator.`<br>`AuthenticateAsync(request):`<br>`Task<EventSourceAuthenticationResult>` | `request` no debe ser nulo y debe contener la información de autenticación requerida para identificar al emisor. | Se verifica la identidad del emisor y se devuelve un `EventSourceAuthenticationResult` que indica si está autenticado y, cuando corresponde, identifica la fuente mediante `SourceId`. Una autenticación inválida se representa mediante el resultado y no mediante una excepción. | Puede producir una excepción ante una falla técnica inesperada en el mecanismo de autenticación o ante la cancelación de la operación. |
| `IEventIngestionService.`<br>`AcceptAsync`<br>`(event: MonitoringEvent):`<br>`Task<EventIngestionResult>` | `event` no debe ser nulo y debe provenir de un emisor previamente autenticado. | Se valida técnicamente el evento y, si resulta válido, se genera su `OutboxMessage` y se solicita su aceptación durable. Si el `EventId` ya fue aceptado, se devuelve `AlreadyAccepted` sin crear un nuevo evento ni una nueva intención de publicación. Si la aceptación es exitosa, se devuelve `Accepted`. | Puede producir una excepción cuando ocurre una falla técnica que impide garantizar la aceptación durable, durante el acceso a persistencia o ante la cancelación de la operación. Los eventos inválidos o duplicados se representan mediante el resultado y no mediante excepciones. |
| `IIngestionEventValidator.`<br>`Validate(event: MonitoringEvent):`<br>`EventValidationResult` | `event` no debe ser nulo. | Se verifica que el evento contenga la estructura, identificadores, formatos y valores técnicos mínimos requeridos para su aceptación. Se devuelve un `EventValidationResult` que indica si el evento puede continuar con el proceso de ingesta y, cuando corresponda, la razón de su rechazo. | Puede producir una excepción únicamente ante una falla técnica inesperada durante la validación. Un evento técnicamente inválido se representa mediante `EventValidationResult`. |
| `IEventIngestionRepository.`<br>`AcceptAsync(event: MonitoringEvent,`<br>`outboxMessage: OutboxMessage):`<br>`Task<EventPersistenceResult>` | `event` y `outboxMessage` no deben ser nulos; `outboxMessage.EventId` debe corresponder a `event.EventId`. | El `MonitoringEvent` y su `OutboxMessage` se persisten **atómicamente en una misma transacción**. Si ambas escrituras se confirman, el evento se considera aceptado durablemente. Si `EventId` ya existe, no se crea un segundo evento ni un segundo mensaje de Outbox y se devuelve `AlreadyExists`. | Puede producir una excepción ante una falla técnica de persistencia, una violación de integridad distinta de la duplicidad esperada, la imposibilidad de completar la transacción de forma durable o la cancelación de la operación. |
| `OutboxPublisher.`<br>`PublishPendingAsync(): Task` | No Aplica. | Se recuperan los mensajes pendientes y se intenta publicar cada uno. Las publicaciones aceptadas se marcan como `Published`; ante fallas recuperables se registra el intento y el mensaje permanece disponible para un nuevo intento. | Puede producir una excepción ante una falla técnica que impida continuar de forma segura con el procesamiento del Outbox o ante la cancelación de la operación. Una falla individual de publicación debe registrarse sin provocar la pérdida del mensaje pendiente.                  |
| `IOutboxRepository.`<br>`GetPendingAsync():`<br>`Task<OutboxMessageCollection>` | No Aplica. | Se devuelve la colección de mensajes pendientes de publicación. Si no existen mensajes pendientes, se devuelve una colección vacía. | Puede producir una excepción ante una falla técnica de consulta o ante la cancelación de la operación. |
| `IEventPublisher.`<br>`PublishAsync(`<br>`outboxMessage: OutboxMessage): Task` | `outboxMessage` no debe ser nulo, debe encontrarse pendiente de publicación y debe contener un `Payload` válido. | Se envía el contenido del mensaje al Bus de Mensajería. Si el Bus acepta la publicación, la operación concluye satisfactoriamente. | Puede producir una excepción cuando el Bus no está disponible, ocurre un timeout, se rechaza la publicación, existe una falla de comunicación o se cancela la operación.                                                                                                             |

#### 10.3.3 Análisis de robustez

| Objeto | Tipo | Responsabilidad |
|---|---|---|
| `EventIngestionController`  | Boundary                           | Recibir las solicitudes provenientes de las fuentes de monitoreo, coordinar la autenticación del emisor, transformar la solicitud en un `MonitoringEvent` y delegar su aceptación mediante `IEventIngestionService`.                     |
| `IEventSourceAuthenticator` | Boundary                           | Definir el contrato para autenticar la fuente emisora del evento y devolver un resultado normalizado de autenticación, independientemente del mecanismo concreto utilizado.                                                             |
| `IEventIngestionRepository` | Boundary                           | Proporcionar acceso persistente para aceptar de forma durable un `MonitoringEvent`, almacenándolo atómicamente junto con su `OutboxMessage` y garantizando la idempotencia mediante la unicidad de `EventId`.                     |
| `IOutboxRepository`         | Boundary                           | Proporcionar acceso persistente a los mensajes pendientes del Outbox y permitir actualizar su estado e información de seguimiento después de cada intento de publicación.                                                              |
| `IEventPublisher`           | Boundary                           | Definir el contrato para publicar los mensajes pendientes hacia el Bus de Mensajería, aislando al componente de la tecnología concreta de mensajería utilizada.                                                                   |
| `EventIngestionService`     | Control                            | Coordinar el proceso de aceptación del evento, incluyendo validación técnica, creación del `OutboxMessage`, solicitud de persistencia durable e interpretación del resultado de aceptación o duplicidad.                   |
| `IngestionEventValidator`   | Control                            | Verificar que el evento recibido contenga la estructura, identificadores, formatos y valores técnicos mínimos requeridos para continuar con el proceso de ingesta.                                                                    |
| `OutboxPublisher`           | Control                            | Coordinar el procesamiento de los mensajes pendientes del Outbox, solicitar su publicación al Bus de Mensajería y registrar el resultado de cada intento sin perder los mensajes ante fallos temporales.                             |
| `MonitoringEvent`           | Entity                             | Representar el evento de monitoreo aceptado por el componente, incluyendo su identificador, fuente, adulto mayor asociado, tipo, versión del perfil, fechas e información necesaria para su trazabilidad y posterior procesamiento. |
| `OutboxMessage`             | Entity                             | Representar la intención durable de publicar un evento, incluyendo su identificador, referencia al evento, payload, estado de publicación, fechas e información de seguimiento de los intentos realizados.                         |

#### 10.3.4 Diagrama de secuencia — flujo principal

El siguiente diagrama de secuencia representa el flujo principal del Servicio de Ingesta de Eventos, desde la recepción de un evento proveniente de una fuente de monitoreo hasta su aceptación durable y posterior publicación en el Bus de Mensajería. El componente autentica al emisor, valida técnicamente el evento y persiste atómicamente el MonitoringEvent junto con su OutboxMessage. Posteriormente, OutboxPublisher recupera el mensaje pendiente y gestiona su publicación al Bus, actualizando su estado cuando la publicación se completa satisfactoriamente.

![Secuencia — Componente 3, flujo principal](../diagramas/secuencia-comp3-principal.png)
*Figura 10 — Secuencia: Aceptación durable y publicación de un evento de monitoreo*
> **Imagen en tamaño completo:** [`secuencia-comp3-principal.png`](../diagramas/secuencia-comp3-principal.png) · **Fuente editable:** [`secuencia-comp3-principal.mmd`](../diagramas/secuencia-comp3-principal.mmd)

El siguiente diagrama de secuencia representa un camino de error en el que ocurre una falla transitoria durante la publicación de un evento previamente aceptado de forma durable. El MonitoringEvent y su OutboxMessage permanecen persistidos, mientras OutboxPublisher registra la falla y mantiene el mensaje disponible para un intento posterior. De esta forma, una indisponibilidad temporal del Bus de Mensajería no provoca la pérdida del evento ni afecta su aceptación previa.

![Secuencia — Componente 3, camino de error](../diagramas/secuencia-comp3-error.png)
*Figura 11 — Secuencia: Falla transitoria durante la publicación de un evento*
> **Imagen en tamaño completo:** [`secuencia-comp3-error.png`](../diagramas/secuencia-comp3-error.png) · **Fuente editable:** [`secuencia-comp3-error.mmd`](../diagramas/secuencia-comp3-error.mmd)

---

# BLOQUE 6 — CALIDAD Y TENDENCIAS
*Hito: Entrega final (S14)*

---

## 14. Asuntos clave de diseño

Este capítulo analiza SeniorCareHub bajo las lentes transversales que atraviesan su arquitectura: su condición de sistema distribuido desplegado en la nube, su comportamiento concurrente y con restricciones temporales, y su relación con el dominio de Internet de las Cosas. Cada apartado se apoya en las vistas del capítulo 7 y en las decisiones registradas en los capítulos 8 y 9, y explicita tanto lo que el diseño resuelve como lo que deliberadamente deja fuera de alcance.

### 14.1 Sistemas distribuidos y computación en la nube

#### 14.1.1 Por qué SeniorCareHub es un sistema distribuido

SeniorCareHub cumple la caracterización clásica de un sistema distribuido: un conjunto de procesos independientes que se ejecutan en nodos separados, se comunican únicamente mediante paso de mensajes por red, no comparten memoria y no disponen de un reloj global común.

La vista de despliegue (§7.4) lo hace evidente. El wearable emite desde fuera de la nube; el Servicio de Ingesta y la API se ejecutan en un plan de App Service; el Motor de Reglas y el Servicio de Notificaciones lo hacen en un entorno de Container Apps con réplicas independientes; el bus y las bases de datos son servicios gestionados con su propio ciclo de vida; y los proveedores de notificación son sistemas de terceros administrados por organizaciones distintas.

Esta distribución no es un accidente de implementación sino una consecuencia directa de los drivers. QA-03 exige que ninguna alerta se pierda ante el fallo de un componente, y QA-01 una disponibilidad que no dependa del eslabón más débil de una cadena. Ambos requisitos son inalcanzables si la recepción, la evaluación y el despacho comparten destino: el análisis de alternativas de §8.3 descartó por ese motivo tanto el monolito sincrónico como los servicios acoplados por llamadas REST encadenadas.

#### 14.1.2 Consecuencias de la distribución y respuestas del diseño

| Consecuencia | Cómo se manifiesta en SeniorCareHub | Respuesta del diseño |
|---|---|---|
| Fallos parciales | Un componente puede caer mientras los demás siguen operando, y el que falla no puede avisar de forma confiable | El intermediario durable conserva el mensaje hasta que un consumidor lo confirma; la cola de mensajes muertos captura lo que agota los reintentos (§8.2) |
| Consistencia eventual | El dashboard puede no reflejar todavía un evento ya recibido por la ingesta | Se acepta explícitamente como consecuencia negativa del estilo (§8.4); las consultas de estado no se usan como fuente para decisiones críticas, que viajan por notificación |
| Entrega al menos una vez | Un mismo evento o alerta puede procesarse más de una vez tras un reintento | Idempotencia obligatoria en todos los consumidores, deduplicación por adulto mayor, tipo y período (ADR-003) y detección de duplicados del bus |
| Ausencia de reloj global | La ventana de confirmación compara instantes registrados en nodos distintos, cuyos relojes divergen | Las ventanas se calculan sobre la marca temporal del evento de origen y no sobre la del instante de procesamiento, de modo que un desfase entre nodos no altera el resultado de la evaluación |
| Orden parcial de mensajes | Dos eventos de una misma persona podrían evaluarse fuera de secuencia | Particionamiento por sesión con identificador del adulto mayor, que garantiza orden y exclusividad de consumo (§7.5.3) |
| Observabilidad fragmentada | Ninguna traza local explica por sí sola el recorrido de una alerta | Identificador de correlación propagado extremo a extremo y telemetría centralizada en Application Insights (§7.4.1) |

#### 14.1.3 Modelo de nube adoptado

El sistema se despliega bajo un modelo de **plataforma como servicio**, apoyándose en servicios gestionados para el cómputo, la mensajería y la persistencia. Esta elección se contrastó con dos alternativas.

Un modelo de **infraestructura como servicio**, con máquinas virtuales administradas por el equipo, habría otorgado control total sobre el sistema operativo y las versiones del intermediario y del motor de base de datos. Se descartó porque ese control no responde a ningún driver del sistema y, en cambio, traslada al equipo la responsabilidad de parchado, alta disponibilidad y respaldo, actividades que un equipo académico de tres personas no puede sostener con la fiabilidad que exige QA-01.

Un modelo **completamente sin servidor**, con funciones activadas por evento, resultaba atractivo por su costo en reposo. Se descartó por la misma razón que se rechazó el escalado a cero réplicas en §7.4.2: el arranque en frío introduce una demora de varios segundos que compite directamente contra el presupuesto de cinco segundos de QS-02. La decisión ilustra una tensión característica de la nube, entre elasticidad económica y latencia predecible, resuelta a favor de la segunda porque el dominio es la seguridad de una persona.

La adopción de servicios gestionados implica además un **modelo de responsabilidad compartida**: la operación del intermediario, del motor de base de datos y de la plataforma de ejecución corresponde al proveedor, mientras que el equipo conserva la responsabilidad sobre la lógica de negocio, el modelo de datos, la configuración de seguridad y el control de acceso. Los compromisos de disponibilidad publicados por el proveedor forman parte del razonamiento que sustenta la meta de QS-01, tal como se argumentó al dimensionar el despliegue.

#### 14.1.4 Falacias de la computación distribuida

El análisis de las ocho falacias formuladas por Deutsch y Gosling permite verificar qué supuestos implícitos podrían comprometer el diseño.

**La red es confiable.** El diseño no incurre en esta falacia: es precisamente su rechazo lo que motiva el estilo adoptado. La ingesta acepta el evento sin esperar la evaluación, y ningún mensaje se considera procesado hasta que el consumidor lo confirma.

**La latencia es cero.** Parcialmente atendida. El presupuesto de QS-02 contempla los saltos de red internos, pero el diseño depende de la latencia de proveedores externos sobre los que no tiene control. La mitigación es el tiempo límite por canal y el escalamiento al siguiente canal por prioridad (§7.3.2), no la esperanza de una respuesta rápida.

**El ancho de banda es infinito.** Atendida indirectamente. La minimización del contenido de los eventos, adoptada en §8.4 por motivos de privacidad, reduce también el volumen transportado. El sistema no transmite datos clínicos ni multimedia.

**La red es segura.** Atendida. Todo tráfico viaja cifrado, y el acceso entre servicios se realiza mediante identidades administradas en lugar de credenciales incrustadas en configuración (§7.4.2).

**La topología no cambia.** Atendida por construcción. Las réplicas de Container Apps se crean y destruyen según la carga, de modo que ningún componente puede asumir la dirección fija de otro: toda comunicación del camino crítico ocurre a través del intermediario y no por invocación directa entre pares.

**Hay un solo administrador.** No atendida, y es una limitación reconocida. Los proveedores de notificación son operados por terceros, con sus propias ventanas de mantenimiento, cuotas y cambios de contrato. La restricción REST-01 lo declara, y ADR-004 lo mitiga aislando cada proveedor tras una interfaz interna estable, pero el sistema sigue expuesto a decisiones ajenas.

**El costo de transporte es cero.** Parcialmente atendida. Cada salto por el intermediario consume operaciones facturables y añade trabajo de serialización. La decisión de no dividir en más contenedores de los necesarios, y de resolver la gestión de alertas dentro del Motor de Reglas en lugar de crear un contenedor propio (§7.6.5), responde en parte a este criterio.

**La red es homogénea.** No atendida, y tampoco es un objetivo. El sistema convive deliberadamente con tres protocolos distintos según la frontera: HTTP en el borde con el dispositivo, mensajería asincrónica en el interior y protocolos de proveedor en la salida. La heterogeneidad se administra mediante adaptadores en lugar de negarse.

#### 14.1.5 Límites reconocidos del diseño distribuido

**Sin replicación geográfica.** Todos los recursos residen en una única región. Una interrupción regional del proveedor deja al sistema completamente indisponible, sin ruta alterna. La meta de QS-01 se dimensionó contra ese supuesto.

**Base de datos como punto único de falla.** El intermediario protege las alertas en tránsito, pero el Motor de Reglas no puede evaluar sin acceso a las reglas y al estado de confirmación. Ante una caída prolongada de la base, los eventos se acumulan en el bus sin pérdida —lo que preserva QA-03— pero el sistema deja de detectar situaciones críticas en tiempo útil, lo que sí compromete QS-02.

**Zona de disponibilidad única.** Decisión consciente documentada en §7.4.2, coherente con la meta comprometida y no con una meta superior.

**El simulador no reproduce la adversidad real de la red.** Un dispositivo portado por una persona sufre pérdidas de cobertura, agotamiento de batería y reconexiones que el simulador no genera. Las garantías de no pérdida verificadas en pruebas cubren el tramo desde la ingesta en adelante, no el tramo entre el dispositivo y la nube.

**Ausencia de procesamiento en el borde.** Todo evento viaja íntegro a la nube antes de ser evaluado, incluidos aquellos que ninguna regla llegará a considerar. Las implicaciones de esta decisión se analizan en §14.3.

### 14.2 Sistemas concurrentes y de tiempo real

#### 14.2.1 Naturaleza concurrente del sistema

La concurrencia en SeniorCareHub proviene de cuatro fuentes simultáneas: múltiples adultos mayores emitiendo eventos al mismo tiempo, múltiples réplicas de cada servicio consumiendo del intermediario, múltiples usuarios consultando el dashboard, y múltiples notificaciones despachándose en paralelo hacia proveedores distintos.

La vista de concurrencia (§7.5) documenta las unidades de ejecución, los recursos compartidos y los mecanismos de sincronización adoptados. Este apartado no los repite: analiza el sistema desde la perspectiva temporal, que es la que determina si las garantías de concurrencia bastan para cumplir los compromisos asumidos.

#### 14.2.2 Clasificación temporal del sistema

SeniorCareHub es un sistema de **tiempo real blando**, no duro. La distinción es relevante y conviene sostenerla con precisión, porque determina qué técnicas de diseño corresponden y cuáles serían sobredimensionadas.

En un sistema de tiempo real duro, incumplir un plazo constituye un fallo del sistema con consecuencias equivalentes a producir un resultado incorrecto: el control de vuelo o un marcapasos operan bajo esa lógica. En un sistema de tiempo real blando, el valor del resultado se degrada progresivamente al superarse el plazo, pero el resultado sigue siendo útil.

La formulación misma de QS-02 evidencia la clasificación: la meta se expresa como cinco segundos **en el percentil 95**, no como un plazo absoluto para cada evento. Un requisito de tiempo real duro no admitiría un percentil, porque el cinco por ciento restante constituiría un fallo. En SeniorCareHub, una alerta entregada en siete segundos sigue cumpliendo su propósito: es peor que una entregada en tres, pero no equivale a no entregarla.

La consecuencia práctica es que el diseño no requiere un sistema operativo de tiempo real, planificación determinista de tareas, ni análisis de planificabilidad o de inversión de prioridades. Sí requiere, en cambio, administrar de forma explícita un presupuesto de latencia y verificar su cumplimiento mediante medición estadística.

#### 14.2.3 Presupuesto de latencia

Descomponer el objetivo de cinco segundos permite identificar dónde se consume realmente el tiempo y dónde tiene sentido optimizar. Las cifras siguientes son estimaciones de diseño que deben validarse mediante medición sobre el despliegue real; su valor está en las proporciones relativas más que en los valores absolutos.

| Etapa | Estimación | Observaciones |
|---|---|---|
| Recepción, validación y persistencia del evento | ~200 ms | Incluye la escritura en el Almacén de Eventos |
| Publicación y entrega por el intermediario | ~100 ms | Dos tránsitos por el bus a lo largo del flujo |
| Lectura de reglas y estado, y evaluación | ~300 ms | Incluye una lectura y una escritura sobre la BD Operativa |
| Publicación y entrega de la alerta confirmada | ~100 ms | Segundo tránsito por el intermediario |
| Resolución de destinatarios y canales | ~200 ms | Lectura del perfil de notificación |
| **Invocación al proveedor externo** | **1 000 – 3 000 ms** | **Etapa dominante y fuera del control del equipo** |
| Margen disponible | ~1 100 ms | Absorbe variabilidad y reintentos internos |

La conclusión relevante es que el tramo bajo control del equipo consume aproximadamente el veinte por ciento del presupuesto, mientras que la invocación al proveedor externo domina el resto. Optimizar el procesamiento interno tendría un efecto marginal sobre QS-02; en cambio, la elección del proveedor, la configuración de su tiempo límite y el orden de prioridad de canales son las palancas que efectivamente determinan el cumplimiento de la meta. Esta observación refuerza la decisión de ADR-004 de mantener a los proveedores tras adaptadores intercambiables.

Una segunda consecuencia afecta a la ventana de confirmación de ADR-003: cualquier ventana configurada se suma íntegramente al presupuesto. Con el margen estimado, una ventana superior a un segundo comprometería la meta para los eventos que la requieran. Por ello ADR-003 establece que los eventos de criticidad inmediata no esperan ventana alguna, lo que resuelve la tensión entre exactitud y latencia a favor de la latencia justo donde la persona corre riesgo.

#### 14.2.4 Riesgos temporales identificados

**Acumulación por contrapresión.** Si la tasa de eventos entrantes supera de forma sostenida la capacidad de evaluación, la cola crece y la latencia aumenta aunque no se pierda ningún mensaje. Conviene explicitar el matiz: la durabilidad del intermediario protege contra la pérdida, no contra la demora. La mitigación es el escalado automático de réplicas gobernado por la profundidad de la suscripción, con el techo de paralelismo que impone el particionamiento por sesión (§7.5.3).

**Expiración del bloqueo durante el procesamiento.** Si una evaluación excede la duración del bloqueo del mensaje, el intermediario lo reentrega y se produce trabajo duplicado que consume capacidad. La deduplicación evita la alerta doble, pero no el costo temporal. Es una condición de configuración a verificar mediante pruebas, no una propiedad garantizada por el diseño.

**Pausas de la plataforma de ejecución.** Las pausas por recolección de basura y el agotamiento del grupo de hilos en los servicios .NET introducen variabilidad no determinista en el percentil alto de latencia. Es una de las razones por las que la meta se expresa en percentil 95 y no como plazo absoluto.

**Arranque en frío.** Mitigado mediante la réplica mínima permanente decidida en §7.4.2, a costa del consumo en reposo.

**Divergencia de relojes.** Las ventanas temporales se calculan sobre la marca del evento de origen y no sobre la del instante de procesamiento, de modo que un desfase entre nodos no altera el resultado de la evaluación.

#### 14.2.5 Verificación de las garantías temporales

Las garantías de este sistema son estadísticas y por lo tanto deben verificarse por medición, no por demostración analítica. La telemetría centralizada descrita en §7.4.1, con identificador de correlación propagado extremo a extremo, permite reconstruir la latencia real de cada alerta y calcular el percentil comprometido sobre datos de operación. Sin esa instrumentación, QS-02 sería un objetivo declarado pero no verificable, lo que lo dejaría fuera de la definición de escenario de calidad medible.


*Documento generado bajo el template estándar PSWE-04 — Universidad Cenfotec — Maestría Profesional en Ingeniería del Software*
