# Descripción de las entregas

Los checkpoints se dividen entre "Presenciales" y "No presenciales". Este trabajo práctico contará con un único checkpoint presencial, a realizarse en el laboratorio de Medrano. Los checkpoints no presenciales son orientativos, y ayudan al grupo a medir el avance. De forma opcional, pueden ser validados con un ayudante, durante los sábados de soporte.

## Checkpoint 1 - No presencial

**Fecha**: 28 de Abril

**Tiempo estimado**: 2 semanas

**Distribución recomendada**: 5 integrantes conectando los procesos\)

### Objetivos:

* Familiarizarse con Linux y su consola, el entorno de desarrollo y el repositorio
* Aplicar las Commons Libraries, principalmente las funciones para listas, archivos de conf y logs
* Definir el Protocolo de Comunicación
* Familiarizarse con el desarrollo de procesos servidor multihilo \(proceso Coordinador, Planificador\)

### Implementación mínima:

* Creación de todos los procesos que intervienen
* Desarrollar una comunicación simple entre los procesos que permita propagar un mensaje por cada conexión necesaria
* Implementar la consola del Planificador sin funcionalidades

### Lectura recomendada:

* [http://faq.utnso.com/arrancar](http://faq.utnso.com/arrancar)
* Beej Guide to Network Programming - [link](http://beej.us/guide/bgnet/html/multi/index.html)
* Linux POSIX Threads - [link](http://www.yolinux.com/TUTORIALS/LinuxTutorialPosixThreads.html)
* SO UTN FRBA Commons Libraries - [link](https://github.com/sisoputnfrba/so-commons-library)
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 3: Procesos
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 4: Hilos

## Checkpoint 2 - No presencial

**Fecha**: 19 de Mayo

**Tiempo estimado**: 3 semanas

**Distribución recomendada**: 1 ESI - 2 Planificador - 1 Coordinador - 1 Instancia

### Objetivos:

* Implementación de la base del Protocolo de Comunicación
* Comprender y aplicar mmap\(\)
* Entender el concepto de Shared Library
* Comprender con algo de profundidad cómo funcionan algunos algoritmos sencillos

### Implementación mínima:

* Lectura de scripts y utilización del Parser del proceso ESI
* El Planificador debe ser capaz de elegir a un ESI utilizando un algoritmo sencillo \(FIFO por ej\)
* El Coordinador debe ser capaz de distribuir por Equitative Load.
* Desarrollo de lectura y escritura de Entradas en el Instancia \(Operaciones GET/SET\).

### Lectura recomendada:

* Sistemas Operativos, Silberschatz, Galvin - Capítulo 4: Hilos
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 5: Planificación

## Checkpoint 3 - Presencial - Laboratorio

**Fecha**: 9 de Junio

**Tiempo estimado**: 3 semanas

**Distribución recomendada**: 1 ESI - 2 Planificador - 1 Coordinador - 1 Instancia

### Objetivos:

* Entender las implicancias de un algoritmo de planificación real
* Entender el concepto de productor-consumidor y sus problemas de concurrencia
* Implementar algoritmos similares a los usados en Memoria Virtual

### Implementación mínima:

* ESI completo
* Planificador utilizando SJF con y sin desalojo, con todas sus colas.
* La consola del Planificador deberá poder ejecutar los comandos "Pausar/Continuar", "Bloquear", "Desbloquear" y "Listar"
* El Coordinador deberá tener el "Log de Operaciones" funcionando. También deberá ser capaz de comunicar bloqueos.
* La Instancia deberá implementar todas las instrucciones. A la hora de reemplazar claves, deberá implementar el algoritmo Circular

### Lectura recomendada:

* Sistemas Operativos, Silberschatz, Galvin - Capítulo 5: Planificación
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 6: Sincronización
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 8 y 9: Memoria

## Checkpoint 4 - No Presencial

**Fecha**: 30 de Junio

**Tiempo estimado**: 3 semanas

**Distribución recomendada**: 2 Planificador - 2 Coordinador - 1 Instancia

### Objetivos:

* Entender las implicancias de un algoritmo de planificación real
* Utilizar el concepto de "interfase" para que cada proceso planifique de forma similar, soportando diferentes algoritmos.

### Implementación mínima:

* Planificador utilizando HRRN
* La consola del Planificador deberá poder ejecutar los comandos "kill" y "status"
* El Coordinador deberá ser capaz de distribuir utilizando "LSU" y "KE". Implementar retardos
* La Instancia deberá ser capaz de soportar desconexiones y reincorporaciones. Además se deberá implementar el algoritmo LRU

### Lectura recomendada:

* Sistemas Operativos, Silberschatz, Galvin - Capítulo 5: Planificación
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 6: Sincronización
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 8 y 9: Memoria

## Entrega final - Presencial - Laboratorio

**Fecha**: 14 de Julio

**Tiempo estimado**: 2 semanas

**Distribución recomendada**: 1 Planificador - 1 Coordinador - 1 Instancia - 2 Testing general y arreglos

### Objetivos:

* Implementar un algoritmo de detección de deadlocks
* Probar el TP en un entorno distribuido
* Realizar pruebas intensivas
* Finalizar el desarrollo de todos los procesos

### Implementación mínima:

* La consola del Planificador deberá poder ejecutar el comando "deadlock"
* La Instancia deberá ser capaz de soportar dumps y compactación. Se deberá implementar el algoritmo BSU

### Lectura recomendada:

* Sistemas Operativos, Silberschatz, Galvin - Capítulo 6: Deadlock
* Sistemas Operativos, Silberschatz, Galvin - Capítulo 10 y 11: File System

**Segunda fecha de entrega**: 28 de Julio

**Última fecha de entrega**: 4 de Agosto

