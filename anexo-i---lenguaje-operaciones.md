# Anexo I: Lenguaje - Operaciones

* `SET key value`: Almacena el valor con la clave asociada.
* `GET key`: Obtiene el valor de la clave asociada cumpliendo con el listado de opciones que se hayan utilizado.
* `STORE key`: Persiste el valor de la clave asociada en un archivo dentro de la Instancia. Por cada clave persistida se generará un archivo particular conteniendo el valor en texto plano. Esta operación también debe liberar el bloqueo sobre la clave.

## Blocking

En el sistema de Re Distinto, el bloqueo se realiza sobre las claves que son solicitadas a lo largo de la ejecución de un script. Específicamente, los bloqueos se aplicarán a medida que se trabaja con una clave[^16] y solo se liberaran dichas claves cuando se realice una operación `STORE` o cuando termine la ejecución del script que bloqueo la clave.

| Script 1             | Script 2             |
|----------------------|----------------------|
| `GET materias:K3001` | `GET materias:K2001` |
| `GET materias:K2001` | `GET materias:K3001` |

En este caso, el script 1 bloquea la clave materias:K3001 en su primer operación, impidiendo que el script 2 pueda utilizar dicha clave hasta que el script 1 la libere. De la misma manera, el script 2 hace lo mismo con la clave materias:K2001 en su primera operación. Cuando estos scripts llegan a la segunda operación, se da una particularidad y es que ambos están a la espera de que el otro script libere la clave que están usando. Este fenómeno se conoce como deadlock[^17].

## Ejemplo de Script

```
GET deportes:futbol:messi
SET deportes:futbol:messi Lionel Messi
GET deportes:basquet:ginobili
SET deportes:basquet:ginobili Emanuel Ginobili
# Persiste el dato de la instancia y libera esta clave
STORE	deportes:basquet:ginobili
```

## Manejo de Errores

### Error de Tamaño de Clave

De exceder el tamaño máximo de 40 caracteres para la Clave, la operación fallará y se informará al Usuario. Abortando el ESI culpable.

### Error de Clave no Identificada

Cuando un Usuario ejecute una operación que no sea de inserción, que requiera una clave por parámetro y ésta no exista; se deberá generar un error informando de dicha inexistencia al Usuario. Abortando el ESI culpable.

### Error de Comunicación
En caso de que ocurra un error en la comunicación con algún proceso, se deberá generar un error informando al Usuario de dicho problema. Si la desconexión ocurre entre el Planificador y el Coordinador; el sistema se considera en un estado inválido. La desconección de Instancias y de ESIs deberá estar contemplada.

### Error de Clave inexistente

Si la clave que se desea acceder no existe en el sistema, el ESI qué ejecutó ese pedido será abortado, se deberá generar un error informando al Usuario de dicho problema.

---
[^16]: Solo la operación GET bloquea una clave.
[^17]: Se verá durante la cursada con mayor detalle.
