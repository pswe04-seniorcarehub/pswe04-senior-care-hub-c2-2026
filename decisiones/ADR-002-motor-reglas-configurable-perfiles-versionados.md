# ADR-002: Implementar un motor de reglas configurable y perfiles versionados

| Campo | Detalle |
|---|---|
| **Estado** | Propuesta |
| **Fecha** | 2026-07-25 |
| **Autores** | Equipo Grupo 3 |
| **Drivers atendidos** | RF-02, RF-05, QA-02, QA-05 |
| **Escenarios relacionados** | QS-02, QS-05 |

## Contexto

Los criterios que determinan si un evento representa una situación crítica varían entre adultos mayores. Por ejemplo, el tiempo máximo de inactividad, las zonas consideradas seguras, la criticidad de una caída y los destinatarios de una alerta pueden depender del perfil individual.

Estas reglas deben poder modificarse durante la operación normal sin recompilar ni redesplegar el Motor de Reglas. Sin embargo, permitir reglas completamente arbitrarias aumentaría considerablemente la complejidad, los riesgos de seguridad y la dificultad para garantizar la latencia de procesamiento.

## Decisión

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

## Alternativas consideradas

| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|---|---|---|---|
| Reglas codificadas directamente | Simplicidad y alto rendimiento | Cada cambio requiere modificar código y redesplegar | Incumple QA-05 |
| Motor de reglas completamente dinámico o DSL arbitrario | Máxima flexibilidad | Mayor complejidad, riesgos de seguridad y dificultad de validación | Sobredimensionado para el alcance |
| Tipos de reglas controlados con parámetros configurables | Equilibrio entre modificabilidad, control y rendimiento | Los nuevos algoritmos requieren despliegue | **Alternativa seleccionada** |

## Consecuencias

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

## Evidencia y validación

- **Componente detallado principal:** Motor de Reglas y Generación de Alertas.
- **Patrón previsto:** Strategy para seleccionar el evaluador correspondiente al tipo de regla.
- **Prueba prevista:** modificar el umbral de inactividad durante la operación y enviar eventos antes y después del cambio.
- **Resultado esperado:** los eventos posteriores utilizan la nueva versión sin redesplegar el servicio.

## Revisión requerida si

- Los usuarios necesitan expresar reglas arbitrarias no cubiertas por los evaluadores disponibles.
- El número de tipos de regla crece hasta hacer difícil mantener evaluadores independientes.
- La evaluación dinámica impide cumplir la latencia de QS-02.
