# Registro de Decisiones Arquitectónicas (ADR) — SeniorCareHub

Esta carpeta contiene los **Architecture Decision Records (ADR)** del proyecto: el historial de decisiones de arquitectura significativas, con su contexto, las alternativas evaluadas y las consecuencias aceptadas.

Un ADR se crea cuando una decisión involucra trade-offs reales y tiene consecuencias costosas de revertir, no se documentan aquí decisiones triviales o fácilmente reversibles.

## Índice de decisiones

| ADR | Título | Estado | Drivers atendidos |
|---|---|---|---|
| [ADR-001](ADR-001-separar-ingesta-evaluacion-alertas-notificaciones.md) | Separar ingesta, evaluación, alertas y notificaciones mediante eventos | Propuesta | RF-01, RF-03, QA-01, QA-02, QA-03, REST-01 |
| [ADR-002](ADR-002-motor-reglas-configurable-perfiles-versionados.md) | Implementar un motor de reglas configurable y perfiles versionados | Propuesta | RF-02, RF-05, QA-02, QA-05 |
| [ADR-003](ADR-003-gestion-falsas-alarmas-correlacion-confirmacion.md) | Gestionar falsas alarmas mediante correlación, confirmación y deduplicación | Propuesta | RF-02, RF-05, QA-02, QA-03 |
| [ADR-004](ADR-004-notificaciones-canales-configurables-adaptadores.md) | Desacoplar las notificaciones mediante canales configurables y adaptadores | Propuesta | RF-03, RF-05, QA-02, QA-05, REST-01 |

## Convención de nombres

```
ADR-XXX-titulo-corto-en-minusculas.md
```

- `XXX` es un número secuencial de tres dígitos (001, 002, 003...), asignado en orden de creación y **nunca reutilizado**, aunque el ADR quede obsoleto.
- El título va en minúsculas, sin tildes ni caracteres especiales, con palabras separadas por guiones.

## Estados posibles

| Estado | Significado |
|---|---|
| **Propuesta** | La decisión está documentada pero aún no ha sido validada o discutida por el equipo. |
| **Aceptado** | La decisión fue revisada y se adopta como parte de la arquitectura. |
| **Rechazado** | La decisión fue evaluada y descartada, se conserva como referencia histórica. |
| **Reemplazado por ADR-XXX** | La decisión fue superada por otra más reciente. Se indica el ADR que la reemplaza. |
| **Obsoleto** | La decisión ya no aplica, sin que exista un reemplazo directo. |
