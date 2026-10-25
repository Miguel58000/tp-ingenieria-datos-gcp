# Notas del TP

## Parte 1 · Capa raw

### Decisiones de schema
- Dataset `tp_raw` en ubicación US. Cada tabla se carga con su schema declarado (`schemas/viajes.json` y `schemas/clima.json`).
- Las horas están en hora local de Nueva York, sin zona horaria. Por eso las fechas son `DATETIME` y no `TIMESTAMP`.
- Modo de las columnas: `REQUIRED` en las que no tienen nulos en el archivo, `NULLABLE` en las que sí.
  - `viajes`: todas `REQUIRED` salvo `passenger_count` (`NULLABLE`). Esto se debe a que `passenger_count` presenta campos nulos que hubieran impedido la carga si ese campo se hubiera mantenido como required
  - `clima`: todas `REQUIRED` 
- `dropoff_datetime` se carga como `STRING` porque el archivo mezcla dos formatos de fecha (ver abajo).

### Verificación de la carga
- `tp_raw.viajes`: 399.634 filas 
- `tp_raw.clima`: 676 filas 

### Hallazgos de calidad

**viajes**
- `dropoff_datetime` mezcla formato ISO (`2026-09-14 00:26:00`) y formato día/mes/año (`14/09/2026 00:27:01`). Hay que parsear ambos formatos al limpiar.
- `trip_distance` con valores imposibles: no son solo los 12 mayores (15.655 a 172.042 millas). El valor 13.º más grande es 558,18 millas, también imposible, por lo que el problema es más amplio. Tarifas de esos viajes normales (8 a 39 USD): el error está en la distancia, no en el viaje.
  - Viajes con más de 100 millas: 22
  - Viajes con velocidad implícita > 100 mph: 115
  - Criterio propuesto para la limpieza: excluir distancias > 100 millas (máximo razonable aun saliendo de la ciudad) y velocidades implícitas > 100 mph. 
  - Filas en formato ISO: 321932
  - Filas en formato d/m/Y: 77702
  - Otros formatos: 0
- `passenger_count` tiene nulos: 94473. También ceros: 2061.
- `payment_type` aparece con valor `0` en la muestra, que no es un código estándar. Distribución: 
- Distancias en cero o negativas: 11347
- Tarifas en cero o negativas: 23094
- Viajes fuera del período 14/09–11/10: 0
- Duración 0: 8913
- Duplicados exactos: 1633
- Valores extremos (distancia máxima, total máximo, duración > 3 h): 172042.74 millas (valor), US$ 668.25 y 125 viajes con una duración mayor a 3 hs

**clima**
- Se esperan 672 horas (28 días × 24) y hay 676 filas. Horas distintas: 672. Horas repetidas: 4. Horas faltantes: 0
**clima**
- 676 filas para 672 horas esperadas (28 días × 24). No faltan horas del período 14/09–11/10; sobran 4 filas por repetición.
- Horas repetidas (2 filas cada una): 2026-09-24 09:00, 10:00 y 11:00, y 2026-10-03 18:00.
  - Las filas repetidas son idénticas 
- Pendiente: resolver las repetidas antes de cruzar con viajes por hora de subida; de lo contrario el cruce duplicaría viajes.
- Rangos de las mediciones (calculados sobre las 676 filas, incluidas las 4 repetidas):
  - Temperatura: de 5,1 a 29,6 °C (promedio 19,1 °C).
  - Precipitación: de 0,0 a 15,4 mm por hora (promedio 0,15 mm). Hubo lluvia en 75 de 676 filas.
  - Viento: de 0,4 a 24,7 km/h (promedio 8,1 km/h).
  - Ningún valor fuera de rango físico: no hay negativos ni extremos. La única anomalía del clima son las horas repetidas.

### Pendiente para las próximas partes
- Parsear `dropoff_datetime` a `DATETIME`.
- Decidir qué hacer con los valores inválidos de viajes (filtrar vs. corregir).
- Resolver las horas repetidas del clima antes de cruzarlo con viajes (el cruce se haría por hora de subida).