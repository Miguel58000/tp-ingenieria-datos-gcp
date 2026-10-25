# Trabajo práctico integrador · Ingeniería y Análisis de Datos en Google Cloud

UTN Facultad Regional Rosario x TheLab Technology · Octubre–Noviembre 2026

**Autor:** Miguel Rodríguez

## Descripción

Pipeline de datos sobre BigQuery con viajes de taxi amarillo de Nueva York (14/09/2026 al 11/10/2026) y datos de clima por hora del mismo período. El trabajo se construye por partes a lo largo del curso sobre este mismo repositorio.

El proyecto se basa en las consignas y fuentes del repositorio original: juanmantegazza/ingenieria-datos-gcp.

## Estado

| Parte | Contenido | Estado |
|---|---|---|
| 1 | Capa raw: schemas, dataset `tp_raw` y carga de los dos archivos | Completa |

## Estructura del repositorio

```
.
├── README.md
├── NOTAS.md          # decisiones y hallazgos de calidad de datos
├── schemas/
│   ├── viajes.json
│   └── clima.json
└── .gitignore        # excluye los CSV
```

Los archivos `viajes.csv` y `clima.csv` no se incluyen en el repositorio. Están en la carpeta `tp/datos/` del repositorio de la cátedra.

## Parte 1 · Capa raw

- **Proyecto:** curso-datos-utn 
- **Dataset:** `tp_raw`, ubicación `US`
- **Tablas:** `tp_raw.viajes` (399.634 filas) y `tp_raw.clima` (676 filas)

### Decisiones principales

- Las horas están en hora local de Nueva York, sin zona horaria, por lo que las fechas se declaran como `DATETIME`.
- `dropoff_datetime` se carga como `STRING` porque el archivo mezcla dos formatos de fecha (ISO y día/mes/año). Se parsea en la capa de limpieza.
- Las columnas son `REQUIRED` si no tienen nulos en el archivo. La única `NULLABLE` es `passenger_count`.
- La capa raw conserva los datos tal cual vienen: no se elimina ni se corrige nada. Los problemas detectados se documentan en `NOTAS.md`.

### Cómo reproducir la carga

Requiere `gcloud` y `bq` configurados con el proyecto. Con los CSV en la raíz del repositorio:

```cmd
bq --location=US mk --dataset tp_raw

bq load --replace --location=US --source_format=CSV --skip_leading_rows=1 tp_raw.viajes viajes.csv schemas\viajes.json

bq load --replace --location=US --source_format=CSV --skip_leading_rows=1 tp_raw.clima clima.csv schemas\clima.json
```

Verificación:

```sql
SELECT
  (SELECT COUNT(*) FROM tp_raw.viajes) AS viajes,
  (SELECT COUNT(*) FROM tp_raw.clima) AS clima;
```

Resultado esperado: 399.634 y 676.

## Calidad de datos

El detalle de los hallazgos (formatos de fecha mezclados, distancias imposibles, tarifas negativas, duplicados, horas de clima repetidas) está en [`NOTAS.md`](NOTAS.md).

## Fuentes de los datos

- Viajes: NYC Taxi & Limousine Commission (muestra de septiembre y octubre de 2025, fechas corridas un año).
- Clima: Open-Meteo, datos históricos por hora.

Los archivos fueron modificados para el curso.