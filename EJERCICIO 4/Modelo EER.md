<div align="center">

# Bases de datos

## Modelo entidad relación extendido

</div>

<img width="932" height="497" alt="EER" src="https://github.com/user-attachments/assets/acb81774-de75-4dc8-a9bc-e720ccf863de" />

---

# Justificación

La identidad inventario no puede existir de forma independiente porque representa la cantidad física almacenada y el estado de stock de un artículo específico. Sin una entidad fuerte Productos a la cual asociar existencias, los registros de cantidad perderían su contexto.
La identidad categoría  no puede existir en el sistema si no hay artículos que clasificar ni parámetros organizativos vigentes.


Se seleccionó una jerarquía de Especialización Total y Disyunta (d, t) con Persona como superclase. En este caso la disyunta garantiza la exclusividad de roles en la organización, una persona no puede ser clasificada simultaneamente como proveedor y cliente. La total, exige que todo miembro de la entidad padre obligatoriamente pertenezca a uno de los subtipos definidos.


Las parejas de cardinalidad traducen la lógica real de la tienda:

Proveedores — Productos (0, n) : (1, n): Todo producto registrado debe tener al menos un proveedor asociado (1, n) para poder ser surtido, mientras que un proveedor puede registrarse en la plataforma antes de surtir su primer catálogo (0, n). 
Productos — Categoría (1, 1) : (1, n): Todo producto debe pertenecer de forma obligatoria a una sola categoría (1, 1), mientras que una categoría agrupa a uno o varios productos (1, n). 
Empleados — Ventas (0, n) : (1, 1): Cada ticket de venta debe ser procesado de manera única por un empleado (1, 1), mientras que un empleado puede acumular múltiples ventas en su turno (0, n).


Consultas que se pueden hacer: 

1. ¿Cuáles son las ventas realizadas por personas registradas específicamente con el rol de Empleado (con su atributo Puesto), distinguiéndolas de las compras registradas por personas registradas con el rol de Cliente (con su atributo Correo)?

2. ¿Qué productos registrados en la base de datos no tienen aún un registro asociado en la entidad débil Inventario, o cuál es la FechaActu del stock de un producto específico?

3. ¿Cuáles son los productos que pertenecen obligatoriamente a una sola Categoría (1, 1) y que son surtidos por más de un proveedor (1, N) a través de la relación Surtir?
