<div align="center">

# Bases de datos 

## Práctica 2

# Ejercicio 3. Artículos 

<br>

**Nombre:** Daniel Eduardo Lucero Peña y Fiorella Karina del Carmen Piccolo Amendola

**Profesor:** Gabriel Hurtado Áviles 

**Grupo:** 3CV2

**ESCOM**

**Fecha:** 28/09/2026

</div>

---

# Índice 

1. **Introducción**

2. **Desarrollo**
    -2.1. Artículo de sismo
    -2.2. Artículo de obras públicas
    -2.3. Artículo de consumo de agua
    
3. Conclusión

---

# Introducción 
En este trabajo se analizan tres artículos que se enfocan en el diseño, procesamiento y modelado de datos a través de almacenes de datos (Data Warehouses) aplicados a la resolución de problemáticas urbanas y sociales en México.
A continuación, se presentan los resúmenes correspondientes al estudio de la actividad sísmica nacional, la gestión y transparencia en las obras públicas municipales, y el análisis del consumo de agua en la Ciudad de México, destacando en cada caso sus fuentes de información, estructuras de almacenamiento, principales aportaciones, limitaciones y trabajos a futuro.

---

# Desarrollo

## 2.1. Sismos

El primer artículo aborda el problema de que México cuenta con una gran cantidad de datos técnicos sobre sismos, pero resultan difíciles de entender para personas sin conocimientos especializados. Para solucionarlo, se desarrolló un sistema interactivo de visualización. Los datos principales provienen del Servicio Sismológico Nacional, con registros desde 1900, y del INEGI, a través del Censo de Población y Vivienda 2020 y Censos Económicos en formato CSV. Para utilizarlos, se creó un programa en Python encargado de validar y limpiar la información, eliminando errores y generando un archivo SQL.
Posteriormente, estos datos limpios se cargaron en PostgreSQL, organizándolos en una estructura de almacén de datos (data warehouse) que integra información sobre sismos, población y economía. Gracias a esto, el sistema permite responder preguntas concretas sobre dónde ocurren los sismos, su magnitud, profundidad y distribución geográfica, vinculando la actividad sísmica con las características demográficas de las zonas afectadas a través de una interfaz web con mapas y gráficos.
Finalmente, los autores reconocen que el sistema no tiene la capacidad de predecir sismos. Como trabajo futuro, proponen incorporar visualizaciones en 3D, integrar nuevas fuentes de datos oficiales y conectarlo con plataformas de alerta temprana para hacer la información todavía más útil.

## 2.2. Obras públicas

El segundo artículo aborda el problema de la gestión de obras públicas municipales, donde la información sobre presupuestos, contratistas, avances y reportes está dispersa en diferentes sistemas, lo que dificulta detectar retrasos y afecta la transparencia. Los datos utilizados fueron sintéticos (creados artificialmente para pruebas) correspondientes a 1,247 obras en 55 comunidades. Los datos estructurados se obtuvieron en bases relacionales y los archivos multimedia (fotografías y PDFs) en Cloudflare R2. Para utilizarlos, se validaron y organizaron mediante un Data Warehouse con un modelo dimensional en esquema de estrella (diez dimensiones y dos tablas de hechos que registran eventos de auditoría y el estado mensual de las obras, utilizando además dimensiones de cambio lento para conservar el historial).
Gracias a esta estructura, el sistema puede responder preguntas concretas sobre qué obras tienen retrasos y cuántos días llevan, presupuestos asignados y ejecutados, historial de empresas contratistas, regiones con mayor participación ciudadana y qué obras presentan características de anomalías. Los autores reconocen como limitaciones que el sistema fue probado únicamente con datos sintéticos, depende de que los supervisores carguen información correcta, y sus controles de acceso son básicos. Como trabajo futuro, proponen evolucionar hacia una arquitectura lakehouse, integrar inteligencia artificial para analizar fotografías automáticamente, usar modelos de series de tiempo para predecir fechas de término y probarlo con datos reales de municipios.

## 2.3. Consumo de agua

El tercer artículo aborda el problema de la dificultad para consultar y analizar la información sobre el consumo de agua en la Ciudad de México, ya que los datos públicos de SACMEX están dispersos y son difíciles de utilizar, lo que limita la toma de decisiones ante problemas de sobreexplotación y fugas. Los datos principales provienen de fuentes de SACMEX (71,102 registros iniciales de los tres primeros bimestres de 2019), además de datos climáticos de Open-Meteo y geográficos de OpenStreetMap. Los archivos estaban principalmente en formato CSV, por lo que se aplicó un proceso ETL para limpiarlos y validarlos (conservando 70,886 registros) y se almacenaron en PostgreSQL, generando también archivos JSON y GeoJSON.
La información se modeló mediante un esquema de estrella con dos tablas de hechos (fact_consumo_agua y fact_clima) y tres dimensiones compartidas (tiempo, ubicación e índice de desarrollo), transformándose también a un grafo de conocimiento mediante RDF Data Cube y R2RML para analizar relaciones espaciales. Gracias a esto, el sistema responde preguntas sobre el consumo de agua por alcaldía o colonia, comparativa entre bimestres, niveles de desarrollo y detección de consumos atípicos mediante un mapa coroplético y filtros interactivos.
Los autores reconocen como limitaciones que los datos solo abarcan los primeros tres bimestres de 2019, que la detección de anomalías se probó con datos artificiales y no detecta fugas reales, y que las colonias se representan por centroides (con un margen de distancia de hasta 1.5 km en lugar de polígonos exactos). Como trabajo futuro, proponen crear un repositorio permanente con un punto de consulta SPARQL público, usar polígonos oficiales, reevaluar el módulo de anomalías con datos reales de fugas y aplicar la metodología a otros problemas urbanos como movilidad y calidad del aire.

---

# Conclusión 

En conclusión, se puede observar que los tres proyectos tienen una base similar, todos usan almacenes de datos (Data Warehouses) y esquemas de estrella en PostgreSQL para ordenar información que antes no lo estaba. 
Sim embargo cada uno tiene su particularidad. El del sismos se concentra en cruzar datos históricos con información de población. El de obras públicas está más armado para revisar proyectos usando dimensiones que guardan el historial de los cambios. Y el del agua, además del almacén clásico, arma un grafo de conocimiento para conectar las zonas de la ciudad y analizar distancias y consumos.
Como puntos en común, los tres tienen limitaciones parecidas, dependieron de datos de prueba, de que la gente capture bien la info o de que todavía no predicen cosas en tiempo real. Por eso, todos proponen para el futuro mejorar sus herramientas y probarlos con datos reales del día a día.
