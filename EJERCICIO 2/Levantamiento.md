# Levantamiento

### Reporte de Puesta en Funcionamiento: Data Warehouse CDMX
 **Asignatura:** Bases de Datos 
 **Ejercicio:** Ejercicio 2. Clonar y poner en funcionamiento el proyecto asignado 
 **Repositorio Fork:** [Data_Warehouse_static](https://github.com/LuceroPenaPiccoloAmendola/Data_Warehouse_static) 
 **Identificador de la confirmación (Commit):** `b9366f0`
## 1. Requisitos del Sistema y Herramientas

Para llevar a cabo la reproducción del entorno localmente, se requirieron e instalaron las siguientes herramientas:

-   **Git / Git Bash:** Para el control de versiones y ejecución de comandos en terminal.
    
-   **Docker Desktop (Windows 11):** Motor para la creación y gestión del contenedor del Data Warehouse.
    
-   **PostgreSQL (Docker Image):** Servicio de base de datos relacional expuesto en el puerto `5433`.
    
-   **pgAdmin 4:** Cliente gráfico para la gestión y consulta de la base de datos PostgreSQL.
    

## 2. Pasos de Puesta en Funcionamiento

Los pasos se ejecutaron en el siguiente orden estricto dentro de la terminal Git Bash:

### Paso 1: Clonar el repositorio fork

Se clonó el repositorio forkeado en una carpeta independiente de la computadora:

Bash

```
git clone [https://github.com/LuceroPenaPiccoloAmendola/Data_Warehouse_static.git](https://github.com/LuceroPenaPiccoloAmendola/Data_Warehouse_static.git)
cd Data_Warehouse_static

```

### Paso 2: Construir y levantar el contenedor en Docker

Se navegó al directorio `warehouse/` donde reside la configuración de Docker Compose y los scripts de ETL, ejecutando la construcción en segundo plano:

Bash

```
cd warehouse
docker compose up -d --build

```

### Paso 3: Verificación del servicio

Se verificó que el contenedor se encontrara en estado activo (_Up_) y con el puerto `5433` expuesto:

Bash

```
docker ps

```

## 3. Evidencias de Ejecución

### Evidencia 1: Arranque del contenedor en la terminal

Captura del proceso de descarga de componentes, construcción del contenedor e inicio del servicio en Docker:

### Evidencia 2: Estado del contenedor activo

Verificación mediante el comando `docker ps` confirmando que el contenedor `data_warehouse_cdmx` está activo y escuchando en el puerto `5433`:

### Evidencia 3: Conexión y consulta a la Base de Datos

Se estableció la conexión a la base de datos `data_warehouse` mediante **pgAdmin 4** utilizando los siguientes parámetros:

-   **Host:** `localhost`
    
-   **Puerto:** `5433`
    
-   **Base de datos:** `data_warehouse`
    
-   **Usuario:** `postgres`
    

Se ejecutó la consulta SQL para verificar la carga correcta de datos en la tabla de hechos:

SQL

```
SELECT * FROM fact_consumo_agua LIMIT 10;

```

## 4. Registro de Errores y Soluciones

### Problema 1: Nombre de contenedor incorrecto al intentar conectar por CLI

-   **Descripción:** Al intentar acceder a la consola interactiva con `docker exec -it warehouse-db-1 psql ...`, el demonio de Docker devolvió el error `Error response from daemon: No such container: warehouse-db-1`.
    
-   **Causa:** El archivo de configuración de Docker definió el nombre del contenedor como `data_warehouse_cdmx` y no `warehouse-db-1`.
    
-   **Solución:** Se identificó el nombre real ejecutando `docker ps` y se corrigió el comando especificando `data_warehouse_cdmx`.
    

### Problema 2: Bloqueo/Congelamiento de la terminal en Git Bash

-   **Descripción:** Al ejecutar `docker exec -it data_warehouse_cdmx psql -U postgres -d data_warehouse` en Git Bash para abrir la consola interactiva de PostgreSQL, la terminal se congeló e interactuar mediante `Ctrl + C` no respondía.
    
-   **Causa:** Git Bash en Windows presenta incompatibilidades conocidas al gestionar controladores de pseudo-terminal (TTY) en modo interactivo sin el prefijo `winpty`.
    
-   **Solución:** Se optó por conectar la base de datos al cliente gráfico **pgAdmin 4** apuntando al puerto expuesto `5433`. Esto no solo evitó el problema de la terminal, sino que permitió visualizar los datos de la consulta en un formato de tabla estructurado.



