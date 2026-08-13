# Taller 2 — Hoja de trabajo

**Arquitectura de Software · Módulo 1 · Sesión 2**
Los drivers arquitectónicos de TicketPe

---

---

## Las tres reglas

1. **Si tu requisito nombra una tecnología, todavía no es un requisito.** Redis, Kafka, Kubernetes y compañía se deciden en la próxima sesión, no aquí.
2. **Sin número no hay atributo de calidad.** Una medida sin cifra es una opinión.
3. **Los supuestos se declaran.** Cuando inventes un número, márcalo `[SUPUESTO]` y explica en una línea por qué elegiste ese valor.

---

# PARTE A — Resuelto en clase

Este escenario lo construimos juntos. Queda aquí como **modelo del nivel que se espera** en los cuatro que siguen.

> **El promotor quiere ver sus ventas en tiempo real durante la salida a la venta.**
>
> Recuerda la tensión de la sesión anterior: esto es exactamente lo que puede tumbar el sistema durante el pico.

### EC-00 · OBSERVABILIDAD DE NEGOCIO *(resuelto)*

| # | Casilla | La pregunta que responde | Respuesta |
|---|---|---|---|
| 1 | **Fuente** | ¿Quién o qué lo provoca? | El promotor del evento |
| 2 | **Estímulo** | ¿Qué ocurre exactamente? | Consulta el total vendido y la ocupación por zona, y recarga cada pocos minutos |
| 3 | **Artefacto** | ¿Sobre qué parte del sistema? | El panel de reportes del promotor |
| 4 | **Entorno** | ¿En qué condición está el sistema? | Durante los primeros 15 minutos de la salida a la venta, con el pico de compra en curso |
| 5 | **Respuesta** | ¿Qué debe hacer el sistema? | Entregar las cifras por zona sin consultar la base de datos de ventas en vivo, y sin degradar el tiempo de respuesta de la compra |
| 6 | **Medida** | ¿Cómo lo verifico? | Datos con no más de **30 s** de atraso · el panel responde en menos de **3 s** · **0 impacto medible** en la latencia de reserva de asientos |

### Lo que hay que entender de este escenario

El promotor pidió **"tiempo real"**. Nadie discutió si eso era posible: se preguntó **cuánto atraso tolera de verdad**. Y aceptó 30 segundos.

Esa única casilla cambió todo:

| Si la medida fuera | Consecuencia técnica |
|---|---|
| 0 s de atraso | Hay que consultar la base de ventas en vivo, justo durante el pico. Es exactamente lo que tumba el sistema |
| 30 s de atraso | Se puede servir desde una vista aparte que se actualiza sola. El panel del promotor no toca la ruta de compra |

Fíjate también en la **casilla 5**: dice qué debe pasar *y* qué no debe romperse. Cuando un escenario resuelve una tensión entre dos stakeholders, la respuesta tiene que proteger explícitamente al otro lado.

> **Y nota lo que no aparece:** ninguna tecnología. No dice caché, ni réplica, ni base de lectura. Solo dice qué propiedad se quiere. **El cómo se decide en la próxima sesión.**

---

# PARTE B — Lo que completas tú

## B.1 · Seis requisitos funcionales

Formato: **[Actor] puede [acción] [objeto] [condición opcional]**
Verbo en presente, voz activa, un solo comportamiento por requisito.

| ID | Actor | Requisito funcional |
|---|---|---|
| RF-01 | | |
| RF-02 | | |
| RF-03 | | |
| RF-04 | | |
| RF-05 | | |
| RF-06 | | |

> **Control:** si tu requisito contiene "rápido", "fácil", "seguro" o "escalable", no es funcional. Muévelo a la parte B.2.

---

## B.2 · Cuatro escenarios de calidad

Uno de cada tipo. **Usa el EC-00 de la Parte A como referencia del nivel esperado**, pero no lo repitas: los cuatro tienen que ser situaciones distintas.

### EC-01 · RENDIMIENTO

| # | Casilla | Tu respuesta |
|---|---|---|
| 1 | Fuente | |
| 2 | Estímulo | |
| 3 | Artefacto | |
| 4 | Entorno | |
| 5 | Respuesta | |
| 6 | Medida | |

**Checklist** — marca solo lo que se cumple de verdad:

- [ ] Hay al menos un número en la medida
- [ ] Un tester podría aprobarlo o rechazarlo sin discutir
- [ ] No aparece ninguna tecnología
- [ ] El entorno es la condición difícil, no la normal
- [ ] El número sale del enunciado, de una estimación razonada, o está marcado `[SUPUESTO]`

---

### EC-02 · DISPONIBILIDAD

| # | Casilla | Tu respuesta |
|---|---|---|
| 1 | Fuente | |
| 2 | Estímulo | |
| 3 | Artefacto | |
| 4 | Entorno | |
| 5 | Respuesta | |
| 6 | Medida | |

**Checklist:**

