# ADR-001: Separar ingesta, evaluación, alertas y notificaciones mediante eventos

| Campo | Detalle |
|---|---|
| **Estado** | Aceptada |
| **Fecha** | 2026-07-25 |
 **Última revisión** | 2026-08-10 |
| **Autores** | Roberto Obed Del Cid Winter, Lisdiana Mercedes Rodriguez Alvarado, Maria Isabel Vallejos Rodriguez |
| **Drivers atendidos** | RF-01, RF-03, QA-01, QA-02, QA-03, REST-01 |
| **Escenarios relacionados** | QS-01, QS-02, QS-03 |

## Contexto

SeniorCareHub debe recibir continuamente eventos simulados de monitoreo, evaluarlos mediante reglas configurables, generar alertas cuando se confirma una situación crítica y despacharlas mediante proveedores externos.

Estas actividades presentan características y ritmos distintos. La recepción de eventos debe continuar aunque el motor de reglas se encuentre temporalmente saturado, y la evaluación de eventos no debe detenerse por la indisponibilidad de un proveedor de SMS o correo electrónico.

Una cadena de llamadas sincrónicas entre recepción, evaluación y notificación provocaría que el fallo de un componente se propagara al resto del pipeline. Además, obligaría a que todos los componentes estuvieran disponibles al mismo tiempo para poder procesar un evento.
Esto tensionaría QA-01 y permitiría que un reinicio o fallo temporal provocara pérdida de trabajo, contrario a QA-03.

## Decisión

Se decide dividir el pipeline crítico en tres responsabilidades principales:

- **Servicio de Ingesta**, responsable de validar y aceptar eventos.
- **Motor de Reglas**, responsable de evaluar los eventos y determinar si deben generar una alerta, responsable de registrar la alerta y controlar su estado.
- **Servicio de Notificaciones**, responsable de seleccionar canales, invocar proveedores y registrar los intentos de entrega.

La comunicación entre estas etapas se realizará de manera asíncrona mediante un intermediario de mensajería durable basado en publicación y suscripción.

El productor no dependerá de que el consumidor se encuentre disponible en el instante en que se publica el evento. Los mensajes permanecerán almacenados hasta que puedan ser procesados o trasladados a una cola de mensajes fallidos.

## Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Monolito con procesamiento sincrónico | Menor complejidad operativa y menor latencia interna | Un fallo en reglas o notificaciones afecta la recepción; los reintentos dependen del proceso | No satisface adecuadamente QA-01 y QA-03 |
| Servicios separados mediante REST sincrónico | Permite desplegar servicios independientes | Acoplamiento temporal, propagación de fallos y necesidad de que todos los servicios estén disponibles | Mantiene los principales riesgos de pérdida e indisponibilidad |
| Procesamiento orientado a eventos | Desacoplamiento temporal, aislamiento de fallos y almacenamiento durable | Mayor complejidad de despliegue, consistencia eventual y depuración distribuida | **Alternativa seleccionada** |

## Consecuencias

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

## Evidencia y validación

- §7.2 — Vista de contenedores.
- §7.3.1 — Flujo de evento crítico.
- §7.3.2 — Fallo de proveedor y fallback.
- §7.5 — Vista de concurrencia.
- §10.1 — Motor de Reglas.
- §10.2 — Servicio de Notificaciones.
- §10.3 — Servicio de Ingesta.
- ADR-005 — aceptación durable mediante Transactional Outbox.

- **Prueba prevista:** detener temporalmente el Motor de Reglas mientras el Servicio de Ingesta continúa recibiendo eventos.
- **Resultado esperado:** los eventos permanecen disponibles en el intermediario y son procesados cuando el consumidor se recupera.

## Revisión requerida si

- La latencia adicional del intermediario impide cumplir QS-02.
- La operación del sistema no puede asumir la complejidad de una plataforma distribuida.
- El volumen real de eventos resulta suficientemente pequeño y tolerante a fallos como para justificar una arquitectura más simple.
