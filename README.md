# data-engineering-roadmap-2026

Repositorio de aprendizaje aplicado en `Data Engineering` durante 2026. El objetivo es documentar, por niveles, la construccion de fundamentos tecnicos reales: ingesta, modelado inicial, trabajo con entornos locales, SQL y operacion de servicios de datos.

## Objetivo del repositorio

Este repositorio busca mostrar:

- progresion tecnica visible y verificable
- practica con herramientas comunes del ecosistema de datos
- decisiones y bloqueos documentados de forma honesta
- capacidad para iterar sobre problemas reales de entorno, datos y operacion

## Estado actual

El `nivel-0` ya tiene una base funcional y evidencia de trabajo real:

- entorno Python orientado a notebooks, SQL y analisis de datos
- stack local con `Postgres 13` y `pgAdmin 4` levantado con `docker-compose`
- notebook de validacion para conectividad y consultas basicas
- notebook de laboratorio con carga inicial hacia capas `raw` y `curated`
- dataset local `yellow_tripdata_2024-01.parquet` como fuente de trabajo

## Estructura

```text
data-engineering-roadmap-2026/
|- README.md
|- BITACORA.md
`- nivel-0/
   |- docker-compose.yml
   |- LAB — Nivel-0.ipynb
   |- test.ipynb
   `- yellow_tripdata_2024-01.parquet
```

## Tecnologias y herramientas

- Python
- JupyterLab
- PostgreSQL
- pgAdmin
- Docker Compose
- SQL
- pandas
- pyarrow
- jupysql

## Evidencia tecnica

Lo que ya se puede comprobar en el repositorio:

- `nivel-0/docker-compose.yml` define una red `bridge` compartida para `postgres` y `pgadmin`
- `nivel-0/test.ipynb` valida conexion al motor con `SELECT 1` y consultas sobre `information_schema`
- `nivel-0/LAB — Nivel-0.ipynb` crea esquemas `raw` y `curated`
- el notebook de laboratorio crea la tabla `raw.yellow_taxi_raw` y carga datos con `\copy`
- el mismo notebook define la tabla `curated.trip` e intenta poblarla desde `raw.yellow_taxi_raw`
- `.gitignore` ya contempla `venv`, `nyc-tlc-data`, parquet, csv e checkpoints locales

## Que demuestra este nivel

- configuracion de un entorno local reproducible para analisis y carga de datos
- validacion de conectividad y operacion sobre una base Postgres local
- separacion inicial entre capa `raw` y capa `curated`
- carga de un volumen relevante de datos a la zona de aterrizaje
- identificacion y documentacion de problemas operativos reales

## Estado del nivel-0

- Infraestructura local: operativa
- Conexion a Postgres desde notebook: validada
- Exploracion inicial con SQL: validada
- Ingestion a capa `raw`: lograda
- Definicion de capa `curated`: lograda
- Carga completa a `curated.trip`: pendiente por capacidad de disco
- Estabilizacion de pgAdmin: en seguimiento
- Higiene del repositorio local: en ajuste

## Riesgos y bloqueos actuales

Los principales puntos abiertos son:

- la maquina virtual trabaja con un disco de aproximadamente `10 GB` y llego a `100%` de uso
- PostgreSQL fallo al insertar sobre `curated.trip` por falta de espacio disponible
- `pgAdmin` ha presentado arranques lentos o estados inconsistentes despues de reinicios
- el volumen `nivel-0/nyc-tlc-data` queda con permisos restringidos por el usuario del contenedor
- el push al remoto no esta resuelto mientras la autenticacion siga apuntando a la cuenta corporativa

## Proximos pasos

- ampliar el disco virtual de la VM y luego extender la particion dentro de Ubuntu
- reintentar la carga de `curated.trip` una vez recuperado espacio
- validar conteos entre `raw.yellow_taxi_raw` y `curated.trip`
- documentar el flujo estable de arranque para Postgres y pgAdmin
- separar mejor notebooks de prueba rapida y notebooks de laboratorio
- cerrar el tema de permisos/ignores para que el repo solo rastree archivos utiles

## Bitacora

El detalle del avance, las decisiones y los bloqueos tecnicos vive en `BITACORA.md`. Esta trazabilidad forma parte del valor del repositorio: no solo muestra resultados, tambien el proceso de ingenieria detras de ellos.
