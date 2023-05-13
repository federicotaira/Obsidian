
La programación estructurada busca dividir o descomponer un problema complejo en pequeños problemas. La solución de cada uno de esos pequeños problemas nos trae la solución del problema complejo.

En el lenguaje C el planteo de esas pequeñas soluciones al problema complejo se hace dividiendo el programa en funciones.

Una función es un conjunto de instrucciones que resuelven un problema específico. En el lenguaje C es obligatorio el planteo de una función como mínimo llamada main:

```
int main()
{
    ....
}
```

# Estructura de una función.

Una función tiene un nombre, puede tener parámetros y encerrada entre llaves le sigue su algoritmo:

```
(valor que retorna)   (nombre de la función) (parámetros de la función)
{
    (algoritmo)
}
```
## Problema
Confeccionar un programa que muestre una presentación en pantalla del programa. Solicite la carga de dos valores y nos muestre la suma. Mostrar finalmente un mensaje de despedida del programa.  

Implementar estas actividades en tres funciones.
## Codigo
```
#include<stdio.h>
#include<conio.h>

void presentacion()
{
    printf("Programa que permite cargar dos valores por teclado.\n");
    printf("Efectua la suma de los valores\n");
    printf("Muestra el resultado de la suma\n");
    printf("*******************************\n");
}

void cargaSuma()
{
    int valor1,valor2,suma;
    printf("Ingrese el primer valor:");
    scanf("%i",&valor1);
    printf("Ingrese el segundo valor:");
    scanf("%i",&valor2);
    suma=valor1+valor2;
    printf("La suma de los dos valores es: %i",suma);
}

void finalizacion()
{
    printf("\n*******************************\n");
    printf("Gracias por utilizar este programa");
}


int main()
{
    presentacion();
    cargaSuma();
    finalizacion();
    getch();
    return 0;
}
```
Como podemos observar hemos dispuesto previo a la función main otras tres funciones.

Lo importante es tener en cuenta que todo programa en C comienza a ejecutarse siempre a partir de la primer línea contenida en la función main, en nuestro ejemplo sería:

int main()
{
    presentacion();

Lo que estamos indicando en esta línea es llamar a la función presentacion que debe estar codificada en líneas anteriores en nuestro archivo.

La codificación de la función presentacion es:

void presentacion()
{
    printf("Programa que permite cargar dos valores por teclado.\n");
    printf("Efectua la suma de los valores\n");
    printf("Muestra el resultado de la suma\n");
    printf("*******************************\n");
}

Veamos un poco la sintaxis necesaria para crear una función en el lenguaje C.  
Toda función tiene un nombre que no puede tener espacios en blanco, comenzar con un número ni tener caracteres especiales. Previo al nombre de la función indicamos el dato que devuelve la función, con la palabra clave void indicamos que la función no retorna valor (no requiere que tenga la palabra clave return como hemos dispuesto siempre en la función main)

Luego del nombre de la función indicamos entre paréntesis los parámetros, si no los tiene disponemos los paréntesis abiertos y cerrados y no disponemos un punto y coma al final:

void presentacion()

Luego del nombre de la función debe ir encerrado entre llaves el algoritmo propio de la función, en nuestro ejemplo disponemos una serie de llamadas a la función printf para mostrar la presentación del programa:

{
    printf("Programa que permite cargar dos valores por teclado.\n");
    printf("Efectua la suma de los valores\n");
    printf("Muestra el resultado de la suma\n");
    printf("*******************************\n");
}

Cuando se ejecuta esta función al llegar a la llave de cerrado vuelve a la función main desde donde se la llamó.

De regreso en la función main ahora se llama a la función cargaSuma:

int main()
{
    presentacion();
    cargaSuma();

El programa ahora nuevamente sale de la función main y pasa a ejecutarse la función cargaSuma:

void cargaSuma()
{
    int valor1,valor2,suma;
    printf("Ingrese el primer valor:");
    scanf("%i",&valor1);
    printf("Ingrese el segundo valor:");
    scanf("%i",&valor2);
    suma=valor1+valor2;
    printf("La suma de los dos valores es: %i",suma);
}

La función cargaSuma no tiene parámetros y no devuelve datos (void)

Dentro del algoritmo de la función cargaSuma definimos tres variables llamadas locales a la función cargaSuma. Estas variables solo existen mientras se ejecuta la función cargaSuma y no pueden ser utilizadas por las otras funciones de nuestro programa.

El algoritmo de cargar dos valores por teclado, sumarlos y mostrar la suma por pantalla no varía con lo visto anteriormente, pero si varía la ubicación de dicho algoritmo dentro de una función creada por nosotros.

Cuando finaliza de ejecutarse cada una de las instrucciones de la función cargaSuma regresa a la función main donde se procederá a llamar a la tercer función que creamos nosotros llamada finalizacion:

int main()
{
    presentacion();
    cargaSuma();
    finalizacion();

Como vemos desde la main debemos indicar el nombre exacto de la función que llamamos (recordar que el lenguaje C es sensible a mayúsculas y minúsculas)

La función finalizacion tiene por objetivo mostrar una serie de mensajes por pantalla para que el operador sepa que el programa está finalizando:

void finalizacion()
{
    printf("\n*******************************\n");
    printf("Gracias por utilizar este programa");
}

Luego de ejecutar los dos printf de la función finalizacion el programa regresa a la main:

int main()
{
    presentacion();
    cargaSuma();
    finalizacion();
    getch();
    return 0;
}

En la main solo queda la llamada a la función getch y la instrucción return que finaliza el programa.

Si ejecutamos el programa podemos ver una salida por pantalla similar a esta:
![[Pasted image 20230214093603.png]]
