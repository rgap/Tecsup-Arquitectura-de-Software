# Solucion del Taller 2 - Drivers de TicketPe

**Nombre: REL GUZMAN**

## Estructuras usadas

Segun las diapositivas

- Requisito funcional: **[Actor] puede [accion] [objeto]**.
- Escenario de calidad: **Fuente o Estimulo o Artefacto o Entorno o Respuesta o Medida**.
- Todo valor inventado se marca como **`[SUPUESTO]`** y se justifica.
- Cada restriccion indica su **categoria**.

## Parte B.1 - Requisitos funcionales

| ID    | Actor                  | Requisito funcional                                                            |
| ----- | ---------------------- | ------------------------------------------------------------------------------ |
| RF-01 | **Promotor**           | El promotor **puede registrar** un evento con zonas, precios y fecha de venta. |
| RF-02 | **Administrador**      | El administrador **puede aprobar** la publicacion de un evento.                |
| RF-03 | **Comprador**          | El comprador **puede buscar** eventos por nombre, categoria, ciudad o fecha.   |
| RF-04 | **Comprador**          | El comprador **puede seleccionar** una zona y hasta 4 asientos por evento.     |
| RF-05 | **Personal de puerta** | El personal de puerta **puede validar** una entrada mediante su codigo QR.     |
| RF-06 | **Administrador**      | El administrador **puede anular** una entrada y solicitar su reembolso.        |

## Parte B.2 - Escenarios de calidad

### EC-01 - Rendimiento

| #   | Casilla    | Respuesta                                                                                                                                   |
| --- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Fuente     | 50 000 compradores concurrentes                                                                                                             |
| 2   | Estimulo   | Solicitan reservar asientos al abrirse la venta.                                                                                            |
| 3   | Artefacto  | Componente de inventario y reserva de asientos.                                                                                             |
| 4   | Entorno    | Durante los primeros 3 minutos de venta de un evento masivo.                                                                                |
| 5   | Respuesta  | El sistema confirma o rechaza cada reserva sin vender dos veces el mismo asiento.                                                           |
| 6   | **Medida** | El **95%** de las solicitudes responde en **menos de 2 segundos**, con **0 asientos duplicados** y **menos de 1% de errores** del servidor. |

Checklist:

- [x] Hay al menos un numero en la medida.
- [x] Un tester puede aprobarlo o rechazarlo.
- [x] No aparece ninguna tecnologia.
- [x] El entorno representa una condicion dificil.
- [x] Los numeros salen del PDF de la sesion.

### EC-02 - Disponibilidad

| #   | Casilla    | Respuesta                                                                                                                           |
| --- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Fuente     | Pasarela externa de pagos.                                                                                                          |
| 2   | Estimulo   | Deja de responder mientras se intenta realizar un pago.                                                                             |
| 3   | Artefacto  | Proceso de compra y pago.                                                                                                           |
| 4   | Entorno    | Durante el pico de salida a la venta.                                                                                               |
| 5   | Respuesta  | El sistema mantiene la navegacion y las reservas, conserva los pagos pendientes e informa al comprador.                             |
| 6   | **Medida** | **0 reservas confirmadas perdidas** y recuperacion automatica en **menos de 60 segundos** desde que la pasarela vuelve a responder. |

Checklist:

- [x] Hay al menos un numero en la medida.
- [x] Un tester puede aprobarlo o rechazarlo.
- [x] No aparece ninguna tecnologia.
- [x] El entorno representa una condicion dificil.
- [x] Los numeros salen del PDF de la sesion.

### EC-03 - Seguridad

| #   | Casilla    | Respuesta                                                                                                      |
| --- | ---------- | -------------------------------------------------------------------------------------------------------------- |
| 1   | Fuente     | Script automatizado de reventa.                                                                                |
| 2   | Estimulo   | Intenta realizar mas de 30 reservas por minuto desde una misma identidad.                                      |
| 3   | Artefacto  | Proceso de reserva de entradas.                                                                                |
| 4   | Entorno    | Durante los primeros minutos de una venta masiva.                                                              |
| 5   | Respuesta  | El sistema detecta y bloquea la actividad automatizada sin impedir la compra de la mayoria de usuarios reales. |
| 6   | **Medida** | Bloquea **mas del 90% de los bots** y afecta a **menos del 2% de los compradores legitimos**.                  |

Checklist:

- [x] Hay al menos un numero en la medida.
- [x] Un tester puede aprobarlo o rechazarlo.
- [x] No aparece ninguna tecnologia.
- [x] El entorno representa una condicion dificil.
- [x] Los numeros salen del PDF de la sesion.

