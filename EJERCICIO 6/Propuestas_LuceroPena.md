  # Bases de datos

## Práctica 2

## Ejercicio 6. Propuestas de Mejora al Data Warehouse

  

**Nombre:** Lucero Peña Daniel Eduardo
**Profesor:** Gabriel Hurtado Avilés
**Grupo:** 3CV2
**ESCOM - IPN**
**Fecha:** 28/09/2026

  

# Propuestas

## Propuesta 1. Impacto económico y cálculo del costo del agua por esquema tarifario



### 1. Modelado del costo financiero, subsidios y cobranza por bloque tarifario.
Actualmente, el sistema analiza únicamente el volumen físico del agua consumida en metros cúbicos ($m^3$). Sin embargo, no contempla la conversión financiera del consumo, la facturación estimada ni la estructura de tarifas escalonadas y subsidios aplicados por el gobierno de la CDMX (SACMEX) según el estrato socioeconómico de cada colonia. Esto impide evaluar la recaudación real y el impacto del costo del agua en la población.

### 2. Descripción de la funcionalidad
Como persona usuaria, al consultar una colonia y un bimestre determinado, podría visualizar el volumen consumido expresado en **monto monetario estimado ($ MXN)**, identificando qué porcentaje del costo real fue cubierto por el usuario y qué cantidad correspondió a subsidio estatal según su `INDICE_DESARROLLO`.  

### 3. Cambios en el modelo de datos
-   **Agregar `dim_tarifa_agua`:** Incluir atributos como `id_tarifa`, `rango_m3_min`, `rango_m3_max`, `costo_m3_base`, `descuento_subsidio` y `anio_vigencia`.
-   **Modificar `fact_consumo_agua`:** Agregar los atributos derivados o medidas numéricas `monto_facturado_estimado`, `monto_subsidio_pesos` y la clave foránea `id_tarifa`.
-   **Relaciones:** `dim_tarifa_agua` se relacionaría con una cardinalidad $(1,N)$ hacia `fact_consumo_agua`.
    
### 4. En qué se apoya
Se apoya en una limitación del artículo, el cual asocia el `INDICE_DESARROLLO` exclusivamente como una etiqueta social cualitativa (_Popular_, _Bajo_, _Medio_, _Alto_) para correlacionar el consumo volumétrico, obviando que dicho índice determina directamente el esquema tarifario y los bloques de cobro aplicables por ley en la Ciudad de México.

### 5. Dificultad estimada
Sería **baja**, ya que la estructura tarifaria se modela como una dimensión de catálogo fija (`dim_tarifa_agua`) y los cálculos financieros se integran directamente como medidas numéricas dentro de la tabla de hechos existente.

## Propuesta 2. Caracterización de eventos climáticos extremos e índice de estrés hídrico


### 1. Evaluación de variaciones del consumo ante eventos meteorológicos atípicos (Olas de calor y Sequías).
La tabla `fact_clima` únicamente almacena mediciones meteorológicas continuas o promedios (temperatura media y precipitación). No obstante, el sistema no categoriza la ocurrencia de **eventos climáticos extremos** o anomalías (olas de calor, sequías prolongadas o lluvias atípicas), lo que dificulta correlacionar de manera directa las olas de calor con los picos extraordinarios en la demanda de agua.

### 2. Descripción de la funcionalidad
Como persona usuaria, podría filtrar un periodo de tiempo e identificar automáticamente las fechas o bimestres donde se declararon olas de calor o sequías en la CDMX. Al seleccionar dicho evento, el sistema permitiría comparar el incremento porcentual del consumo de agua durante el evento atípico frente a la media histórica de la misma colonia.

### 3. Cambios en el modelo de datos
-   **Agregar `dim_evento_climatico`:** Incluir atributos como `id_evento`, `nombre_evento` (_Ola de calor_, _Sequía_, _Lluvia atípica_), `nivel_severidad` (_Leve_, _Moderado_, _Severo_) y `descripcion`.
-   **Modificar o conectar con `fact_clima`:** Agregar la clave foránea `id_evento` en `fact_clima` (o crear una tabla puente `rel_clima_evento` si un registro abarca múltiples anomalías).
     **Relaciones:** `dim_evento_climatico` se relaciona $(0,N)$ con `fact_clima` para catalogar las lecturas meteorológicas. 

### 4. En qué se apoya
Se apoya en la observación del artículo donde se analiza la correlación entre clima y agua de forma lineal, reconociendo que los promedios simples no capturan el comportamiento atípico de los usuarios cuando la temperatura supera los umbrales normales durante varios días consecutivos.

### 5. Dificultad estimada
Sería **media**, ya que requiere definir umbrales históricos para clasificar una lectura climática como un "evento extremo" y procesar las relaciones temporales entre las dimensiones climáticas y de consumo.


## Propuesta 3. Trazabilidad del origen de abastecimiento e infraestructura de red

### 1. Monitoreo del origen del suministro y capacidad de la red (Pozos vs. Sistema Cutzamala).
El Data Warehouse actual analiza el consumo exclusivamente desde la perspectiva del punto de destino (la colonia o alcaldía), tratando el suministro de agua como una fuente infinita. No existe visibilidad sobre la fuente de abastecimiento (si la zona se alimenta de pozos locales, del Sistema Cutzamala o de tanques de almacenamiento), lo que impide evaluar qué zonas sufren mayor vulnerabilidad ante cortes en la red primaria.

### 2. Descripción de la funcionalidad
Como persona usuaria, al seleccionar una colonia o alcaldía, podría visualizar un indicador que desglose el origen del agua que recibe (ejemplo: $70\%$ Sistema Cutzamala, $30\%$ Pozos locales) y consultar la capacidad/presión proyectada de la infraestructura que abastece a esa zona.

### 3. Cambios en el modelo de datos
-   **Agregar `dim_fuente_suministro`:** Incluir atributos como `id_fuente`, `nombre_fuente` (_Sistema Cutzamala_, _Pozo Profundo Local_, _Sistema Lerma_, _Tanque de Almacenamiento_), `tipo_fuente` (_Superficial_, _Subterránea_) y `capacidad_maxima_lps`.
-   **Agregar tabla intermedia / relación `rel_ubicacion_fuente`:** Incluir `id_ubicacion`, `id_fuente` y `porcentaje_aportacion` (para modelar que una ubicación puede recibir agua de más de una fuente).
-   **Relaciones:** La ubicación se vincula con las fuentes de suministro para establecer el mapa de dependencia hídrica.

### 4. En qué se apoya
Se apoya en una limitación explícita del artículo de investigación, el cual señala que el modelo se limita a la demanda (consumo registrado) sin considerar las restricciones de la oferta física ni la procedencia de la infraestructura hídrica de la Ciudad de México.
  
### 5. Dificultad estimada
Sería **alta**, debido a la necesidad de modelar relaciones de red de infraestructura (_muchos a muchos_ entre fuentes de agua y ubicaciones) e integrar información sobre los porcentajes de aportación hidráulica por cada alcaldía.

