# Bitacora del proyecto

Registro breve de cambios, trabajo realizado y nivel de avance del roadmap.

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
