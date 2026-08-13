# Taller 1 — Autopsia arquitectónica


## Objetivo

Deducir decisiones arquitectónicas de un sistema **observándolo desde afuera**, sin acceso a su código. Es el ejercicio que hace un arquitecto cuando entra a un proyecto que no conoce: mirar el comportamiento y preguntarse qué estructura lo hace posible.

## Regla del taller

**No busques en internet cómo está hecho el sistema.** El valor del ejercicio está en razonar desde lo observable. Equivocarse está permitido y es parte del punto: en la puesta en común discutimos las hipótesis, no las respuestas correctas.

---

## Sistema a analizar

Elige **uno** que uses de verdad. Sugerencias: Yape, Netflix, Rappi, Uber, WhatsApp, Spotify, la banca por internet de tu banco, o la intranet de tu trabajo.

Si eliges un sistema de tu trabajo, no incluyas nombres de la empresa ni datos confidenciales.

---

## Qué entregar

### Paso 1 — Los stakeholders (3)

Quiénes son y qué le piden al sistema. Escríbelo **con sus palabras, entre comillas**, no en lenguaje técnico.

| Stakeholder | Qué le pide al sistema ("con sus palabras") |
|---|---|
|xxxxxxxxxxx |xxxxxxxx |
|xxxxxxxxxxx |xxxxxxxxxxx |
|xxxxxxxxxxx | xxxxxxxxxxx|

### Paso 2 — Las decisiones arquitectónicas (3)

Cosas que puedes **inferir desde afuera**. Una decisión arquitectónica no es una funcionalidad: es algo que obliga a que exista cierta estructura por debajo.

| # | Qué observaste | Qué decisión arquitectónica infieres |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |

> **Prueba de control:** si lo que escribiste en la columna derecha se puede leer como "el sistema permite hacer X", es una funcionalidad, no una decisión. Reescríbela respondiendo: *¿y eso qué obliga a que exista por debajo?*

### Paso 3 — El trade-off de cada decisión

| # | Qué ganó el sistema | Qué está pagando por ello |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |

### Paso 4 — La tensión

Elige dos stakeholders de tu Paso 1 que quieran cosas **legítimas pero incompatibles**.

- **Stakeholder A quiere:**
- **Stakeholder B quiere:**
- **Por qué son incompatibles:**
- **Cómo crees que el sistema la resuelve:**

---

## Ejemplo resuelto (WhatsApp)

**Observación.** Cuando envías un mensaje sin señal, la app lo acepta, muestra un reloj y lo entrega después. El check gris aparece antes que el azul.

| Campo | Contenido |
|---|---|
| **Decisión inferida** | El envío es asíncrono. El cliente guarda el mensaje localmente y lo reintenta; no espera confirmación del servidor para darte respuesta. |
| **Qué gana** | La app se siente instantánea y usable con mala conectividad, que es la condición real de la mayoría de sus usuarios en el mundo. |
| **Qué paga** | Complejidad enorme en el cliente: cola local, reintentos, orden de mensajes, deduplicación y resolución de conflictos entre dispositivos. |
| **Tensión detectada** | El usuario quiere saber que el mensaje llegó; el sistema no puede garantizarlo al instante. Se resuelve mostrando estados intermedios en lugar de mentir con un solo "enviado". |


