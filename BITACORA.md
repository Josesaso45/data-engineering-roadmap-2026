# Bitacora del proyecto

Registro de trabajo orientado a trazabilidad tecnica. La idea es documentar avances, decisiones, bloqueos y aprendizajes de forma que cualquier persona que revise el repositorio pueda entender no solo el resultado, sino tambien el proceso.

## 2026-03-18

### Base del repositorio

- Se inicializo el repositorio con un `README.md` minimo.
- Se agrego `.gitignore` orientado a Python/Jupyter.
- Se excluyo el dataset `yellow_tripdata_2024-01.parquet` del control de versiones.

### Nivel 0

- Se creo `nivel-0/docker-compose.yml` con dos servicios:
  - `postgres:13`
  - `dpage/pgadmin4`
- Se definio la red `pg-network` con driver `bridge`.
- Se mapearon puertos locales:
  - `5432:5432` para Postgres
  - `5050:80` para pgAdmin
- Se monto el volumen `./nyc-tlc-data:/var/lib/postgresql/data` para persistencia local.

### Entorno de trabajo

- Se preparo `nivel-0/requirements.txt` con dependencias de:
  - JupyterLab e IPython
  - acceso SQL (`jupysql`, `SQLAlchemy`, `psycopg2-binary`)
  - analisis de datos (`pandas`, `numpy`, `pyarrow`)
- Existe un entorno virtual local en `nivel-0/venv`.

### Validaciones realizadas

- En `nivel-0/test.ipynb` se comprobo conectividad con Postgres usando:
  - `SELECT 1;`
- En el mismo notebook se consulto `information_schema.tables`.
- Esto confirma que el motor esta arriba y acepta conexiones locales desde `localhost:5432`.

### Evidencia de avance real

Avance actual estimado del nivel:

- Infraestructura local: lista
- Conexion a base de datos: validada
- Dataset disponible localmente: listo
- Ingestion del parquet a tablas propias: pendiente
- Modelado/transformaciones: pendiente
- Automatizacion y pruebas: pendiente
- Documentacion operativa: en progreso

### Siguientes pasos recomendados

1. Crear una tabla destino para el dataset de taxis.
2. Cargar el parquet en Postgres.
3. Validar conteos, esquema y tipos de datos.
4. Documentar el flujo de ejecucion de extremo a extremo.
5. Dividir exploracion y carga en notebooks o scripts con un objetivo claro.

## 2026-03-19

### Convergencia de trabajo y control de versiones

- Se reviso la divergencia entre trabajo realizado en Codespaces y trabajo local en Cursor.
- Se resolvio un conflicto de `nivel-0/requirements.txt` durante un rebase.
- Se conservo una version coherente del entorno para continuar el trabajo local.
- Se detecto un bloqueo posterior para `push` por autenticacion con una cuenta corporativa distinta del propietario del repositorio.

Valor mostrado:

- manejo de integracion entre entornos de trabajo
- resolucion de conflictos de versionado sin perder avance local
- cuidado por la coherencia del entorno antes de seguir construyendo

### Notebook de laboratorio

- Se agrego `nivel-0/LAB — Nivel-0.ipynb` como espacio de trabajo para la parte practica del nivel.
- Se cargo el parquet `yellow_tripdata_2024-01.parquet` con `pandas`.
- Se exporto el dataset a `yellow_tripdata_2024-01.csv` para facilitar la carga inicial hacia Postgres.
- Se valido conexion a la base `ny_taxi`.

### Modelo de datos inicial

- Se creo el esquema `raw`.
- Se definio la tabla `raw.yellow_taxi_raw`.
- Se ejecuto `\copy` para cargar datos desde el csv a la capa `raw`.
- Se verifico un `COPY 2964624`, lo que evidencia una carga grande y exitosa a la tabla de aterrizaje.
- Se creo el esquema `curated`.
- Se definio la tabla `curated.trip` con una estructura tipada y mas cercana al consumo analitico.

Valor mostrado:

- separacion inicial entre capa de aterrizaje y capa de consumo
- conversion de tipos desde una fuente cruda hacia un modelo mas controlado
- trabajo con una carga real de varios millones de registros

