# Caso TicketPe

**Arquitectura de Software · Módulo 1 · Caso hilo conductor**
Lectura obligatoria antes de la Sesión 2 

---

## 1. El negocio

**TicketPe** es una plataforma peruana de venta de entradas para conciertos, teatro y eventos deportivos. Lleva tres años operando con eventos medianos (teatros de 500 a 2 000 localidades) y acaba de firmar contratos con dos promotores grandes que llenan el Estadio Nacional y el Jockey Club.

El sistema actual es un monolito en PHP con una sola base de datos MySQL, desarrollado por dos personas que ya no trabajan en la empresa. Funciona para los eventos chicos. Con el primer evento grande se cayó durante 40 minutos y se vendieron 118 asientos por duplicado, que la empresa tuvo que reembolsar con una penalidad.

Te contratan como arquitecto de software para rediseñar la plataforma.

---

## 2. Actores del sistema

| Actor | Quién es | Qué hace en el sistema |
|---|---|---|
| **Comprador** | Público general | Busca eventos, elige asientos, paga y recibe su entrada |
| **Promotor** | Empresa que organiza el evento | Publica el evento, define aforo, precios y fechas; consulta ventas |
| **Personal de puerta** | Staff del recinto | Escanea códigos QR y valida el ingreso el día del evento |
| **Analista de finanzas** | Área interna de TicketPe | Concilia lo cobrado con lo emitido y liquida a los promotores |
| **Administrador** | Área interna de TicketPe | Gestiona promotores, comisiones, reembolsos y bloqueos |
| **Operaciones / SRE** | Área interna de TicketPe | Despliega, monitorea y responde ante incidentes |

---

## 3. Qué debe hacer el sistema

### 3.1 Publicación de eventos
- El promotor registra un evento: nombre, recinto, fecha, hora, descripción e imágenes.
- Define zonas (platea, tribuna, campo) con su aforo y precio por zona.
- Algunos recintos venden **por asiento numerado** y otros **por zona sin numerar**. El sistema debe soportar ambos.
- Define la fecha y hora exacta de apertura de venta al público, y opcionalmente una **preventa** restringida a un grupo (por ejemplo, clientes de un banco).
- Un evento se publica solo después de una aprobación interna del administrador.

### 3.2 Venta de entradas
- El comprador busca eventos por nombre, categoría, ciudad o fecha.
- Selecciona zona y, si aplica, asientos específicos en un mapa.
- Los asientos seleccionados quedan **retenidos** mientras el comprador completa el pago. La retención expira si no paga.
- Se paga con tarjeta de crédito o débito, o con billetera digital. TicketPe no procesa el pago: lo delega a una pasarela externa.
- Al confirmarse el pago se emiten las entradas: una por asiento, cada una con un código QR único y el nombre del titular.
- Las entradas se envían por correo electrónico y quedan disponibles en la cuenta del comprador.
- Un comprador no puede adquirir más de 4 entradas por evento.

### 3.3 Control de acceso
- El día del evento, el personal de puerta escanea el QR con una app móvil.
- El sistema responde si la entrada es válida, ya fue usada, corresponde a otro evento o fue anulada.
- Una entrada válida solo puede usarse **una vez**.

### 3.4 Liquidación y reportes
- Finanzas concilia diariamente los cobros de la pasarela contra las entradas emitidas.
- El promotor consulta cuántas entradas ha vendido, por zona y por tramo de tiempo.
- El administrador puede anular una entrada y disparar un reembolso.

---

## 4. Condiciones de operación

Estas son las condiciones reales bajo las que el sistema debe funcionar. **Son la parte importante del caso.**

### 4.1 El pico de salida a la venta
El escenario que define la arquitectura:

> Viernes, 10:00:00 am. Sale a la venta un concierto en el Estadio Nacional. **40 000 localidades**. Se han registrado **120 000 personas interesadas** que reciben una notificación y abren la página al mismo tiempo. En los primeros 3 minutos se venden 32 000 entradas y se procesan alrededor de 90 000 intentos de compra que no prosperan.

Fuera de estos picos, el sistema atiende entre 200 y 2 000 visitas por hora.

### 4.2 Otras condiciones
- **Sobreventa:** vender dos veces el mismo asiento numerado es inaceptable. Cada caso implica reembolso, penalidad contractual y daño reputacional.
- **Cobro duplicado:** un usuario que da doble clic, o una red que reintenta la petición, no puede terminar con dos cargos.
- **Conectividad en puerta:** el Estadio Nacional tiene señal móvil saturada el día del evento. La validación de QR debe funcionar con conectividad intermitente y sincronizar después.
- **Velocidad en puerta:** el ingreso de 40 000 personas ocurre en unas 2 horas por 12 puntos de acceso. Cada escaneo debe resolverse en menos de 1 segundo.
- **Reventa y bots:** en el último evento, se estima que el 60% del tráfico del pico fueron scripts automatizados de revendedores.
- **Datos de tarjeta:** por normativa de la industria de pagos, TicketPe no puede almacenar números de tarjeta en su base de datos.
- **Auditoría:** finanzas debe poder demostrar, para cualquier entrada emitida, qué pago la originó y en qué momento.
- **Equipo:** TicketPe tiene 6 desarrolladores. No hay equipo de infraestructura dedicado; una persona hace de SRE a medio tiempo.
- **Presupuesto:** el rediseño tiene 9 meses y no puede detener la operación actual.

---

## 5. Tensiones conocidas

Estas contradicciones son reales y **no tienen una solución que deje contentos a todos**. Aparecerán una y otra vez durante el módulo.

| Tensión | Lado A | Lado B |
|---|---|---|
| Reportes vs. estabilidad | El promotor quiere ver ventas en tiempo real | Consultar la base de ventas durante el pico es lo que la tumba |
| Retención vs. disponibilidad | Bloquear el asiento evita que dos personas lo compren | Asientos bloqueados por gente que nunca paga son ventas perdidas |
| Antifraude vs. conversión | Doble factor y CAPTCHA frenan a los bots | También hacen que compradores reales abandonen |
| Consistencia vs. velocidad | Verificar el inventario real en cada clic es exacto | Es también lo más lento y lo primero que colapsa |
| Simplicidad vs. escala | 6 desarrolladores no pueden operar 20 microservicios | Un monolito único no aguanta el pico sin sobredimensionarse |

