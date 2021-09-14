# Ejercicio 0 - Contador de palabras
Taller de Programación I - Ejercicio 0\
Nicolás De Giácomo\
99702

## Contenido
* [0. Paso 0](#0-paso-0-entorno-de-trabajo)
  * [0.1. Hola Mundo](#01-aplicación-hola-mundo)
  * [0.2. Valgrind](#02-utilidad-de-valgrind)
  * [0.3. Instrucción `sizeof`](#03-qué-representa-sizeof)
  * [0.4. Estructuras y `sizeof`](#04-estructuras-y-sizeof)
  * [0.5. Archivos estándar](#05-archivos-estándar)
* [1. Paso 1](#1-paso-1-sercom---errores-de-generación-y-normas-de-programación)
  * [1.1. Problemas de estilo](#11-problemas-de-estilo)
  * [1.2. Errores de generación del ejecutable](#12-errores-de-generación-del-ejecutable)
  * [1.3. Warnings](#13-warnings)
* [2. Paso 2](#2-paso-2-sercom---errores-de-generación-2)
  * [2.1. Correcciones](#21-correcciones)
  * [2.2. Errores en las normas](#22-errores-en-las-normas)
  * [2.3. Errores en la generación del ejecutable](#23-errores-de-generación-del-ejecutable)
* [3. Paso 3](#3-paso-3-sercom---errores-de-generación-3)
  * [3.1. Correcciones](#31-correcciones)
  * [3.2. Errores de generación del ejecutable](#32-errores-de-generación-del-ejecutable)
* [4. Paso 4](#4-paso-4-sercom---memory-leaks-y-buffer-overflows)
  * [4.1. Correcciones](#41-correcciones)
  * [4.2. Prueba _TDA_](#42-prueba-tda)
  * [4.3. Prueba _Long Filename_](#43-prueba-long-filename)
  * [4.4. Segmentation Fault y Buffer Overflow](#44-segmentation-fault-y-buffer-overflow)
* [5. Paso 5](#5-paso-5-sercom---código-de-retorno-y-salida-estándar)
  * [5.1. Correcciones](#51-correcciones)
  * [5.2. Pruebas](#52-pruebas-invalid-file-y-single-word)
  * [5.3. Hexdump](#53-hexdump)
  * [5.4. GDB](#54-gdb)
* [6. Paso 6](#6-paso-6-sercom---entrega-exitosa)
  * [6.1. Correcciones](#61-correcciones)
  * [6.2. Entregas](#62-entregas-realizadas)
  * [6.3. Ejecución local](#63-ejecución-local)
* [7. Paso 7](#7-paso-7-sercom---revisión-de-la-entrega)
* [8. Paso 8](#8-paso-8-sercom---netcat-ss-y-tiburoncin)

## 0. Paso 0: Entorno de Trabajo
Se preparó un ambiente local con las siguientes características:
- Ubuntu Focal 20.04
- GCC 9.3.0
- Valgrind 3.15.0
- GDB 9.2

### 0.1. Aplicación "Hola Mundo"
Se programó una aplicación que imprime "Hola Mundo" en la pantalla y retorna 0.

![img_1.png](images/img0.png)

### 0.2. Utilidad de Valgrind
Valgrind es un programa utilizado para la detección de errores en el manejo de la memoria (heap) de las aplicaciones.\
Dentro de las opciones más comunes se encuentran:
- `-q` _(quiet)_. Configura a Valgrind para que se ejecute "silenciosamente", es decir, imprimiendo menos mensajes (solamente los errores).
- `-v` _(verbose)_. Lo contrario a `-q`, configura a Valgrind para que imprima información extra.
- `-s` _(show error list)_. Al final de la ejecución muestra una lista con los errores detectados.
- `-h` _(help)_. Muestra el mensaje de ayuda.

### 0.3. ¿Qué representa `sizeof()`?
La instrucción `sizeof()` devuelve un entero que representa la cantidad de **bytes** que ocupa el tipo de variable que recibe por parámetro.\
En la arquitectura descrita anteriormente el resultado de la llamada a `sizeof()` sería 1 para el tipo `char` y 4 para el tipo `int`.

### 0.4. Estructuras y `sizeof()`.
Cuando se llama a la función `sizeof()` y se pasa como parámetro una estructura, el resultado no es necesariamente la suma de los `sizeof()` de cada tipo individual.\
Por ejemplo, se puede tener una estructura en la que el compilador deje un espacio de **bytes** sin modificar llamado **padding**.\
A continuación se presenta un ejemplo de cada caso.\
En el primer ejemplo se puede observar una estructura que no utiliza **padding**.
```
struct Example {
    int a;
    int b;
};
```
Si utilizamos `sizeof()` en esta estructura, el resultado será la suma de los `sizeof()` de sus elementos.

![equation](http://www.sciweavers.org/upload/Tex2Img_1631382338/render.png)

Ahora, tenemos una nueva estructura que utiliza padding luego del primer elemento.
```
struct Example {
    char c;
    int a;
    int b;
};
```
En este caso el `sizeof` de la estructura es mayor que el `sizeof` de la suma de sus elementos.

![equation](http://www.sciweavers.org/upload/Tex2Img_1631382460/render.png)

### 0.5. Archivos estándar
Los archivos estándar corresponden a tres streams (canales de comunicación) de datos definidos por el sistema.
- `STDIN` _Standard Input_. Utilizado por los programas para leer los datos de entrada.
- `STDOUT` _Standard Output_. Utilizado para escribir la salida de un programa.
- `STDERR` _Standard Error_. Archivo independiente de `STDOUT`, en el cual se escriben los mensajes de error.

Estos archivos pueden ser redirigidos a través del **pipeline**. Este es un mecanismo que permite la comunicación entre diferentes programas.\
Utilizando el símbolo **pipe** (`|`), se pueden conectar las todas las salidas estándar entre procesos.

```
comando1 | comando2
```
Utilizando el símbolo `>` se puede redireccionar la salida de un proceso.
```
comando1 > comando2
```
Utilizando el símbolo `<` se puede redireccionar la entrada de un proceso.
```
comando1 < comando2
```
## 1. Paso 1: SERCOM - Errores de generación y normas de programación
### 1.1. Problemas de estilo
En el programa se detectaron once problemas de estilo que van a ser descritos a continuación.

![img_2.png](images/img2.png)

1. En la línea `paso1_wordscounter.c:27` debe haber un espacio entre el _while_ y el paréntesis posterior.
2. En la línea `paso1_wordscounter.c:41` la sentencia dentro del _if_ tiene diferente cantidad de espacios a la derecha y a la izquierda.
3. En la línea `paso1_wordscounter.c:41` la sentencia dentro del _if_ debería tener solamente uno o ningún espacio que lo separe del paréntesis correspondiente.
4. En la línea `paso1_wordscounter.c:47` la instrucción `else` debería estar ubicada en la misma línea que el cierre de llaves del _if_ correspondiente.
5. En la línea `paso1_wordscounter.c:47`, si una instrucción _else_ tiene una llave de un lado, debería tenerla de ambos.
6. En la línea `paso1_wordscounter.c:48` debería haber un espacio entre la instrucción _if_ y el paréntesis siguiente.
7. En la línea `paso1_wordscounter.c:53` hay un espacio extra previo al símbolo `;`.
8. En la línea `paso1_main.c:12` debería usarse la instrucción `snprintf` en lugar de `strcpy`.
9. En la línea `paso1_main.c:15` se encuentra el mismo problema que el descrito en el ítem 4.
10. En la línea `paso1_main.c:15`, se encuentra el mismo problema que el descrito en el ítem 5.
11. En la línea `paso1_wordscounter.h:5` se supera el máximo sugerido de 80 caracteres.

### 1.2. Errores de generación del ejecutable
Dentro de los errores detectados durante la generación del ejecutable encontramos el error _implicit declaration of function_ que se repite en cuatro líneas (23, 24, 25 y 27) dentro del archivo _main.c_.
Este error hace referencia a que se está usando una función que no se declaró anteriormente.\
Se trata de un error en la etapa de compilación.

![img.png](images/img1.png)

Luego, también podemos encontrar el error _unknown type name_ que ocurre en la línea 22 del archivo _main.c_.
Este error nos indica que el tipo en cuestión (_wordscounter_t_) no fue definido previamente.\
Se trata de un error en la etapa de compilación.

![img_1.png](images/img3.png)

### 1.3 Warnings
El sistema no reportó ningún warning debido al flag `-Werror` utilizado al compilar. Este flag le indica al compilador que todos los warnings detectados deben ser tratados como errores.

## 2. Paso 2: SERCOM - Errores de generación 2
### 2.1. Correcciones
En el archivo **main.c**:
- Se incluyó el archivo `wordscounter.h` que contiene definiciones.
- Se reemplazó la función `strcpy` por `memcpy`.
- Se puso la instrucción _else_ en la misma línea que el cierre de llaves del _if_ anterior.

En el archivo **wordscounter.c**:
- Se realizaron correcciones de estilo en las líneas 13, 26, 40, 45, 46 y 51.

En el archivo **wordscounter.h**:
- Se arregló el largo de la línea 5.

### 2.2. Errores en las normas
No se detectaron problemas en las normas controladas por **Cpplint**.

![img.png](images/img4.png)

### 2.3 Errores de generación del ejecutable
En la generación del ejecutable se presentaron los siguientes errores.

Hubo tres instancias en las que se detectó el error _unknown type name_. Este es un error del compilador que se debe a un tipo no reconocido (no declarado previamente).\
Para ayudar con este error, el compilador indica que el tipo `FILE` está definido en la librería `<stdio.h>` y el tipo `size_t` en la `<stddef.h>`.

![img_1.png](images/img5.png)
![img.png](images/img6.png)

Luego, se obtiene el error _conflicting types_. Este es un error del compilador que está indicando que una función está siendo declarada por una segunda vez, con un tipo diferente de la primera declaración.

![img.png](images/img7.png)

Por último, tenemos el error _implicit declaration of function_. Este es un error del compilador que nos indica que cierta función (en este caso la función `malloc`) no fue declarada antes de ser invocada.\
El compilador provee un mensaje de ayuda indicando que la librería `<stdlib.h>` tiene una declaración de esta función.

![img_1.png](images/img8.png)

## 3. Paso 3: SERCOM - Errores de generación 3
### 3.1. Correcciones
En el archivo **wordscounter.c**:
- Se importó la librería `<stdlib.h>`.

En el archivo **wordscounter.h**:
- Se importó la librería `<string.h>`.
- Se importó la librería `<stdio.h>`.

### 3.2. Errores de generación del ejecutable
Se obtuvo un único error: _undefined reference_. Este es un error del linker, que está indicando que una función que fue **declarada** (por eso no falló el compilador) nunca fue **definida**.

![img.png](images/img9.png)

## 4. Paso 4: SERCOM - Memory Leaks y Buffer Overflows
### 4.1. Correcciones
Se agregó la definición de la función _wordscounter_destroy_ en el archivo `wordscounter.c`.

### 4.2. Prueba _TDA_
En la ejecución con _Valgrind_ de la prueba _TDA_ se encontraron los siguientes errores.

Primero, un file descriptor no fue cerrado antes del término de la ejecución.

![img.png](images/img.png)

Segundo, se encontraron problemas con el manejo de la memoria que dejaron _leaks_.

![img_1.png](images/img10.png)
![img_2.png](images/img11.png)

### 4.3. Prueba _Long Filename_
En la ejecución con _Valgrind_ de la prueba _Long Filename_ se encontró un _buffer overflow_ al usar `memcpy` en la línea `paso4_main.c:13`.

![img_3.png](images/img12.png)

El error se podría solucionar utilizando la función `strncpy`. Sin embargo, esto podría un límite de caracteres al nombre del archivo que de ser superado hará que el programa devuelva un código de error (no va a encontrar el archivo teniendo el nombre incompleto).

### 4.4. Segmentation Fault y Buffer Overflow
Un _Segmentation Fault_ se obtiene cuando se quiere acceder a una parte de la memoria a la cual no se tiene permisos. Por ejemplo, cuando por error se intenta modificar un espacio de memoria dentro del _Code Segment_.

Un _Buffer Overflow_ se obtiene cuando se intenta guardar datos dentro de un _buffer_ (por ejemplo un arreglo) con un largo que supera al mismo. Esto provoca que se corrompan (remplacen sus datos) los segmentos de memoria adyacentes al _buffer_.

## 5. Paso 5: SERCOM - Código de retorno y salida estándar
### 5.1. Correcciones
En el archivo `wordscounter.c` previo, el array con los caracteres delimitadores de palabras se guardaba en el _heap_ utilizando `malloc`. En la nueva versión se utiliza una constate y el programador ya no debe encargarse de la liberación de este espacio de memoria.

En el archivo `main.c` se corrigió el problema con la función `memcpy` y ahora para el llamado a la función `fopen` se utiliza el argumento recibido por parámetro sin almacenar los datos en otra variable.\
Por otro lado, se agregó el cierre del archivo (en caso de que no sea el estándar) al final del programa.

### 5.2. Pruebas _Invalid File_ y _Single Word_
En el caso de la prueba _Invalid File_ se espera que se retorne un **1** (código de error), pero se obtiene un **255** del programa.

![img_4.png](images/img13.png)

En el caso de la prueba _Single Word_ se espera que el programa retorne el resultado **1** (una palabra contada), pero retornó el resultado **0**.

![img_5.png](images/img14.png)

### 5.3. Hexdump

Utilizando la herramienta hexdump se puede ver que el archivo `input_single_word.txt` contiene 4 caracteres, siendo el último el caracter con ASCII 0x64 **'d'**.
![img_6.png](images/img15.png)

### 5.4. GDB
Se utilizaron los siguientes comandos:
- `info functions`: Lista las funciones declaradas por archivo.
- `list wordscounter_next_state`: Muestra la definición de la función.
- `break 45`: Coloca un _breakpoint_ en la línea 45 
- `run input_single_word.txt`: Ejecuta el programa con el parámetro correspondiente.
- `quit`: Sale de la herramienta GDB.

En _debugger_ no se detuvo en la línea 45, ya que, esta línea nunca se ejecuta. Esto se puede ver claramente observando que el programa retorna 0 palabras contadas para este archivo.\
La ejecución del programa termina (erróneamente) antes de sumar 1 a la variable que contiene la cantidad de palabras contadas hasta el momento.

## 6. Paso 6: SERCOM - Entrega exitosa
### 6.1. Correcciones
En el archivo `main.c` se corrigió el _status code_ devuelto en caso de error.

En el archivo `wordscounter.c`, se pasó a utilizar un `define` para la cadena de caracteres delimitadores. Luego, se arregló la lógica dentro de la función `wordscounter_next_state` (ver [5.2. Pruebas](#52-pruebas-invalid-file-y-single-word) y [5.4. GDB](#54-gdb)).

### 6.2. Entregas realizadas
A continuación se muestran las entregas realizadas hasta el momento.

![img_8.png](images/img17.png)

![img_9.png](images/img18.png)

### 6.3. Ejecución local
A continuación se muestra la ejecución local de la prueba _Single Word_

![img_10.png](images/img19.png)

## 7. Paso 7: SERCOM - Revisión de la entrega
Todos los casos de prueba (públicos y privados) fueron superados.\
Valgrind no detectó ningún error en el manejo de memoria o de archivos.\
Se superaron los estándares de estilo del código.

## 8. Paso 8: SERCOM - Netcat, ss y tiburoncin
El flag `-l` de _Netcat_ le indica al programa que se ponga en modo escucha (_listen mode_), contrariamente al modo cliente (_client mode_).\
El flag `-p` indica el puerto local que se va a escuchar, se utiliza cuando se está en _listen mode_.

Con el comando `ss -tuplan` se muestra la siguiente información en el puerto `9081`.

![img.png](images/img20.png)

Luego de escribir en la segunda consola, se puede observar que la primera consola recibe los mensajes.

![img_3.png](images/img23.png)

Ahora, con el comando `ss` se observan dos netcats en estado `ESTAB`.

![img_1.png](images/img21.png)
![img_2.png](images/img22.png)

El programa **Tiburoncin** imprimió los mensajes enviados y recibidos entre ambos `netcat`. Lo imprime de la misma forma que `hexdump` mostraría el contenido de un archivo: de forma formateada por columnas y en hexadecimal.\
Estos tipos de programas se conocen como _man in the middle_, ya que se "posicionan" entre ambos programas y leen y/o alteran el contenido de la información intercambiada entre los mismos.

Finalmente, el programa deja dos archivos con los mensajes intercambiados entre los programas.

![img_4.png](images/img24.png)