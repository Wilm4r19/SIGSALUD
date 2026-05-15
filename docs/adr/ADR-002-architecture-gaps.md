# ADR-002: Mitigación de fallos silenciosos en la integración

## Estado
Propuesto

## Contexto
En el servicio HIS, la función `dispatchOrderToDiagnosticSystem` captura los errores de red al enviar órdenes a LIS/RIS, los registra en la base de datos, pero no detiene el flujo ni notifica al usuario en la interfaz. Esto puede dejar órdenes en estado inconsistente.

## Decisión
Proponer la implementación de un mecanismo de **reintentos automáticos (Retry Pattern)** o, en su defecto, cambiar el estado de la orden a 'ERROR_ENVIO' y notificar visualmente al médico en el HIS para que intente el reenvío manual.

## Alternativas consideradas
- **Dejar como está:** Riesgo alto de pérdida de órdenes.
- **Hacer la llamada síncrona y bloquear:** Bloquearía la experiencia del usuario si el sistema destino está lento.

## Consecuencias
- **Positivas:** Mayor confiabilidad en la entrega de órdenes. Trazabilidad de fallos.
- **Negativas:** Requiere modificar el esquema de base de datos o agregar lógica de colas (ej. RabbitMQ) para reintentos.

## Riesgos
- Complejidad adicional en un sistema que actualmente es simple (KISS).
