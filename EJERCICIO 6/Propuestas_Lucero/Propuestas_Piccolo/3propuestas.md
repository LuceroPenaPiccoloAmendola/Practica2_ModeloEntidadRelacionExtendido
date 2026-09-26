<div align="center">

# Bases de datos

## Práctica 2 

# Ejercicio 6. Propuestas

<br>

**Nombre:** Fiorella Karina del Carmen Piccolo Amendola

**Profesor:** Gabriel Hurtado Áviles 

**Grupo**: 3CV2

**ESCOM**

**Fecha:** 28/09/2026

</div>

---

<div align="center">

# Propuestas

</div>

## Propuesta 1. Mapa con límites oficiales de las colonias

**1. Título y necesidad que atiende**

**Título:** Visualización territorial con polígonos oficiales. 

El artículo representa las ubicaciones mediante puntos o centroides y calcula la cercanía entre colonias con una distancia definida. Esto puede ser impreciso cuando se necesita conocer exactamente qué zona corresponde a cada colonia.

**2. Descripción de la funcionalidad**

Como persona usuaria, podría consultar un mapa donde cada colonia aparezca delimitada por su contorno oficial. Al seleccionar una colonia, se mostrarían sus datos de consumo y se podría comparar con las colonias que comparten un límite territorial.
Esto permitiría explorar la distribución del consumo de forma más precisa.

**3. Cambios en el modelo de datos**

Modificar dim_ubicacion: agregar los atributos geometria_poligono, area_km2 y version_limites, conservando la latitud y longitud.
Agregar dim_colonia: incluir id_colonia, nombre_colonia, clave_colonia y id_alcaldia, para identificar cada colonia.
Agregar rel_colonia_colindante: registrar las parejas de colonias que comparten un límite, con los atributos id_colonia_1 e id_colonia_2.
Relaciones: dim_ubicacion se relacionaría con dim_colonia; la tabla de colindancias conectaría una colonia con otra. La tabla de hechos de consumo seguiría relacionada con la ubicación correspondiente.

**4. En qué se apoya**

Se apoya en una limitación declarada por el artículo: el sistema utiliza centroides y una distancia de proximidad de 1.5 km, en lugar de polígonos oficiales y límites compartidos. Los autores proponen sustituir ese método por información territorial oficial.

**5. Dificultad estimada**

Sería **media** porque se necesita conseguir y procesar los polígonos oficiales, relacionarlos con las colonias del sistema y actualizar el mapa, pero no sería necesario modificar la forma principal en que se almacena el consumo.

---

## Propuesta 2. Divición del consumo por tipo de uso 

**1. Título y necesidad que atiende**

**Título:** Diferenciación del consumo

El artículo analiza el consumo general por colonia, pero no distingue claramente cómo se divide el gasto de agua entre viviendas, negocios o industrias dentro de una misma zona, lo cual limita entender qué sector tiene más demanda y por qué.

**2. Descripción de la funcionalidad**

Como persona usuaria, al seleccionar una colonia, podría ver una gráfica que muestre qué porcentaje del agua consumida corresponde a uso habitacional, comercial o industrial.

**3. Cambios en el modelo de datos**

Agregar dim_tipo_uso: Incluir atributos como id_tipo_uso, categoria (doméstico, comercial, industrial, mixto).
Modificar fact_consumo_agua: Agregar la clave foránea id_tipo_uso para asociar cada registro de consumo con su categoría correspondiente.

**4. En qué se apoya**

Se apoya en la frecuencia con la que se manejan a veces los volúmenes globales por colonia en los sistemas urbanos, donde no siempre se separa el tipo de gasto con el uso que se le da.

**5. Dificultad estimada**

Sería baja ya que solo requiere agregar una tabla de catálogo pequeña (dim_tipo_uso) y relacionarla en la tabla de hechos, sin necesidad de integrar fuentes externas complejas.

---

## Propuesta 3. Cruce de consumo con reportes ciudadanos de fugas

**1. Título y necesidad que atiende**

**Título:** Consulta de reportes de fugas junto con el consumo.

El sistema identifica consumos que se alejan de los valores habituales, pero el artículo aclara que esto no permite confirmar que exista una fuga. La funcionalidad buscaría complementar los datos de consumo con reportes ciudadanos para dar más contexto.

**2. Descripción de la funcionalidad**

Como persona usuaria, podría seleccionar una colonia y un bimestre para consultar su consumo de agua junto con los reportes de fugas, falta de agua u otros problemas registrados en esa zona.
Si el sistema detecta un consumo inusual y también existen reportes cercanos, mostraría ambos datos para que la persona pueda revisarlos. El sistema no afirmaría que la fuga causó el consumo anormal, sino que presentaría información que necesita verificarse.

**3. Cambios en el modelo de datos**

Agregar fact_reporte_agua: una fila por reporte ciudadano, con atributos como id_reporte, fecha_reporte, tipo_reporte y estado_reporte.
Agregar dim_tipo_reporte: clasificar los reportes, por ejemplo, fuga, falta de agua, drenaje o mala calidad.
Modificar dim_ubicacion: agregar una clave territorial que permita relacionar el reporte con la colonia correspondiente.
fact_reporte_agua se relacionaría con dim_tiempo, dim_ubicacion y dim_tipo_reporte.

**4. En qué se apoya**

Se apoya en la limitación del artículo de que las anomalías se evaluaron con datos sintéticos y no con reportes reales de fugas. También considera que SACMEX publica información de reportes ciudadanos sobre servicios de agua.

**5. Dificultad estimada**

Sería alta ya que se requiere integrar una segunda fuente de datos, relacionar correctamente los reportes con las ubicaciones y evitar interpretar una coincidencia como una causa comprobada.

