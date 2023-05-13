Hemos empleado variables numéricas de distinto tipo para el almacenamiento de datos (variables int y float) En este concepto veremos otros tipos de variables que permiten almacenar un conjunto de datos en una única variable.

**Un vector en el lenguaje C es una estructura de datos que permite almacenar un CONJUNTO de datos del MISMO tipo.**
Con un único nombre se define un vector y por medio de un subíndice hacemos referencia a cada elemento del mismo (componente)
# Problema
Se desea guardar los sueldos de 5 operarios.  
Según lo conocido deberíamos definir 5 variables si queremos tener en un cierto momento los 5 sueldos almacenados en memoria.  
Empleando un vector solo se requiere definir un único nombre y accedemos a cada elemento por medio del subíndice
![[Pasted image 20230206190242.png]]
# Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    int f;
    int sueldos[5];
    //Carga del vector
    for (f=0; f<5; f++)
    {
        printf("Ingrese valor del sueldo:");
        scanf("%i",&sueldos[f]);
    }
    printf("Listado de sueldos.");
    printf("\n");
    //impresion del vector
    for(f=0; f<5; f++)
    {
        printf("%i",sueldos[f]);
        printf("\n");
    }
    getch();
    return 0;
}
```
Para la declaración de un vector le agregamos corchetes abiertos y cerrados con un valor que indica la cantidad de sueldos que podrá almacenar (en este ejemplo reservamos espacio para cinco sueldos):

    int sueldos[5];

Para cargar cada componente debemos indicar entre corchetes que elemento del vector estamos accediendo:

    for (f=0; f<5; f++)
    {
        printf("Ingrese valor del sueldo:");
        scanf("%i",&sueldos[f]);
    }

Para definir un comentario en nuestro programa en C debemos anteceder dos caracteres //  
Todo lo que dispongamos después de estos caracteres el compilador no lo tiene en cuenta y solo le es útil al programador para recordar el objetivo de esa parte del algoritmo:

    //Carga del vector

La estructura de programación que más se adapta para cargar en forma completa las componentes de un vector es un for, ya que sabemos de antemano la cantidad de valores a cargar, de todos modos no es obligatorio tener que utilizar un for.

Cuando f vale cero estamos accediendo a la primer componente del vector (en nuestro caso sería):

        scanf("%i",&sueldos[f]);

Lo mas común es utilizar una estructura repetitiva for para recorrer cada componente del vector.

Utilizar el for nos reduce la cantidad de código, si no utilizo un for debería en forma secuencial implementar el siguiente código:

    printf("Ingrese valor de la componente:");
    scanf("%i",&sueldos[0]);
    printf("Ingrese valor de la componente:");
    scanf("%i",&sueldos[1]);
    printf("Ingrese valor de la componente:");
    scanf("%i",&sueldos[2]);
    printf("Ingrese valor de la componente:");
    scanf("%i",&sueldos[3]);
    printf("Ingrese valor de la componente:");
    scanf("%i",&sueldos[4]);

La impresión de las componentes del vector lo hacemos en otro [[Estructura repetitiva for|for]]]:

    for(f=0; f<5; f++)
    {
        printf("%i",sueldos[f]);
        printf("\n");
    }

Siempre que queremos acceder a una componente del vector debemos indicar entre corchetes la componente, dicho valor comienza a numerarse en cero y continua hasta un número menos del tamaño del vector, en nuestro caso creamos el vector con 5 elementos