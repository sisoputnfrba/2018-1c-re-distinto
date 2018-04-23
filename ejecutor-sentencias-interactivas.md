# Ejecutor Sentencias Interactivas (ESI)

Los Procesos ESI son el punto de entrada al Sistema. Estos correrán un script con instrucciones a ser ejecutadas[^1]. Sú única función es la de interpretar de a una las líneas de un programa _(que se le pasará como argumento)_ a medida que el Planificador se lo indique. Para resolver las distintas sentencias del programa a ser interpretado; deberá colaborar con el coordinador para bloquear/liberar recursos o datos.

## Conexión

Al iniciarse un Proceso ESI, éste se conectará tanto al Planificador como al Coordinador de Re Distinto e intercambiará los necesarios mensajes iniciales para confirmar la conexión con ambos. Una vez establecida la conexión, y a medida que así lo dicte el Planificador, irá parseando las sentencias del script y transmitiéndolas al Coordinador de Re Distinto, quién retornará un mensaje informando el resultado.

## Parser

Por cada una de las sentencias interpretadas, es necesario realizar un proceso de parseo, con el fin de traducir la cadena de caracteres en una operación entendible por el Sistema  Re Distinto. Para esto, la cátedra proveerá a los alumnos del Parser de Re Distinto ([ParSI](https://github.com/sisoputnfrba/parsi)), ya que la implementación del mismo no concierne a los temas de la materia.

## Interacción con Coordinador y Planificador

1. Se recibe una solicitud de ejecución desde el Planificador de Re Distinto a través de la conexión.
2. Se obtiene la próxima sentencia a ser ejecutada según el programa a interpretar.
3. En caso de ser necesario, se envía una solicitud al Coordinador de Re Distinto.
4. El Coordinador retorna un resultado, ya sea por éxito de la operación o por un fallo de la misma.
5. Se le envía el resultado de este al planificador.

## Configuración

| Campo                              | Tipo       | Ejemplo       |
|------------------------------------|------------|---------------|
| IP de Conexión al Coordinador      | [cadena]   | `"127.0.0.1"` |
| Puerto de Conexión al Coordinador  | [numérico] | `8000`        |
| IP de Conexión al Planificador     | [cadena]   | `"127.0.0.2"` |
| Puerto de Conexión al Planificador | [numérico] | `8001`        |

---
[^1]: Para conocer en profundidad las instrucciones ver el [Anexo I](anexo-i---lenguaje-operaciones.md).