### Validaciones y pruebas

- Se hicieron pruebas de conectividad con `psql`.
- Se validaron tablas base e informacion del catalogo desde notebooks.
- Se exploro `pgAdmin` para revisar objetos, esquemas y tablas desde la interfaz grafica.

### Bloqueos operativos detectados

- `pgAdmin` mostro comportamiento intermitente: en ocasiones el contenedor estaba arriba pero la UI no respondia de inmediato.
- El volumen `nivel-0/nyc-tlc-data` quedo con permisos restringidos por el usuario del contenedor, generando warnings al recorrerlo desde Git.
- El archivo `.gitignore` requirio reglas mas especificas para `venv`, `nyc-tlc-data`, parquet, csv y checkpoints.

Aprendizaje:

- en proyectos de datos, la operacion del entorno local es parte del trabajo de ingenieria
- los problemas de permisos, herramientas y orquestacion impactan directamente la productividad

## 2026-03-20

### Falla por capacidad de disco

- Al intentar poblar `curated.trip` desde `raw.yellow_taxi_raw`, PostgreSQL devolvio un error de escritura por falta de espacio:
  - `could not extend file`
  - `Check free disk space`
- Se verifico que la VM tenia la particion raiz al `100%`.
- Se identifico que el problema principal no era memoria RAM, sino capacidad de disco virtual insuficiente para el volumen local de Postgres y los artefactos del proyecto.

Valor mostrado:

- diagnostico correcto del fallo a partir del mensaje del motor y del estado del sistema
- diferenciacion entre problema de memoria y problema de almacenamiento
- capacidad para detener la implementacion y priorizar estabilidad del entorno

### Estado de infraestructura

- Postgres siguio aceptando conexiones y levantando correctamente.
- `pgAdmin` pudo iniciar, reiniciarse y exponer el puerto `5050`, aunque con episodios de inicializacion lenta o respuestas inconsistentes hacia fuera.
- La red de Docker siguio asignando IPs internas correctas a `postgres-container` y `pgadmin-container`.

### Higiene del repositorio

- Se reforzo `.gitignore` para excluir:
  - `nivel-0/venv/`
  - `nivel-0/nyc-tlc-data/`
  - `nivel-0/yellow_tripdata_2024-01.parquet`
  - `nivel-0/yellow_tripdata_2024-01.csv`
  - `nivel-0/.ipynb_checkpoints`
- Se confirmo que el warning sobre `nyc-tlc-data` viene de permisos del sistema de archivos y no solo del ignore de Git.

Valor mostrado:

- preocupacion por la mantenibilidad del repositorio
- separacion entre artefactos de trabajo local y entregables relevantes

### Problemas pendientes

- La VM necesita ampliacion de disco y expansion posterior de particion/filesystem dentro de Ubuntu.
- El flujo de instalacion de paquetes con `apt` encontro problemas de conectividad con el mirror `co.archive.ubuntu.com`.
- El push al remoto sigue condicionado por la autenticacion de GitHub con la cuenta correcta.

### Estado actual del nivel

- Infraestructura local: funcional pero fragil por capacidad de disco
- Capa `raw`: creada y cargada
- Capa `curated`: definida
- Carga total a `curated.trip`: interrumpida por falta de espacio
- Documentacion del avance: en actualizacion

Lectura ejecutiva:

- ya existe evidencia de implementacion, no solo preparacion del entorno
- el principal bloqueo actual es de infraestructura local, no de entendimiento del flujo
- el siguiente hito tecnico claro es completar la carga a `curated.trip` y validar calidad

### Siguientes pasos recomendados

1. Expandir el disco virtual y luego crecer la particion del sistema dentro de Ubuntu.
2. Recuperar espacio util y validar `df -h` antes de seguir cargando datos.
3. Reintentar la insercion hacia `curated.trip`.
4. Verificar conteos y calidad entre `raw.yellow_taxi_raw` y `curated.trip`.
5. Estabilizar el flujo de uso de `pgAdmin` y dejarlo documentado.
6. Resolver autenticacion GitHub antes del siguiente push.