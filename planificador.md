# Planificador

Es el proceso encargado de orquestar la ejecución de los procesos ESI. Los procesos ESI llegarán a una cola de listos, donde se los planificará para su próxima ejecución según un algoritmo predeterminado por archivo de configuración.

A razón de simplificar el desarrollo, la ejecución de cada línea de código _del script a ser interpretado por el ESI_ **será atómica** y cada vez que una instrucción sea ejecutada se le informará al planificador. En este sentido el Planificador actúa como un Clock del sistema. No se deberá re-planificar los ESI a seguir ejecutando al recibir la confirmación de sentencia ejecutada.

En caso de que un proceso ESI no pueda seguir ejecutando debido a un bloqueo, deberá ser enviado a la cola de proceso bloqueados, esperando a que quien bloqueó la key utilice un _store_ y libere la misma.

Por otro lado, en el caso de que el ESI haga una operación de GET sobre un recurso libre, no deberá moverse del estado de ejecución ni ser re-planificado \(a menos que el algoritmo del planificador así lo indique\).

Una vez finalizado el proceso ESI, se lo enviará a una cola de _finalizados_.

Es importante destacar que cada proceso ESI deberá tener un identificador dentro del sistema.

![Planificaci&#xF3;n](.gitbook/assets/planificacion.png)

## Algoritmos de Planificación ^6

La ejecución de las instrucciones de cada ESI estará dividida en ráfagas de CPU y ráfagas de bloqueo. Dado que no podemos predecir con certeza la duración de cada ráfaga, el planificador será responsable de realizar las estimaciones correspondientes, cuando sea necesario. La unidad a utilizar será "sentencias" en lugar de tiempo, por cuestiones de simplicidad.

A partir de estas estimaciones, el planificador podrá utilizar los siguientes algoritmos, los cuales deberán poder ser modificados, junto con sus parámetros, mediante el archivo de configuración al momento de inicializar el planificador:

1. _Shortest Job First:_ se dará prioridad al ESI cuya próxima ráfaga sea la más corta. Deberá soportar los modos con y sin desalojo.
2. _Highest Response Ratio Next:_ se dará prioridad al ESI cuyo response ratio sea el más alto. 

En ambos algoritmos se desconoce la próxima ráfaga, por lo que será estimada utilizando la fórmula de la media exponencial. La formula para esta estimación será la siguiente:

![t\_{n+1} = \alpha t\_n+\(1-\alpha\) \tau \_{n+1}\\\textup{Siendo}\\t\_n \textup{ la duraci\&apos;on de la rafaga } n\\\alpha, 0 \leq \alpha \leq 1\\\tau\_n \textup{ la estimaci\&apos;on de la rafaga } n](.gitbook/assets/estimacion.png)

La estimación inicial de todos los ESI será la misma, y deberá poder ser modificable por archivo de configuración. Ante un empate en la estimación se podrá optar por utilizar el algoritmo de FCFS \(First Come First Served\)

## Consola del planificador

Mediante una consola, el planificador deberá facilitar al usuario las siguientes operaciones:

* Pausar/Continuar planificación\(^2\): El Planificador no le dará nuevas órdenes de ejecución a ningún ESI mientras se encuentre pausado.
* bloquear _clave ID_: Se bloqueará el proceso ESI hasta ser desbloqueado _\(ver más adelante\)_, especificado por dicho _ID_\(^3\) en la cola del recurso _clave_. _Vale recordar que cada línea del script a ejecutar es atómica, y no podrá ser interrumpida; si no que se bloqueará en la próxima oportunidad posible. Solo se podrán bloquear de esta manera ESIs que estén en el estado de listo o ejecutando._
* desbloquear _clave_: Se desbloqueara el primer proceso ESI bloquedo por la _clave_ especificada, en caso de que no queden mas procesos bloqueados se debera liberar la clave.
* listar _recurso_: Lista los procesos encolados esperando al recurso.
* kill _ID_: finaliza el proceso. Recordando la atomicidad mencionada en “bloquear”. Al momento de eliminar el ESI, se debloquearan las claves que tenga tomadas.
* status _clave_: Con el objetivo de conocer el estado de una clave y de probar la correcta distribución de las mismas se deberan obtener los siguientes valores: \(Este comando se utilizara para probar el sistema\)
  * Valor, en caso de no poseer valor un mensaje que lo indique.
  * Instancia actual en la cual se encuentra la clave. \(En caso de que la clave no se encuentre en una instancia, no se debe mostrar este valor\)
  * Instancia en la cual se guardaría actualmente la clave \(Calcular este valor mediante el algoritmo de distribución\(^4\), pero sin afectar la distribución actual de las claves\).
  * ESIs bloqueados a la espera de dicha clave.
* deadlock: Esta consola también permitirá analizar los deadlocks que existan en el sistema y a que ESI están asociados. Pudiendo resolverlos manualmente con la sentencia de kill previamente descrita.

## Configuración

| Campo | Tipo | Ejemplo |
| --- | --- | --- |
| Puerto de Escucha de conexiones | \[numérico\] | `8000` |
| Algoritmo de planificación | `SJF-CD` / `SJF-SD` / `HRRN` | `HRRN` |
| Alfa planificación | \[numérico entre 0 y 100\] | `30` \(^5\) |
| Estimación inicial | \[numérico\] | `5` |
| IP de Conexión al Coordinador | \[cadena\] | `"127.0.0.1"` |
| Puerto de Conexión al Coordinador | \[numérico\] | `8001` |
| Claves inicialmente bloqueadas | \[Lista de claves\] | `materias:K3002, materias:K3001` |

^2: Esto se puede lograr ejecutando una sycall bloqueante que espere la entrada de un humano.

^3: El Planificador empezará con una serie de claves bloqueadas de esta manera.

^4: Estos algoritmos se detallarán más adelante.

^5: Para parsear este valor de forma más sencilla, recomendamos cargarlo como un entero de 0 a 100 y dividirlo por 100 antes de usarlo, con el fin de que sea un coeficiente entre 0 y 1

^6: Para mayor información sobre los algoritmos pueden recurrir a: Sistemas Operativos, Silberschatz, Galvin - Capítulo 5: Planificación de la CPU Sistemas Operativos, William Stallings 5ta Ed - Capítulo 9: Planificación uniprocesador \(En otras ediciones varía el capitulo\)

