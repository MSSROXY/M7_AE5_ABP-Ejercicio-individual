# M7_AE5_ABP-Ejercicio-individual

## Índices en bases de datos y su utilidad en Django
Un índice en una base de datos permite acceder a los datos de manera rápida y eficiente, similar al índice de un libro. Su función principal consiste en localizar registros sin necesidad de recorrer toda la tabla, lo que reducir el tiempo de búsqueda y optimizar las consultas.

En Django, se puede definir índices en los modelos usando db_index=True en un campo o la clase Meta con la propiedad indexes. Esto permite que Django genere automáticamente las instrucciones SQL correspondientes al ejecutar las migraciones, facilitar consultas filtradas y mejorar el rendimiento en operaciones frecuentes como SELECT, JOIN y ORDER BY.

Aunque los índices acelerar la lectura de datos, también pueden aumentar el tiempo de escritura, ya que cada inserción, actualización o eliminación requiere actualizar los índices asociados. Por ello, es importante evaluar cuidadosamente qué campos requieren índices, especialmente en tablas grandes o aplicaciones con alto volumen de datos.

En conclusión, utilizar índices contribuye a desarrollar aplicaciones escalables y eficientes, optimizar el acceso a la información y mejorar el rendimiento general de las bases de datos en proyectos Django. Su correcta implementación es clave para garantizar eficiencia y velocidad en el manejo de grandes cantidades de datos.

## Manejo de omisión de campos en consultas en Django

Django ORM no permite excluir directamente un campo de un modelo al recuperar registros, pero ofrece mecanismos para seleccionar solo los campos que se necesitan. Esto se realiza utilizando los métodos values(), values_list() y consultas only() o defer().
| Método          | Qué hace                                   | Devuelve             | Uso típico / Ventaja                                       |
| --------------- | ------------------------------------------ | -------------------- | ---------------------------------------------------------- |
| `values()`      | Selecciona solo los campos indicados       | Diccionarios         | Recuperar solo los datos necesarios; reducir tráfico DB    |
| `values_list()` | Igual que `values()`, pero en tuplas       | Tuplas               | Cuando solo se necesitan valores sin nombres de campos     |
| `only()`        | Carga solo los campos indicados en memoria | Instancias de modelo | Optimizar memoria; otros campos se cargan bajo demanda     |
| `defer()`       | Omite ciertos campos al cargar registros   | Instancias de modelo | Evitar cargar campos grandes que no se usarán inicialmente |


## Diferencia con raw() con valores fijos

- Valores fijos: 
Producto.objects.raw('SELECT * FROM productos_producto WHERE precio < 100')
El valor está hardcodeado en la consulta.
Para cambiarlo, hay que editar la cadena SQL.

- Parámetros
Permiten pasar valores dinámicos sin modificar la consulta.
La base de datos recibe los parámetros de forma segura, evitando inyecciones SQL.

### Beneficios de usar parámetros:

- Seguridad: Protege contra ataques de inyección SQL, ya que Django maneja la sanitización automáticamente.

- Reutilización: Permite ejecutar la misma consulta con diferentes valores sin escribir múltiples consultas.

- Mantenimiento: Facilita cambiar los valores de búsqueda de manera dinámica, manteniendo el código más limpio.


## Ejecutando SQL Personalizado Directamente

¿Cuándo usar esta técnica?

- Operaciones complejas o específicas que no se pueden hacer fácilmente con el ORM, como joins avanzados, funciones de base de datos o queries optimizadas.
- Optimización de rendimiento, cuando una operación masiva es más eficiente en SQL puro que en múltiples llamadas ORM.
- Migraciones especiales o scripts administrativos donde se necesita control total sobre la ejecución SQL.

Precaución:

- Evitar usarlo para operaciones simples que el ORM puede manejar, para mantener la portabilidad y seguridad de la aplicación.
- Siempre usar parámetros para evitar inyecciones SQL.


## Ventajas y desventajas de usar conexión y cursor

- Ventajas: 

1. Control total sobre la consulta: Permite ejecutar cualquier SQL, incluso consultas complejas o específicas que el ORM no soporta fácilmente.
2. Optimización: Se puede aprovechar características específicas de la base de datos (funciones, joins complejos, índices).
3. Operaciones masivas: Ejecutar grandes insert, update o delete en bloque de manera eficiente.

- Desventajas:

1. Más propenso a errores: Hay que manejar manualmente los parámetros, cierres de cursor y transacciones.
2. Menos portable: SQL directo puede depender del motor de base de datos (PostgreSQL, MySQL, SQLite), mientras que el ORM es independiente.
3. No aprovecha funcionalidades de Django: Como annotate(), relaciones, validaciones, y señales del modelo.
4. Código menos legible y mantenible: Las consultas SQL embebidas pueden hacer el código más complejo que usando ORM.


## Procedimientos almacenados

Un procedimiento almacenado (stored procedure) es un conjunto de instrucciones SQL predefinidas y guardadas en la base de datos.

Permite ejecutar operaciones complejas (INSERT, UPDATE, DELETE, SELECT) de manera centralizada, sin necesidad de enviar todo el SQL desde la aplicación.

Sus ventajas incluyen:

- Reutilizar código SQL en múltiples aplicaciones.
- Reducir tráfico entre aplicación y base de datos, ya que la lógica se ejecuta en la base de datos.
- Mejorar rendimiento, especialmente en operaciones masivas o complejas.
- Aumentar seguridad, limitando permisos a la ejecución del procedimiento en lugar de dar acceso directo a tablas.

Django no tiene un ORM específico para procedimientos almacenados, pero se pueden invocar usando un cursor de base de datos.