# Laboratorio #4
## Autores
- Josue Valenzuela

## `structs` en C

Un `struct` es un tipo de datos compuesto definido por el usuario. Definen un grupo de variables, no necesariamente del mismo tipo, bajo un mismo nombre y un mismo bloque de memoria; el grupo se puede acceder utilizando una sola dirección de memoria (Un solo puntero).

 [Leer más](https://en.wikipedia.org/wiki/Struct_(C_programming_language)

 ## Preprocesador

 Un preprocesador es un programa que procesa una entrada para producir una salida que es utilizada como entrada en un programa diferente.  El preprocesador de c toma todas las líneas que comienzan con `#` como directivas.

Una directiva es una construcción del lenguaje (que no forma parte de su sintaxis) que especifica como un compilador procesa las entradas que recibe. En c el preprocesador reemplaza las directivas con las definiciones textuales que se requieran.

 [Leer más](https://en.wikipedia.org/wiki/C_preprocessor)

 ## Diferencia entre `*` y `&`

En c `*`  es conocido como el operador de referencia, este retorna un puntero con la dirección de memoria que apunta un objeto.

El operador `&`  dado un valor retorna la dirección de memoria en la que está almacenado dado valor.

 Ejemplo:
```c
int *p; // p es un puntero a un int
int x = 0;
p = &x; // La dirección que apunta a x es asignada a p.
```
[Leer más](https://en.wikipedia.org/wiki/Dereference_operator)

## `apt` y `dpkg`

Apt es un sistema de manejo de paquetes de software. Se encarga de la instalación y eliminación de de software, originalmente diseñada para debian ha sido adaptada para el manejo de otro tipo de paquetes. Los modos de uso que apt son `update`, `upgrade`y `full-upgrade`. [Leer más](https://linux.die.net/man/8/apt)

Dpkg es una herramienta para instalar, compilar, remover, y manejo general de paquetes Debian (`.deb`).  [Leer más](https://linux.die.net/man/1/dpkg)

## ¿Cuál es el propósito de los archivos `sched.h` modificados?

Definen la estructura `sched_param`, la cual contiene los parámetros de calendarización requeridos para la implementación de cada una de las políticas de calendarización.

[Leer más](https://pubs.opengroup.org/onlinepubs/007908799/xsh/sched.h.html)

## ¿Cuál es el propósito de la definición incluida y las definiciones existentes en el archivo?

Las definiciones incluidas son las políticas de calendarización definidas con un rango de prioridad (fifo, round robin. *etc*). La nueva definición es una nueva política de calendarización y el rango de prioridad.

## ¿Qué es una task en Linux?

Task se refiere a todos los procesos que entran al calendarizador.

## ¿Cuál es el propósito de `task_struct` y cuál es su análogo en Windows?

Es la estructura de datos que describe un proceso o `task` en el sistema operativo ([Leer más](http://www.tldp.org/LDP/tlk/ds/ds.html)). En windows la estructura análoga a `struct_task` sería `EPROCESS` ([Leer más](https://manybutfinite.com/post/how-the-kernel-manages-your-memory/)).

## ¿Qué información contiene sched_param?

Esta estructura define los parámetros de calendarización. Esta se usa cuando se obtiene o establece los parámetros de calendarización. Antes de las definiciones agregadas solamente contenía la definición de la prioridad de calendarización.

[Leer más](http://www.qnx.com/developers/docs/6.5.0SP1.update/com.qnx.doc.neutrino_lib_ref/s/sched_param.html)


## ¿Para qué sirve la función `rt_policy` y para qué sirve la llamada `unlikely` en ella?

La función `rt_policy` es utilizada para decidir si la política de calendarización pertenece las políticas de tiempo real.

`unlikely` es una optimización del compilador, para favorecer a las instrucciones que esten marcadas como `likely`

[Leer más](https://stackoverflow.com/questions/109710/how-do-the-likely-unlikely-macros-in-the-linux-kernel-work-and-what-is-their-ben)

## Precedencia de prioridades para las políticas EDF, RT y CFS

## Explique el contenido de la estructura casio_task

```c
struct casio_task {
    struct rb_node casio_rb_node;
    unsigned long long absolute_deadline;
    struct list_head casio_list_node;
    struct task_struct* task;
};
```
- `casio_rb_node` es el nodo del árbol red-black en el que se encuentra la tarea.
- `absolute_deadline` es el deadline de la tarea.
- `casio_list_node` es la posición de la tarea en el *ready queue*
- `task_struct` es la información de la tarea.

## Explique el propósito de la estructura casio_rq

```c
struct casio_rq {
    struct rb_root casio_rb_root;
    struct list_head casio_list_head;
    atomic_t nr_running;
};
```
Es la información del *ready queue* para la política CASIO

## ¿Qué indica el campo .nextde esta estructura?

```c
const struct sched_class casio_sched_class = {
    .next                   = &rt_sched_class,
    .enqueue_task           = enqueue_task_casio,
    .dequeue_task           = dequeue_task_casio,
    .check_preempt_curr     = check_preempt_curr_casio,
    .pick_next_task         = pick_next_task_casio,
    .put_prev_task          = put_prev_task_casio,
#ifdef CONFIG_SMP
    .load_balance           = load_balance_casio,
    .move_one_task          = move_one_task_casio,
#endif
    .set_curr_task          = set_curr_task_casio,
    .task_tick              = task_tick_casio,
};
```
Es la siguiente tarea que va a ser ejecutada.

## Explique el ciclo de vida de una casio_taskdesde el momento en el que  se  le  asigna  esta  clase  de  calendarización  mediante sched_setscheduler.  El objetivo es que indique el orden y los escenarios en los que se ejecutan estas funciones, así   como   las   estructuras   de   datos   por   las   que   pasa.   ¿Por   quése   guardan   las casio_tasksen un red-black treey en una lista encadenada?

## ¿Cuándo preemptea una casio_taska la taskactualmente en ejecución?