# ABD 21-22

## Tema 1: Nivel interno

Eficiencia en grandes cantidades de datos:

- Forma de almacenamiento de los datos 
- Forma de acceso rápido a los datos almacenados 
- Arquitecturas para relacionar los datos

Para que estos tres niveles sean satisfactorios tenemos que tener muy en cuenta como se relacionan estos niveles, como distribuimos los datos en el disco, qué tipo de almacenamiento usamos...

### 1. Medidas para evaluar un sistema de archivos 

Nivel físico: se almacena en un disco en forma de ficheros mediante el proceso de formateo (dar formato al disco)

Hay muchas opciones a la hora del formateo. Podríamos cambiar el tamaño de bloque según nuestras necesidades, por ejemplo. 

Por ello, previa la creación de la base de datos hay que hacer un estudio de los datos que vamos a utilizar, para tomar las mejores decisiones posibles. 

Para esto será necesario

- comparar sistemas de ficheros
- comparar modos de acceso a los ficheros

También tener en cuenta el hardware sobre el que almacenamos la info. Normalmente son discos duros, pero por ejemplo las cintas se pueden tener en cuenta. No se degradan con el tiempo si se almacenan correctamente, pero son de acceso secuencial... de hecho se siguen usando para, por ejemplo, carga de sistemas operativos, ya que la carga empieza en un punto de inicio y avanza secuencialmente.

#### Parámetros evaluables

Son medidas de espacio y tiempo

| Parámetro | Mide...                                                      | Comentarios                                                  |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| R         | memoria necesaria para almacenar un registro                 | Independiente del tipo de hardware                           |
| T         | tiempo para encontrar un registro arbitrario                 | El tiempo sí es dependiente del sistema de ficheros y del hardware |
| Tf        | tiempo para encontrar un registro por clave                  | - **Secuencial**: N+1 /2, O(n) (en el peor caso, el último elemento)<br />-**Árboles binarios**: O(n) (depende del orden en el que se hayan insertado los elementos, es una "lista mala")<br />-**Árbol AVL**: O(log2(n)). Son los árboles equilibrados los que se usan en SGDB |
| Tw        | tiempo para escribir un registro cuando ya se tiene su posición | Si lo escribimos sobre un fichero y queda un tamaño menor del original, guay pero y si es más? |
| Tn        | tiempo para encontrar el registro siguiente a uno dado       | -**Secuencial**: O(1)<br />-**Árbol AVL**: O(log2(n))<br />-**Hash**: dependiendo de la resolución de colisiones, pero vaya que se lía parda y en el peor de los casos hay que darle toda la vuelta |
| Ti        | tiempo para insertar un registro                             |                                                              |
| Tu        | tiempo para actualizar un registro                           | Hash: dependiendo de la resolución de colisiones, pero vaya que se lía parda y en el peor de los casos hay que darle toda la vuelta |
| Tx        | tiempo para leer un archivo                                  | **Secuencial**: O(n)                                         |
| Ty        | tiempo para reorganizar el archivo                           |                                                              |

Existen cuatro tipos de acceso secuencial:

1. físico
2. lógico
3. indexado
4. directo (hash)

#### Operaciones evaluables

Con los parámetros previos podemos contabilizar...

- recuperar un registro por valor de clave 
- obtener el siguiente registro 
- insertar registro (ampliar el fichero) 
- actualización de registro 
- leer todo el archivo 
- reorganizar el archivo

Todo lo que involucra cambiar datos toma mucho tiempo (insertar, modificar, eliminar, mover...), porque requiere varias de estas operaciones.

### 2. Registros y bloques 

Un SGBD almacena la información en tablas. 

- La estructura de una tabla la determinan las columnas 
- La información de una tabla se almacena en filas 

¿Y cómo se almacena todo a nivel físico?

Un SGBD provee de una serie de tipos de datos para las columnas de una tabla:0

- numéricos: enteros y reales
- cadenas de caracteres: CHAR, VARCHAR, VARCHAR2 
- fecha: DATE, TIME, TIMESTAMP 
- otros tipos: BLOBs, CLOBs...

#### Tamaño de un registro

| Tipo           | Tamaño           | Comentarios |
| -------------- | ---------------- | ----------- |
| CHAR(x)        | x Bytes          |             |
| VARCHAR2(x)    | de 1 a x+1 Bytes |             |
| FLOAT          | 6B               |             |
| BINARY_INTEGER | 2B               |             |

A tener en cuenta una cosa: un SGBD no almacena dependiendo de float, double, int... sino en función el número de cifras 

```
NUMBER 2 -> digitos de 2 cifras  
NUMBER 2.2 -> digitos de 2 cifras y 2 cifras decimales
```

- Campo: almacena un valor - en términos de tuplas, una celda
- Registro: conjunto de campos - en términos de tuplas, una tupla
- Bloque: conjunto de registros - no tiene equivalente en el sistema de tablas
- Fichero: conjunto de bloques

### 3. Organización de archivos y métodos de acceso 



### 4. Evaluación del sistema