# data-engineering-roadmap-2026

Hoja de ruta practica para avanzar en data engineering durante 2026, con ejercicios por niveles y evidencia del progreso dentro del repositorio.

## Estado actual

Hoy el proyecto tiene una base funcional para el `nivel-0`:

- entorno Python con dependencias para Jupyter, SQL y analisis de datos
- `docker-compose` con `postgres:13` y `pgadmin4`
- notebook inicial para validar conectividad con Postgres usando `psql`
- archivo de datos `yellow_tripdata_2024-01.parquet` disponible localmente

## Estructura

```text
data-engineering-roadmap-2026/
|- README.md
|- BITACORA.md
`- nivel-0/
   |- docker-compose.yml
   |- requirements.txt
   |- test.ipynb
   `- yellow_tripdata_2024-01.parquet
```

## Progreso verificado

Lo que ya se puede comprobar en el repositorio:

- `nivel-0/docker-compose.yml` define una red `bridge` compartida para `postgres` y `pgadmin`
- `nivel-0/test.ipynb` valida conexion al motor con `SELECT 1`
- el mismo notebook consulta `information_schema.tables`, confirmando acceso SQL basico
- `nivel-0/requirements.txt` deja preparado el stack para trabajo interactivo con notebooks, SQL y `pyarrow/pandas`
- `.gitignore` evita subir el parquet local y artefactos comunes de Python/Jupyter

## Pendientes sugeridos para cerrar nivel-0

- cargar el parquet en una tabla de trabajo dentro de Postgres
- documentar el flujo exacto de arranque y conexion
- agregar consultas de validacion sobre el dataset cargado
- separar exploracion, carga y validacion en notebooks o scripts mas claros

## Bitacora

El seguimiento del avance vive en `BITACORA.md`. La idea es registrar cambios concretos, hitos alcanzados, bloqueos y siguientes pasos para tener trazabilidad del trabajo real y no solo del codigo final.
