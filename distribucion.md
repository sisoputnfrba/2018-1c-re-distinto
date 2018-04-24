# Distribución

Para generar alta disponibilidad del sistema, el Coordinador de Re Distinto se encargará de distribuir la ejecución de las operaciones a lo largo de las Instancias de Re Distinto. Para esto, dispondrá de 3 algoritmos de distribución.

## Algoritmos de Distribución

### Equitative Load(^11)

El algoritmo Equitative Load es el más básico de todos pero, a su vez, es el menos ambiguo de todos. Consiste en despachar una solicitud a una Instancia distinta cada vez. De esta forma, la ejecución de solicitudes se distribuye de forma equitativa entre todas las Instancias.

### Least Space Used

El algoritmo LSU se basa en distribuir la ejecución de solicitudes de almacenamiento de acuerdo al espacio utilizado por cada Instancia. Para esto, el Coordinador deberá llevar cuenta del espacio en uso en cada una de las Instancias, para así poder despachar las solicitudes a aquella instancia con el mayor espacio libre _(medido en cantidad de entradas libres)_.

### Key Explicit

Por último, el algoritmo Key Explicit, define la Instancia que ejecutará cada solicitud de almacenamiento en la Clave misma. Cuando este algoritmo está en uso, se tomará el primer carácter de la clave en minúscula como indicador de Instancia a la que le corresponde la ejecución de la solicitud.

_Consideremos un instante donde existan 4 instancias, la primera instancia será la encargada de almacenar claves que comienzan con las letras "a" a la "g". La segunda instancia de la "h" a la "m". La tercera instancia de la letra "n" hasta la "t", y por último la cuarta instancia de la letra "u" a la "z"(^12). De agregarse nuevas instancias, las claves previamente almacenadas no se moverán de lugar; pero nuevas claves usarán el nuevo número de instancias para calcular la distribución._

## Desconexión

Durante la ejecución del Sistema, existe la posibilidad de que una o más Instancias dejen de estar disponibles para el Coordinador de Re Distinto. Ante éstas situaciones, el Coordinador deberá ajustar su distribución de forma acorde a la cantidad de Instancias disponibles. No se deberá eliminar una clave si la instancia de desconectó; solo se elimina cuando un ESI intenta acceder a ella; de tal forma, si la instancia se reincorpora previo al uso de la clave; el sistema sigue funcionando.

## Reincorporación

Así como las Instancias pueden dejar de estar disponibles, también pueden reincorporarse al Sistema y nuevas Instancias también pueden ser agregadas al mismo.

---
^11: Llegado el caso de que un algoritmo produzca un empate, se utilizará el algoritmo Equitative Load, el cual será el desempatador para todos los algoritmos de distribución.

^12: La letra "a" se codifica como el carácter 97 y la "z" como el 122, por lo que habrá 25 letras a ser divididas en 4 instancias, cada una con 7 letras y la última con solo 4. Cada instancia redondeará para arriba la cantidad de claves a ser usadas; y la última posiblemente tenga menos rango que aceptaría
