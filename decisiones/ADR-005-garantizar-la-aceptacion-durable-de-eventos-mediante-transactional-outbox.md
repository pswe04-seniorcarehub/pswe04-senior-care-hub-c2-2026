# ADR-005: Garantizar la aceptación durable de eventos mediante Transactional Outbox

| Campo | Detalle |
|---|---|
| **Estado** | Aceptada |
| **Fecha** | 2026-08-11 |
| **Última revisión** | 2026-08-11 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-01, QA-01, QA-02, QA-03 |
| **Escenarios relacionados** | QS-01, QS-02, QS-03 |

## Contexto

El Servicio de Ingesta debe conservar un evento antes de considerarlo aceptado y posteriormente publicarlo al Bus de Mensajería.

Persistir en PostgreSQL y publicar en Service Bus son operaciones sobre recursos transaccionales diferentes. Si primero se persiste y después se publica directamente, una falla entre ambos pasos puede dejar un evento almacenado que nunca avance hacia el Motor de Reglas. Invertir el orden puede dejar un mensaje publicado cuya aceptación durable no quedó registrada.

No existe en el diseño una transacción ACID única que abarque PostgreSQL y Service Bus.

## Decisión

Se aplica **Transactional Outbox** dentro del Servicio de Ingesta.

`IEventIngestionRepository.AcceptAsync(event, outboxMessage)` persiste en **la misma base de datos y la misma transacción local**:

1. `MonitoringEvent`.
2. `OutboxMessage`, que representa la intención durable de publicar dicho evento.

En el despliegue actual ambos registros deben pertenecer al **Almacén de Eventos** para que la atomicidad descrita sea físicamente realizable.

El evento se considera **aceptado durablemente después del commit local**. La respuesta de aceptación al emisor no espera que Service Bus acepte la publicación.

Posteriormente, `OutboxPublisher`:

1. recupera mensajes pendientes;
2. los publica mediante `IEventPublisher`;
3. marca como `Published` los mensajes aceptados por el Bus;
4. mantiene pendientes los que fallan de forma recuperable.

`EventId` es único. Una repetición de la misma solicitud produce `AlreadyAccepted` sin crear otro evento ni otro `OutboxMessage`.

Puede ocurrir que Service Bus acepte un mensaje y falle el marcado local de `Published`. Por ello, la republicación es posible y los consumidores siguen siendo idempotentes. La detección de duplicados del broker es una defensa adicional, no un sustituto de la idempotencia de los consumidores.

## Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Persistir y publicar directamente | Implementación simple | Ventana `DB OK / Bus FAIL` | Puede dejar eventos sin avanzar |
| Publicar y luego persistir | Publicación inmediata | Ventana `Bus OK / DB FAIL` | Rompe trazabilidad de aceptación |
| Transacción distribuida | Semántica transaccional fuerte entre recursos | Mayor complejidad y acoplamiento; no forma parte del diseño actual | No justificada |
| Transactional Outbox | Atomicidad local, recuperación y desacoplamiento | Requiere tabla/estado adicional y publicador | **Alternativa seleccionada** |

## Consecuencias

**Positivas**

- Todo evento aceptado queda acompañado por una intención durable de publicación.
- Una indisponibilidad temporal del Bus no provoca pérdida de un evento aceptado.
- Los mensajes pendientes pueden publicarse cuando el Bus se recupera.
- La unicidad de `EventId` mantiene idempotencia en la frontera de entrada.
- QA-03 se extiende hasta el inicio del pipeline.

**Negativas**

- Se agregan `OutboxMessage`, `IOutboxRepository` y `OutboxPublisher`.
- La publicación ocurre de forma asíncrona después de la respuesta HTTP.
- El tiempo `commit → publicación` forma parte de QS-02.
- Deben monitorearse profundidad y antigüedad del Outbox.
- Una falla después de publicar pero antes de marcar `Published` puede producir republicación.
- Varias instancias del publicador deben coordinar la selección de mensajes pendientes para evitar trabajo concurrente innecesario.

## Evidencia y validación

- §10.3.1 — diseño de Ingesta.
- §10.3.2 — contratos de persistencia, Outbox y publicación.
- §10.3.4 — flujo principal y falla de publicación.
- §12.2 — Transactional Outbox.
- §13.1 — QS-02 y QS-03.
- §13.2 — simplicidad de Ingesta vs. no pérdida.

**Prueba prevista:** confirmar la transacción de aceptación y provocar indisponibilidad de Service Bus inmediatamente después.

**Resultado esperado:** `MonitoringEvent` y `OutboxMessage` continúan persistidos y el evento se publica cuando el Bus vuelve a estar disponible.

**Prueba adicional:** permitir la publicación y provocar una falla antes de marcar el Outbox como `Published`.

**Resultado esperado:** una eventual republicación no produce una segunda alerta de negocio debido a la idempotencia y deduplicación del pipeline.

## Revisión requerida si

- La arquitectura cambia de forma que el Bus se convierta en la primera aceptación durable.
- La latencia del Outbox impide cumplir QS-02.
- La profundidad o antigüedad del Outbox crece sostenidamente bajo la carga objetivo.
- Se adopta una tecnología que permita una única transacción confiable entre almacenamiento y publicación sin introducir un acoplamiento inaceptable.