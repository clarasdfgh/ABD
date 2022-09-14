## Tema 6: Sistemas distribuidos de bases de datos

### Introducción

Tradicionalmente, un procesador y varios dispositivos de almacenamiento. La distribución del proceso se debe a: 

- funcionalidad 
- geografía 
- autonomía 
- fiabilidad 
- escalabilidad

Un SGBDD se compone de nodos (localidades): 

- el nodo es un SGBD 
- los nodos pueden trabajar aisladamente o intercambiar datos 

Base de datos distribuida = virtual = lógica 

Tipos: homogéneo, heterogéneo



> ###### ÍNDICE
>
> 1. Características
> 2. Transparencia
> 3. Diseño
> 4. Recuperación
> 5. Control de concurrencia



### 1. Características

Para un usuario debe ser igual trabajar contra un sistema distribuido que contra un sistema aislado.

**Conceptos**: 

- Sección posterior: SGBD
- Sección frontal: aplicaciones (en el SGBD o fuera)
- Administrador de Comunicaciones de Datos (DC)

**Procesamiento distribuido. Caso: cliente/servidor:**

- aplicaciones en un servidor, SGBD en otro (mejores tiempos) 
- servidor exclusivo para SGBD (mejor rendimiento) 
- cliente exclusivo para aplicaciones (mejor adaptado) 
- varios clientes acceden al mismo servidor
- varios servidores tras un mismo cliente 
- usuarios de un nodo pueden acceder a datos de otro nodo

**Ventajas:** 

- Uso compartido de datos (departamentos, divisiones, …) 
- Fiabilidad y disponibilidad: aislamiento ante fallos, redundancia (requiere detección de fallos y reintegración) 
- Agilización de consulta: dividir consultas en subconsultas para distintos nodos

**Desventajas:** 

- Coste de desarrollo de software 
- Mayor probabilidad de error 
- Mayor tiempo de procesamiento 
- Tiempo de la red de comunicación



### 2. Transparencia

Es lo mismo trabajar contra un SGBD que contra un SGBDD. 

Niveles: 

- **Red**: ocultar los detalles de la distribución de la información en la red. 

  - Concepto relacionado (opuesto): autonomía local

- **Nombres de datos (Asignación de nombres):** cada elemento, un solo nombre. Posibilidades:

  - Asignador de nombres centralizado: 
    - cuello de botella
    - sensible a caídas
    - poca autonomía local
  - Prefijo de nodo: 
    - mayor autonomía local
    - menor transparencia de red

- **Copia y fragmentación:** 

  - **La réplica es beneficiosa:** 

    - lectura sobre datos locales
    - objeto accesible si una de sus copias lo está 

    Transparente al usuario en el acceso (implica actualización de todas las copias)

  - **Fragmentación:** 

    - Horizontal (a nivel de instancia) 
    - Vertical (a nivel de esquema) 

    Fragmento: sub-relación obtenida mediante proyección y/o selección. Es necesaria una nomenclatura unificada, por ejemplo: 

    ```
    nodo<refnodo>.<relacion>.f<idfrag>.r<idcopia>
    ```

- **Localización de fragmentos y copias:** la nomenclatura unificada viola la transparencia de red. Se usan alias simples que el sistema traduce a nomenclatura unificada. 

  Requiere de un esquema de asignación de nombres.

- **Actualizaciones:** Todas las copias deben actualizarse. El problema está relacionado con el problema de actualización de vistas.



### 3. Diseño

- Catálogo global: contiene los metadatos sobre la distribución.

  - Una posibilidad: en un solo nodo. ¡Vulnerable! 

  - Otra posibilidad: copias en todos los nodos. ¡Poco eficiente! 

    **Solución:** catálogo distribuido (cada nodo tiene información de sus objetos, las copias y los fragmentos)

#### Las 12 reglas de DATE

1. Autonomía local 

2. Independencia de nodo central

3. Operación continua 

4. Independencia con respecto a la localización 

5. Independencia con respecto a la fragmentación 

6. Independencia de réplica 

7. Procesamiento distribuido de consultas

8. Manejo distribuido de transacciones 

9. Independencia con respecto al equipo

10. Independencia con respecto del S.O. 

11. Independencia con respecto de la red

12. Independencia con respecto del SGBD

    - Diversos SGBD entienden el mismo estándar de lenguaje pero permiten variaciones: la traducción la realiza una compuerta (programa que hace que un SGBD actúe como otro distinto para permitir la conexión).

      Si un usuario accede por un SGBD concreto, la transparencia garantiza que verá toda la BDD como se ve en su SGBD de entrada. Es responsabilidad de su SGBD proveer de la compuerta.

      - **Compuertas:** debe... 

        - Incluir protocolos para el intercambio de información con el otro SGBD (de manejo y de resultados) 
        - Actuar como servidor para el otro SGBD 
        - Proveer correspondencia de tipos 
        - Proveer correspondencia de dialectos SQL 
        - Proveer correspondencia de feedback 
        - Proveer correspondencia de diccionarios 
        - Proveer de protocolos de dos fases para las transacciones compatibles 
        - Proveer de bloqueo de datos en el SGBD de destino

        

### 4. Recuperación

Dos gestores: – Gestor de transacciones distribuidas – Gestores de transacciones locales Todos colaboran para que las transacciones globales sean correctas. Cada nodo tiene: – Gestor local de transacciones – Coordinador de transacciones

### 5. Control de concurrencia