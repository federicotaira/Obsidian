Cuando en un problema sólo participan operaciones, entradas y salidas se la denomina una estructura secuencial.  
Los problemas diagramados y codificados previamente emplean solo estructuras secuenciales.

La programación requiere una práctica ininterrumpida de diagramación y codificación de problemas.

# Problema
Realizar la carga de dos números enteros por teclado e imprimir su suma y su producto

## Diagrama de flujo:
![[Captura de pantalla_20230204_121343.png]]
Tenemos dos entradas num1 y num2, dos operaciones: calcular la suma y el producto de los valores ingresados y dos salidas, que son los resultados de la suma y el producto de los valores ingresados. En el símbolo de impresión podemos indicar una o más salidas, eso queda a criterio del programador, lo mismo para indicar las entradas por teclado.
## Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    int num1, num2, suma, producto;
    printf("Ingrese primer valor:");
    scanf("%i",&num1);
    printf("Ingrese segundo valor:");
    scanf("%i",&num2);
    suma = num1 + num2;
    producto = num1 * num2;
    printf("La suma de los dos valores es:");
    printf("%i",suma);
    printf("\n");
    printf("El producto de los dos valores es:");
    printf("%i",producto);
    getch();
 return 0;
}
```


  ( # INCLUDE<stdio.h>)

La función main es donde disponemos nuestro algoritmo. Retorna un entero y lleva unos paréntesis abiertos y cerrados al final (además no lleva punto y coma al final):

int main()

Todo el algoritmo de la función main va encerrado entre llaves de apertura y cerrado:

int main()
{
   ...........
}

Dentro de la función main lo primero que hacemos es definir las variables que requiere nuestro algoritmo.  
Podemos definir varias variables en la misma línea:

    int num1, num2, suma, producto;

Mostramos un mensaje por pantalla indicando al operador que cargue el primer valor empleando la función printf:

    printf("Ingrese primer valor:");

Para la entrada de datos por teclado utilizamos la función scanf donde obligatoriamente indicamos en el primer parámetro encerrada entre comillas el tipo de dato a cargar, si es un entero "%i" y si es un float "%f".  
Como segundo parámetro indicamos el nombre de la variable a cargar antecedida por el caracter &:

    scanf("%i",&num1);

Los mismos pasos efectuamos para la carga del segundo número:

    printf("Ingrese segundo valor:");
    scanf("%i",&num2);

Las operaciones las codificamos en forma idéntica a como lo indicamos en el diagrama de flujo. Recordar que siempre una operación debe tener el operador de asignación "=":

    suma = num1 + num2;
    producto = num1 * num2;

Debemos mostrar seguidamente un mensaje por pantalla:

    printf("La suma de los dos valores es:");

y el contenido de la variable suma lo mostramos también con la función printf, teniendo en cuenta que en el primer parámetro indicamos que tipo de variable se va a mostrar ("%i" si es una variable entera y "%f" si es una variable float):

    printf("%i",suma);

Para hacer un salto de línea en pantalla para que no se amontone en la misma línea la suma y el producto debemos pasar a la función printf los caracteres \n que lo reconoce como un salto de línea:

    printf("\n");

Mostramos de forma semejante el producto por pantalla:

    printf("El producto de los dos valores es:");
    printf("%i",producto);

El resultado en pantalla de ejecutar este programa será similar a esto:

![[Pasted image 20230204123530.png]]
