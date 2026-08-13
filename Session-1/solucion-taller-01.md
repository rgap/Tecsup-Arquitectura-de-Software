# Solucion del Taller 1 - WhatsApp

## Paso 1 - Stakeholders

| Stakeholder | Que quiere |
|---|---|
| Usuario que envia | "Quiero enviar mensajes rapido, aunque tenga mala señal." |
| Usuario que recibe | "Quiero recibir los mensajes sin errores ni duplicados." |
| Administrador de grupo | "Quiero controlar quienes participan en el grupo." |

## Paso 2 - Decisiones de arquitectura

| # | Que observe | Decision que infiero |
|---|---|---|
| 1 | Puedo enviar un mensaje sin internet y queda pendiente. | La aplicacion guarda el mensaje y lo envia cuando vuelve la conexion. |
| 2 | Los mensajes muestran un reloj y diferentes checks. | El sistema guarda diferentes estados para cada mensaje. |
| 3 | Puedo leer mensajes antiguos sin internet. | La aplicacion guarda una copia de los mensajes en el celular. |

## Paso 3 - Ventajas y costos

| # | Que gana | Que cuesta |
|---|---|---|
| 1 | Funciona con mala conexion. | Debe controlar reintentos y mensajes duplicados. |
| 2 | El usuario sabe si su mensaje llego. | Debe actualizar el estado de cada mensaje. |
| 3 | Permite leer mensajes sin internet. | Usa espacio en el celular y debe sincronizar los datos. |

## Paso 4 - Tension

- **Usuario que envia:** quiere que el mensaje aparezca de inmediato.
- **Usuario que recibe:** quiere recibirlo una sola vez y en orden.
- **Problema:** una mala conexion puede retrasar, repetir o desordenar los mensajes.
- **Solucion:** WhatsApp guarda el mensaje, lo reintenta y muestra su estado con un reloj y checks.
