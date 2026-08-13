Como arqs definimos: nueva arq q aguante eventos masivos sin tirar abajo lo q ya funciona

escalar

//////////

- atributos de calidad: ESCALABILIDAD, CONSISTENCIA, DISPONIBILIDAD

/////////

q sucede ahora?
ya vende entradas. la arq se quedo corta para crecimiento

Para eventos chicos:
antes: 500 a 2000 personas por evento
ahora: 40,000 entradas disponibles.

Ahora eventos mas grandes:
hasta 120,000 simultaneas intentando comprar cuando se abre la venta

//////////

Paso 1: desde el punto de vista de stakeholder

Que cargue rapido desde ...

Stakeholders: quien responde si hay multa, quien paga nube, y usuario final

Paso 2: decisiones arquitectonicas

- No son funcionalidades
- por ejemplo, netflix permite .... pero que hay por detras? -> el envio es asincrono, el cliente guarda el mensaje localmente y lo reintenta; no espera confirmación del servidor para darte respuesta?

Nombrar observaciones

Paso 3: tradeoff

que gano y a que costo

Paso 4: la tensión

stakeholders buscan cosas compatibles pero incompatibles?

