# ADR-004: Desacoplar las notificaciones mediante canales configurables y adaptadores

| Campo | Detalle |
|---|---|
| **Estado** | Aceptada |
| **Fecha** | 2026-07-25 |
| **Última revisión** | 2026-08-10 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-03, RF-05, QA-02, QA-05, REST-01 |
| **Escenarios relacionados** | QS-02, QS-05 |

## Contexto

SeniorCareHub debe notificar a familiares y cuidadores mediante distintos canales, como SMS, correo electrónico o mensajería. Los proveedores pueden cambiar, utilizar contratos diferentes o no estar disponibles en todos los entornos.

Acoplar el Motor de Reglas directamente a un proveedor dificultaría incorporar nuevos canales y haría que los cambios de integración afectaran la lógica de detección.

## Decisión

Se decide implementar un Servicio de Notificaciones independiente que:

- Reciba alertas confirmadas.
- Consulte el perfil de notificación.
- Resuelva destinatarios y canales.
- Ordene los canales según prioridad.
- Seleccione un adaptador compatible con cada canal.
- Registre cada intento y resultado de entrega.
- Permita incorporar nuevos adaptadores sin modificar el Motor de Reglas.

Cada proveedor externo se ubicará detrás de una interfaz interna estable. La selección de canales y proveedores se determinará mediante configuración.

## Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Integrar proveedores dentro del Motor de Reglas | Menos componentes | Alto acoplamiento y propagación de fallos | Contradice RF-03 y QA-05 |
| Un servicio independiente por proveedor | Máximo aislamiento | Mayor costo operativo y duplicación de lógica | Complejidad innecesaria para el alcance |
| Servicio multicanal con adaptadores | Aislamiento, reutilización y extensibilidad | El servicio concentra coordinación de varios canales | **Alternativa seleccionada** |

## Consecuencias

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

## Evidencia y validación

- **Patrones previstos:** Adapter para proveedores y Strategy para selección de canal.
- **Prueba prevista:** incorporar un proveedor simulado nuevo sin modificar el Motor de Reglas.
- **Resultado esperado:** el nuevo adaptador puede seleccionarse mediante configuración.

## Revisión requerida si

- La cantidad de canales o el volumen de notificaciones requiere separar cada canal en un servicio independiente.
- Un proveedor exige un modelo de integración incompatible con la interfaz común.
- Se requiere enviar simultáneamente por todos los canales en lugar de utilizar prioridad.
