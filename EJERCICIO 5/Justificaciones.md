
# Ejercicio 5: Modelo Entidad-Relación Extendido (EER)

## 3. Identificación y Justificación de la Jerarquía de Especialización

En el dominio conceptual del proyecto se identificó una estructura de Especialización Total con Exclusividad centrada en la entidad general **`MEDICION`**.

  

### Justificación Teórica y Estructural

-   **Superclase (`MEDICION`):** Representa la abstracción de cualquier evento de captura de datos ambientales en la Ciudad de México. Esta superclase centraliza la localización espacial (`UBICACION`) y la referencia temporal (`TIEMPO`) en las que ocurre el registro.
    
      
    
-   **Subclases Disjuntas (`CONSUMO_AGUA` y `REGISTRO_CLIMA`):** Heredan el contexto de espacio y tiempo de `MEDICION` y agregan los atributos específicos del fenómeno físico capturado:
    
      
    -   **`CONSUMO_AGUA`:** Agrega métricas volumétricas (`consumo_m3`, `consumo_promedio`) y se relaciona directamente con el catálogo socioeconómico `INDICE_DESARROLLO`.
        
          
        
    -   **`REGISTRO_CLIMA`:** Agrega variables meteorológicas específicas (`temp_media`, `precipitacion`).
        
          
        

### Restricciones de la Especialización

1.  **Totalidad (Círculo en la arista de la superclase):** Toda medición registrada en el sistema pertenece obligatoriamente a una de las dos subclases definidas; no existen registros de mediciones en el Data Warehouse que no pertenezcan a agua o clima.
    
      
    
2.  **Exclusividad / Disyunción (Arco cruzando las subclases):** Un evento de medición es un consumo de agua **o** un registro de clima, pero **nunca ambos simultáneamente**.
    
      
    

## 4. Tabla de Correspondencia entre el Modelo Conceptual (EER) y el Esquema Relacional (Data Warehouse)

A continuación se presenta el mapeo entre las entidades del modelo conceptual EER y las tablas físicas del almacén de datos dimensional, detallando la transformación de la información (pérdidas y adiciones) durante la transición de un modelo a otro:

  

**Entidad Conceptual (EER)**

**Tabla(s) en el Data Warehouse**

**Información Agregada / Perdida al pasar al Data Warehouse**

**`MEDICION`** _(Superclase)_

_Ninguna (Implementada mediante las tablas de hechos)_

  

  

**Agregada:** En el esquema lógico/dimensional no existe la tabla abstracta `MEDICION`. Su estructura se materializa dividida físicamente en las tablas de hechos `FACT_CONSUMO_AGUA` y `FACT_CLIMA`.

  

**`CONSUMO_AGUA`** _(Subclase)_

`FACT_CONSUMO_AGUA`

**Perdida:** Se pierde la frecuencia o granularidad transaccional original al agregarse los registros por periodo bimensual.

  

  

**Agregada:** Claves subrogadas (`id_consumo`) y claves foráneas (`id_ubicacion`, `id_tiempo`, `id_indice_des`) para soporte del modelado OLAP. | | **`REGISTRO_CLIMA`** _(Subclase)_ | `FACT_CLIMA` | **Perdida:** Se pierden las mediciones meteorológicas continuas u horarias al resumirse en promedios diarios.

  
  

**Agregada:** Clave subrogada `id_clima` y claves foráneas de dimensión (`id_ubicacion`, `id_tiempo`). | | **`UBICACION`** | `DIM_UBICACION` | **Agregada:** Desnormalización de la jerarquía geográfica (Alcaldía, Colonia y Geo Point se consolidan en una sola dimensión para acelerar el tiempo de respuesta en consultas). | | **`TIEMPO`** | `DIM_TIEMPO` | **Agregada:** Atributos calculados y jerarquías temporales explícitas (Bimestre, Mes, Año) derivados a partir de la fecha base. | | **`INDICE_DESARROLLO`** | `DIM_INDICE_DES` | **Perdida:** Se pierde el valor numérico continuo del margen de vulnerabilidad al discretizarse en categorías cualitativas (_Popular_, _Bajo_, _Medio_, _Alto_). |

![Tabla de correspondencia](https://postimg.cc/VSTLB70V)

## Link al Modelo Entidad-Relación Extendido (EER)
https://drive.google.com/file/d/1YlrWsSGhdqBTuRxSUecZjyGlfEpgFbqx/view?usp=sharing

> Written with [StackEdit](https://stackedit.io/).
