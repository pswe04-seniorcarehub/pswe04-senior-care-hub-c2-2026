# SeniorCareHub – Plataforma Inteligente de Monitoreo y Asistencia para Adultos Mayores

## Contexto y problema

Los adultos mayores que viven solos o requieren supervisión parcial pueden enfrentar situaciones de riesgo como caídas, períodos prolongados de inactividad o desorientación. En muchos casos, la detección tardía de estos eventos limita la capacidad de respuesta de familiares y cuidadores.

La propuesta busca abordar este problema mediante una plataforma distribuida de monitoreo que permita detectar oportunamente eventos críticos y generar alertas automáticas para mejorar la atención y el seguimiento de las personas adultas mayores.

## Objetivo

Diseñar una plataforma IoT distribuida que reciba datos desde un dispositivo wearable simulado, detecte eventos críticos mediante reglas configurables y notifique a familiares o cuidadores a través de múltiples canales externos, priorizando disponibilidad, tolerancia a fallas y baja latencia en la entrega de alertas.

El proyecto estará orientado principalmente al diseño arquitectónico del sistema, incluyendo la definición de subsistemas, contratos e integración entre componentes, más que a la implementación completa de una solución productiva.

## Stakeholders principales

* Adulto mayor
* Familiar
* Cuidador profesional
* Administrador del sistema

## Alcance

### Incluye

* Recepción de eventos desde un wearable simulado.
* Procesamiento y detección de eventos críticos mediante reglas configurables.
* Dashboard de monitoreo para seguimiento de usuarios.
* Generación y envío de alertas multicanal.

### Fuera de alcance

* Diagnóstico médico.
* Desarrollo de hardware físico real.
* Integración con sistemas hospitalarios o expedientes clínicos.

## Acceso al dominio

La problemática asociada al cuidado de adultos mayores es ampliamente conocida y documentada, lo que permite fundamentar decisiones de diseño y arquitectura de software basadas en necesidades y escenarios reales del dominio.

## Complejidad cubierta

La propuesta cumple con los siguientes criterios de complejidad:

* Múltiples actores con responsabilidades diferenciadas.
* Persistencia no trivial de eventos, alertas y datos históricos de monitoreo.
* Distribución y tolerancia a fallas mediante una arquitectura distribuida.
* Subsistemas con fronteras claras e interfaces definidas.
* Integración con servicios externos de notificación (correo electrónico, SMS o mensajería).
* Atributos de calidad en tensión, especialmente disponibilidad, rendimiento y confiabilidad.
