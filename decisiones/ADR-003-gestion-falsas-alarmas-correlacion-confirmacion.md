# ADR-003: Gestionar falsas alarmas mediante correlación, confirmación y deduplicación

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-02, RF-05, QA-02, QA-03 |
| **Escenarios relacionados** | QS-02, QS-03, QS-05 |

## Contexto

Generar una alerta por cada evento recibido podría producir notificaciones duplicadas o falsas alarmas. Por ejemplo, múltiples eventos de movimiento pueden representar una misma caída, y una pérdida temporal de comunicación puede recuperarse antes de requerir intervención.

No obstante, esperar demasiado tiempo para confirmar una condición reduce las falsas alarmas, pero incrementa la latencia de las alertas verdaderamente críticas.

## Decisión

Se decide incorporar en el Motor de Reglas una etapa de confirmación configurable que considere:

- Correlación de eventos relacionados.
- Ventanas temporales de confirmación.
- Deduplicación por adulto mayor, tipo de evento y período.
- Estado temporal de evaluación por persona monitoreada.
- Niveles de confianza o criticidad.
- Políticas diferenciadas según el tipo de evento.

Los eventos de criticidad inmediata podrán generar una alerta sin esperar una ventana adicional cuando la regla configurada así lo determine. Los eventos ambiguos podrán requerir confirmación mediante eventos posteriores o el cumplimiento de una duración mínima.

Una alerta confirmada deberá incluir una referencia a los eventos que la originaron y a la versión de reglas utilizada.

## Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Alertar por cada evento individual | Mínima latencia y lógica sencilla | Fatiga de alarmas, duplicados y falsas alertas | No atiende adecuadamente el problema central |
| Confirmación manual antes de notificar | Reduce alertas incorrectas | Requiere supervisión humana permanente y puede retrasar emergencias | No satisface QS-02 |
| Correlación y confirmación configurable | Equilibra latencia y reducción de falsas alarmas | Requiere estado temporal y aumenta complejidad | **Alternativa seleccionada** |

## Consecuencias

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

## Evidencia y validación

- **Componente:** Motor de Reglas y Generación de Alertas.
- **Flujo de comportamiento:** correlación de eventos y confirmación de alerta.
- **Prueba prevista:** simular eventos duplicados, pérdida breve de comunicación y una caída confirmada.
- **Resultado esperado:** los duplicados producen una única alerta; el evento transitorio no genera una alerta crítica; la caída confirmada respeta la meta de latencia.

## Revisión requerida si

- Las ventanas de confirmación provocan incumplimientos repetidos de QS-02.
- El dominio requiere modelos probabilísticos o aprendizaje automático para reducir falsas alarmas.
- Las métricas muestran que las reglas configurables no alcanzan la exactitud necesaria.
