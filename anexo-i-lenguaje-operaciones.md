# Anexo I: Lenguaje - Operaciones

* `GET clave`: Intenta bloquear el valor de la clave asociada. Si la clave no existe la crea y la bloquea.
* `SET clave valor`: Almacena el valor con la clave asociada. La clave debera estar previamente tomada por el ESI que intenta hacer el SET. 
* `STORE clave`: Persiste el valor de la clave asociada en un archivo dentro de la Instancia. Por cada clave persistida se generará un archivo particular conteniendo el valor en texto plano.

## Aclaraciones importantes sobre las operaciones

`SET`: En caso de que la clave cuente previamente con un valor, el comando SET no podra hacer que se ocupen mas entradas \(puede ocupar mas espacio hasta llenar las entradas actualmente asignadas\), pero si podra hacer que se reduzca la cantidad de entradas utilizadas, es decir, que el SET puede liberar entradas ocupadas por la clave al achicar el valor.  
`STORE`: La operación STORE no elimina ninguna entrada de las instancias ni las marca como vacías, solamente baja al archivo el valor y libera la clave en el planificador.

## Blocking

En el sistema de Re Distinto, el bloqueo se realiza sobre las claves que son solicitadas a lo largo de la ejecución de un script. Específicamente, los bloqueos se aplicarán a medida que se trabaja con una clave\(^19\) y solo se liberaran dichas claves cuando se realice una operación `STORE` o cuando termine la ejecución del script que bloqueo la clave.

| Script 1 | Script 2 |
| --- | --- |
| `GET materias:K3001` | `GET materias:K2001` |
| `GET materias:K2001` | `GET materias:K3001` |

En este caso, el script 1 bloquea la clave materias:K3001 en su primer operación, impidiendo que el script 2 pueda utilizar dicha clave hasta que el script 1 la libere. De la misma manera, el script 2 hace lo mismo con la clave materias:K2001 en su primera operación. Cuando estos scripts llegan a la segunda operación, se da una particularidad y es que ambos están a la espera de que el otro script libere la clave que están usando. Este fenómeno se conoce como deadlock\(^20\).

## Ejemplo de Script

```text
GET deportes:futbol:messi
SET deportes:futbol:messi Lionel Messi
GET deportes:basquet:ginobili
SET deportes:basquet:ginobili Emanuel Ginobili
# Persiste el dato de la instancia y libera esta clave
STORE    deportes:basquet:ginobili
```

## Manejo de Errores

### Error de Tamaño de Clave

De exceder el tamaño máximo de 40 caracteres para la Clave, la operación fallará y se informará al Usuario. Abortando el ESI culpable.

### Error de Clave no Identificada

Cuando un Usuario ejecute una operación de STORE o SET y la clave no exista, se deberá generar un error informando de dicha inexistencia al Usuario. Abortando el ESI culpable.

### Error de Comunicación

En caso de que ocurra un error en la comunicación con algún proceso, se deberá generar un error informando al Usuario de dicho problema. Si la desconexión ocurre entre el Planificador y el Coordinador; el sistema se considera en un estado inválido. La desconección de Instancias y de ESIs deberá estar contemplada.

### Error de Clave Innacesible

Si la clave que intenta acceder existe en el sistema pero se encuentra en una instancia que se encuentra desconectada se debera informar al Usuario de dicho problema y se deberá abortar el ESI.

### Error de Clave no Bloqueada

Si la clave que intenta acceder no se encuentra tomada por el ESI en cuestión, se debera informar al Usuario y se debera abortar el ESI.

^19: Solo la operación GET bloquea una clave.

^20: Se verá durante la cursada con mayor detalle.

