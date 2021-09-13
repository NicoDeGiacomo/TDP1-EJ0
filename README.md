# Ejercicio 0 - Contador de palabras
Taller de Programación I - Ejercicio 0\
Nicolás De Giácomo\
99702

## Contenido
* [Paso 0](#paso-0-entorno-de-trabajo)
  * [Hola Mundo](#aplicación-hola-mundo)
  * [Valgrind](#utilidad-de-valgrind)
  * [Instrucción `sizeof`](#qué-representa-sizeof)
  * [Estructuras y `sizeof`](#estructuras-y-sizeof)
  * [Archivos estándar](#archivos-estándar)
* [Paso 1](#paso-1-sercom---errores-de-generación-y-normas-de-programación)
  * [Problemas de estilo](#paso-8-sercom---netcat-ss-y-tiburoncin-nico)

## Paso 0: Entorno de Trabajo
Se preparó un ambiente local con las siguientes características:
- Ubuntu Focal 20.04
- GCC 9.3.0
- Valgrind 3.15.0
- GDB 9.2

### Aplicación "Hola Mundo"
Se programó una aplicación que imprime "Hola Mundo" en la pantalla y retorna 0.

![img_1.png](images/img_1.png)

### Utilidad de Valgrind
Valgrind es un programa utilizado para la detección de errores en el manejo de la memoria (heap) de las aplicaciones.\
Dentro de las opciones más comunes se encuentran:
- `-q` _(quiet)_. Configura a Valgrind para que se ejecute "silenciosamente", es decir, imprimiendo menos mensajes (solamente los errores).
- `-v` _(verbose)_. Lo contrario a `-q`, configura a Valgrind para que imprima información extra.
- `-s` _(show error list)_. Al final de la ejecución muestra una lista con los errores detectados.
- `-h` _(help)_. Muestra el mensaje de ayuda.

### ¿Qué representa `sizeof()`?
La instrucción `sizeof()` devuelve un entero que representa la cantidad de **bytes** que ocupa el tipo de variable que recibe por parámetro.\
En la arquitectura descrita anteriormente el resultado de la llamada a `sizeof()` sería 1 para el tipo `char` y 4 para el tipo `int`.

### Estructuras y `sizeof()`.
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

### Archivos estándar
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
## Paso 1: SERCOM - Errores de generación y normas de programación
### Problemas de estilo
En el programa se detectaron once problemas de estilo que van a ser descritos a continuación.

![img_2.png](images/img_2.png)

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
11. La línea `paso1_wordscounter.h:5` supera el máximo sugerido de 80 caracteres.

### Errores de generación del ejecutable
Dentro de los errores detectados durante la generación del ejecutable encontramos el error "implicit declaration of function" que se repite en cuatro líneas (23, 24, 25 y 27) dentro del archivo _main.c_.
Este error hace referencia a que se está usando una función que no se declaró anteriormente.\
Se trata de un error en la etapa de compilación.

![img.png](images/img.png)

Luego, también podemos encontrar el error "unknown type name" que ocurre en la línea 22 del archivo _main.c_.
Este error nos indica que el tipo en cuestión (_wordscounter_t_) no fue definido previamente.\
Se trata de un error en la etapa de compilación.

![img_1.png](images/img_3.png)
### Warnings
El sistema no reportó ningún warning debido al flag `-Werror` utilizado al compilar. Este flag le indica al compilador que todos los warnings detectados deben ser tratados como errores.
## Paso 2: SERCOM - Errores de generación 2
Docu
## Paso 3: SERCOM - Errores de generación 3
Docu
## Paso 4: SERCOM - Memory Leaks y Buffer Overflows
Docu
## Paso 5: SERCOM - Código de retorno y salida estándar
Docu
## Paso 6: SERCOM - Entrega exitosa
Docu
## Paso 7: SERCOM - Revisión de la entrega
Docu
## Paso 8: SERCOM - Netcat, ss y tiburoncin
Docu
## Paso 9: SERCOM -
Docu