- [ ] Hay al menos un número en la medida
- [ ] Un tester podría aprobarlo o rechazarlo sin discutir
- [ ] No aparece ninguna tecnología
- [ ] El entorno es la condición difícil, no la normal
- [ ] El número está respaldado o marcado como supuesto

> **Pista:** el escenario interesante de disponibilidad no es "el sistema está caído". Es "algo de lo que dependemos dejó de responder y nosotros seguimos funcionando".

---

### EC-03 · SEGURIDAD

| # | Casilla | Tu respuesta |
|---|---|---|
| 1 | Fuente | |
| 2 | Estímulo | |
| 3 | Artefacto | |
| 4 | Entorno | |
| 5 | Respuesta | |
| 6 | Medida | |

**Checklist:**

- [ ] Hay al menos un número en la medida
- [ ] Un tester podría aprobarlo o rechazarlo sin discutir
- [ ] No aparece ninguna tecnología
- [ ] El entorno es la condición difícil, no la normal
- [ ] El número está respaldado o marcado como supuesto

> **Pista:** en seguridad, una sola medida casi nunca alcanza. Si dices cuántos atacantes bloqueas, di también a cuántos usuarios legítimos estás dispuesto a molestar.

---

### EC-04 · MANTENIBILIDAD

| # | Casilla | Tu respuesta |
|---|---|---|
| 1 | Fuente | |
| 2 | Estímulo | |
| 3 | Artefacto | |
| 4 | Entorno | |
| 5 | Respuesta | |
| 6 | Medida | |

**Checklist:**

- [ ] Hay al menos un número en la medida
- [ ] Un tester podría aprobarlo o rechazarlo sin discutir
- [ ] No aparece ninguna tecnología
- [ ] El entorno es la condición difícil, no la normal
- [ ] El número está respaldado o marcado como supuesto

> **Pista:** este es el más difícil de imaginar, porque no habla de segundos. La fuente suele ser un desarrollador o el negocio, y la medida se cuenta en días de trabajo, archivos modificados o cobertura de pruebas.

---

## B.3 · Tres restricciones

De **categorías distintas**. Elige entre: normativa, técnica, de negocio, de equipo, de plazo/costo.

| ID | Restricción | Categoría | De dónde sale |
|---|---|---|---|
| R-01 | | | |
| R-02 | | | |
| R-03 | | | |

> **Pista:** las restricciones de equipo y de plazo son las que casi nadie escribe, y suelen ser las que más deciden. Relee la sección 4 del enunciado de TicketPe.

---

## B.4 · Tabla de prioridad ISO/IEC 25010

Recorre las ocho características y marca cuánto pesa cada una **en TicketPe**, con una línea de justificación.

| Característica | Prioridad | Por qué |
|---|---|---|
| Adecuación funcional | | |
| Eficiencia de desempeño | | |
| Compatibilidad | | |
| Usabilidad | | |
| Fiabilidad | | |
| Seguridad | | |
| Mantenibilidad | | |
| Portabilidad | | |

Prioridad: **crítica · alta · media · baja**

> **Advertencia:** si marcas más de tres como críticas, no has priorizado. Decir que todo es crítico equivale a decir que nada lo es.

---

## De dónde sacar los números

No tienes cliente a quien preguntar. Usa estas tres fuentes, en este orden:

**1. Del enunciado.** 40 000 localidades · 120 000 interesados · 3 minutos de venta · 12 puntos de acceso · 2 horas de ingreso · 6 desarrolladores · 9 meses · 200 a 2 000 visitas por hora fuera de los picos.

**2. De una estimación razonada.** Ejemplo: 40 000 personas ÷ 12 puertas ÷ 120 minutos ≈ 28 escaneos por minuto por puerta. **Escribe el razonamiento, no solo el resultado.**

**3. De un supuesto declarado.** Márcalo `[SUPUESTO]` y explica en una línea por qué elegiste ese valor.

---

## Errores que te van a costar puntos

| Error | Cómo se ve | Cómo se arregla |
|---|---|---|
| Escribir la solución | "Usar una cola virtual con Redis" | "Ante 50 000 solicitudes simultáneas, el sistema mantiene tiempos bajo 2 s" |
| Medida sin número | "Responde rápidamente" | "Responde en menos de 2 s en el percentil 95" |
| Entorno ideal | "En operación normal, con pocos usuarios" | "Durante los primeros 3 minutos de la salida a la venta" |
| Escenario doble | "Es rápido y además seguro" | Pártelo en dos escenarios |
| Todo crítico | Ocho características marcadas como críticas | Máximo tres |

---

## Además, individual

Escribe **un escenario de calidad de 6 partes sobre el sistema donde trabajas.** Real, con números reales o estimados por ti. Si es información confidencial, cambia los nombres pero conserva las magnitudes.

| # | Casilla | Tu respuesta |
|---|---|---|
| 1 | Fuente | |
| 2 | Estímulo | |
| 3 | Artefacto | |
| 4 | Entorno | |
| 5 | Respuesta | |
| 6 | Medida | |

Este es el ejercicio que más aprendizaje genera de todo el módulo, porque te obliga a mirar tu propio sistema con la herramienta nueva.
