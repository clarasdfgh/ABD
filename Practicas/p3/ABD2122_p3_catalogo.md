# ABD 21/22 Práctica 2 - Catálogos de implementaciones SGBDR

###### Clara María Romero Lara

## Introducción

El SGDB escogido para el desarrollo de la práctica es PostgreSQL. PostgreSQL es un sistema de gestión de bases de datos relacional, orientado a objetos y de código abierto.

He escogido este sistema porque, al ser un SGBD SQL relacional, su sistema de catálogo será relativamente similar al estudiado en Oracle. Además, su documentación es extensa y su comunidad, activa.

## Catálogo

El catálogo del sistema es el lugar donde el SGBD almacena tanto los metadatos de las bases de datos como la información contenida en estas. El catálogo de postgreSQL consiste en una serie de tablas con las que se puede operar de igual forma que con una tabla normal (añadir elementos, *dropear* la tabla...). Pero, por razones obvias, no se recomienda manipular manualmente las tablas del catálogo. 

Las tablas del catálogo generalmente se manipulan de forma indirecta a través de los comandos SQL. Por ejemplo, al mandar la consulta `CREATE DATABASE`, estamos insertando una nueva fila en el catálogo `pg_database`. Existen instancias en las que la manipulación del catálogo tiene que realizarse de forma directa sobre las tablas, pero cada vez se incluyen más operaciones en SQL que evitan esto.















### Jerarquía de contenedores

En postgreSQL la jerarquía de contenedores es la siguiente:

1. Un ordenador tiene uno o múltiples *clústers*
2. El servidor de una base de datos es un *clúster*
3. Un *clúster* tiene catálogos, uno para cada base de datos
4. Los catálogos tienen *schemas*
   1. En postgreSQL un *schema* es un *namespace* que contiene objetos como tablas, vistas, tipos de datos, funciones, operadores... Es decir, nos permite agrupar objetos de la base de datos en grupos lógicos.
5. Los *schemas* contienen tablas
6. Las tablas contienen filas
7. Las filas contienen columnas

De esta jerarquía nos interesa todo lo inferior al catálogo, puesto que es información que se almacenará en este. Las tablas catálogo que almacenan estos contenedores son:

-  `pg_namespace` almacena *namespaces*, que son la estructura que soporta los *schemas*. Cada namespace puede tener una colección separada de relaciones, tipos de dato... sin conflictos de nombre. 
- `pg_class` almacena tablas y básicamente cualquier cosa que tenga columnas o sea similar a una tabla. Esto incluye *indexes*, vistas, *composite types*...
- `pg_attribute` almacena información sobre las columnas de las tablas. Existirá una fila en  `pg_attribute` por cada columna en cada tabla de la base de datos (así como para los objetos que entran dentro de la categoría `pg_class`).























### Listado de tablas del catálogo

A continuación listamos algunas de las tablas contenidas en el catálogo de postgreSQL. Cada base de datos contiene estas tablas, salvo por `pg_database` que contiene la lista de bases de datos y es, por tanto, única.

| Nombre del catálogo | Descripción                                          |
| ------------------- | ---------------------------------------------------- |
| pg_database         | base de datos                                        |
| pg_class            | clases (tablas, índices, vistas...)                  |
| pg_attribute        | atributos de clases                                  |
| pg_index            | índices secundarios                                  |
| pg_proc             | procedimientos (C y SQL)                             |
| pg_type             | tipos (del sistema y definidos por el  usuario)      |
| pg_operator         | operadores (del sistema y definidos por el  usuario) |
| pg_aggregate        | agregados y funciones agregadas                      |
| pg_am               | método de acceso                                     |
| pg_amop             | operador de método de acceso                         |
| pg_amproc           | funciones de soporte para el método de acceso        |
| pg_opclass          | clases de operadores de método de acceso             |

Existen muchas más tablas dentro del catálogo, que podemos consultar en la [documentación](https://postgrespro.com/docs/postgresql/10/catalogs-overview).















### Consultar vistas del catálogo

PostgreSQL cuenta, además, con comandos para consultar vistas específicas del catálogo. Esto nos permite ver la información de una forma menos segmentada y más útil para el usuario. Algunos comandos son:

| **Comando**       | **Acción**                                     |
| ----------------- | ---------------------------------------------- |
| \db               | Listar todos los *tablespaces*                 |
| \dn               | Listar todos los *schemas*                     |
| \d_\dt            | Listar todas las tablas                        |
| \d table-name     | Mostrar definición de una tabla                |
| \dp               | Listar todos los privilegios                   |
| \dl               | Listar todos los objetos grandes               |
| \da               | Listar todos los agregados                     |
| \df               | Listar todas las funciones                     |
| \df function-name | Listar todas las funciones con el nombre dado  |
| \do               | Listar todos los operadores                    |
| \do operator-name | Listar todos los operadores con el nombre dado |
| \dT               | Listar todos los tipos                         |
| \du               | Listar todos los usuarios                      |
| \l                | Listar todas las bases de datos en el clúster  |















## Bibliografía

- *PostgreSQL : Documentation: 10: 51.1. overview*. Postgres Pro. (n.d.). Retrieved May 13, 2022, from https://postgrespro.com/docs/postgresql/10/catalogs-overview 
- *PostgreSQL schema*. PostgreSQL Tutorial. (n.d.). Retrieved May 13, 2022, from https://www.postgresqltutorial.com/postgresql-administration/postgresql-schema/ 
- J, N. (2017, April 21). *PostgreSQL system catalog views*. PostgreSQL System Catalog Views. Retrieved May 13, 2022, from https://www.tutorialdba.com/p/postgresql-system-catalog-views.html 
- Tablas Internas. (n.d.). Retrieved May 13, 2022, from https://www.ibiblio.org/pub/linux/docs/LuCaS/Tutoriales/NOTAS-CURSO-BBDD/notas-curso-BD/node170.html 