### EC-04 - Mantenibilidad

| #   | Casilla    | Respuesta                                                                                                                           |
| --- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Fuente     | Area de negocio de TicketPe.                                                                                                        |
| 2   | Estimulo   | Solicita incorporar un nuevo medio de pago.                                                                                         |
| 3   | Artefacto  | Componente de integracion de pagos.                                                                                                 |
| 4   | Entorno    | Con la plataforma en produccion y las ventas actuales en curso.                                                                     |
| 5   | Respuesta  | Un desarrollador integra el nuevo medio sin modificar los medios de pago existentes.                                                |
| 6   | **Medida** | **Menos de 3 dias** de trabajo, **0 archivos existentes modificados** y **mas de 80% de cobertura de pruebas** para el nuevo medio. |

Checklist:

- [x] Hay al menos un numero en la medida.
- [x] Un tester puede aprobarlo o rechazarlo.
- [x] No aparece ninguna tecnologia.
- [x] El entorno incluye la condicion de no detener la operacion.
- [x] Los numeros salen del PDF de la sesion.

## Parte B.3 - Restricciones

| ID   | Restriccion                                                                | Categoria          | De donde sale                                       |
| ---- | -------------------------------------------------------------------------- | ------------------ | --------------------------------------------------- |
| R-01 | TicketPe no puede almacenar numeros de tarjeta en su base de datos.        | **Normativa**      | Norma de la industria de pagos indicada en el caso. |
| R-02 | El proyecto cuenta con 6 desarrolladores y una persona SRE a medio tiempo. | **De equipo**      | Condiciones de operacion de TicketPe.               |
| R-03 | El rediseño debe completarse en 9 meses.                                   | **De plazo/costo** | Presupuesto y condiciones del proyecto.             |

## Parte B.4 - Prioridad ISO/IEC 25010

| Caracteristica          | Prioridad | Por que                                                                                                              |
| ----------------------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| Adecuacion funcional    | Alta      | Debe cubrir la publicacion, venta, pago, emision, validacion y reembolso de entradas.                                |
| Eficiencia de desempeño | Critica   | Debe atender decenas de miles de compradores durante los primeros 3 minutos y validar cada QR en menos de 1 segundo. |
| Compatibilidad          | Alta      | Debe comunicarse con pasarelas de pago, billeteras digitales y servicios de correo.                                  |
| Usabilidad              | Media     | Una compra sencilla ayuda a evitar abandonos, pero no es el principal riesgo arquitectonico.                         |
| Fiabilidad              | Critica   | Una caida o la venta duplicada de un asiento produce reembolsos, penalidades y daño reputacional.                    |
| Seguridad               | Alta      | Debe proteger datos, frenar bots y conservar la trazabilidad de pagos y entradas.                                    |
| Mantenibilidad          | Alta      | Solo hay 6 desarrolladores y el sistema debe evolucionar sin detener la operacion.                                   |
| Portabilidad            | Baja      | El caso no exige cambiar de proveedor ni ejecutar el sistema en instalaciones de clientes.                           |

Solo se marcaron como criticas las dos caracteristicas que mas condicionan la arquitectura: rendimiento y fiabilidad.

## Ejercicio individual - Sistema de matricula academica

Este ejemplo usa valores estimados porque no se proporcionaron datos de un sistema de trabajo real.

### EC-I01 - Rendimiento

| #   | Casilla    | Respuesta                                                                                                                                                                          |
| --- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Fuente     | Estudiantes de una institucion educativa.                                                                                                                                          |
| 2   | Estimulo   | Intentan registrar sus cursos al abrirse la matricula.                                                                                                                             |
| 3   | Artefacto  | Proceso de consulta de vacantes y matricula.                                                                                                                                       |
| 4   | Entorno    | Durante los primeros 10 minutos del inicio de matricula.                                                                                                                           |
| 5   | Respuesta  | El sistema muestra las vacantes y confirma o rechaza cada solicitud sin superar el cupo del curso.                                                                                 |
| 6   | **Medida** | Con **500 estudiantes concurrentes**, el **95%** de las solicitudes responde en **menos de 3 segundos**, con **0 cupos duplicados** y **menos de 1% de errores**. **`[SUPUESTO]`** |

**Razon del supuesto:** se usa un pico de 500 estudiantes como una magnitud razonable para una institucion mediana. Los 3 segundos permiten comprobar que el proceso sigue siendo fluido durante la mayor demanda.
