Ahora en este concepto veremos como se puede almacenar un conjunto de caracteres relacionados, como por ejemplo cuando necesitamos almacenar en una variable un apellido y un nombre.

Hasta ahora podemos resolver problemas que requieran la carga de valores de tipo int (enteros), float (reales) y char (un caracter ASCII individual)

Si queremos almacenar una cadena de caracteres en el lenguaje C debemos definir un vector con componentes de tipo char.

Para definir un vector de caracteres en C debemos indicar entre corchetes la cantidad de caracteres a reservar y tener en cuenta que uno de esas posiciones se utilizará como caracter de control, es decir que si tenemos que almacenar 10 caracteres al vector lo definiremos de 11 caracteres.

# Problema1
Definir una variable para almacenar el nombre y apellido del programador. Mostrar dicho nombre por pantalla.
## Codigo 
```C
#include<stdio.h>

#include<conio.h>

  

int main()

{

    char programador[16]="Pablo Rodriguez";

    printf("Nombre del programador:");

    printf("%s",programador);

    getch();

    return 0;

}
```
Debemos definir una variable de tipo vector con elementos de tipo char:

    char programador[16]="Pablo Rodriguez";

El tamaño 16 no es un capricho, si contamos la cantidad de caracteres que hay entre las comillas incluyendo el espacio en blanco veremos que es 15. Luego recordar que debe haber un caracter extra en el vector donde se indica que ahí finalizan los datos de la cadena. No hay problema si el vector reserva más de 16 caracteres, en cambio se generarán errores inesperados si el vector tiene menos de 16 caracteres.

componente   [0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]  [8]  [9]  [10] [11] [12] [13] [14] [15]
programador  'P'  'a'  'b'  'l'  'o'  ' '  'R'  'o'  'd'  'r'  'i'  'g'  'u'  'e'  'z'  '\0'

En memoria se almacenan en cada posición del vector el caracter respectivo y cuando se ejecuta el programa y se muestran los datos mediante la función printf se muestran todos hasta que encuentra el caracter NULL que coincide con el caracter ASCII 0.

Si modificamos el programa y solo guardamos "Pablo:" sin modificar el tamaño del vector luego en memoria tenemos:

    char programador[16]="Pablo";

componente   [0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]  [8]  [9]  [10] [11] [12] [13] [14] [15]
programador  'P'  'a'  'b'  'l'  'o'  '\0' ''   ''   ''   ''   ''   ''   ''   ''   ''   ''

Lo que se almacena luego del caracter '\0' no nos importa y se lo considera basura (puede haber cualquier caracter ASCII)

Para imprimir un vector de caracteres con la función printf debemos indicar en la cadena de formato el caracter s (string):

    printf("%s",programador);

#### Carga por teclado de una cadena.

Para cargar por teclado una cadena de caracteres debemos emplear por ahora la función gets.

La función gets tiene como parámetro el nombre de la variable:

    gets(nombre del vector);

El problema de esta función que si el operador carga más caracteres que los reservados en la variable produce errores inesperados. Más adelante veremos otras soluciones de carga de cadenas de caracteres.
# Problema 2
Cargar los nombres de dos personas y sus edades. Mostrar el nombre de la persona que tiene mayor edad. Los nombres de las personan no superan los 20 caracteres.
```Ejercicio71.c
#include<stdio.h>
#include<conio.h>

int main()
{
    char nombre1[21];
    char nombre2[21];
    int edad1;
    int edad2;
    printf("Ingrese el nombre de la primer persona:");
    gets(nombre1);
    printf("Ingrese la edad:");
    scanf("%i",&edad1);
    fflush(stdin);    
    printf("Ingrese el nombre de la segundo persona:");
    gets(nombre2);
    printf("Ingrese la edad:");
    scanf("%i",&edad2);
    if (edad1>edad2)
    {
        printf("La persona con mayor edad es:");
        printf("%s",nombre1);
    }
    else
    {
        if (edad2>edad1)
        {
            printf("La persona con mayor edad es:");
            printf("%s",nombre2);
        }
        else
        {
            printf("Tienen la misma edad.");
        }
    }
    getch();
    return 0;
}
```
Definimos dos variables de tipo vector de caracteres de 21 caracteres (recordemos que se pueden guardar como máximo 20 caracteres de información):

    char nombre1[21];
    char nombre2[21];

Cargamos el nombre de la primer persona y su edad:

    printf("Ingrese el nombre de la primer persona:");
    gets(nombre1);
    printf("Ingrese la edad:");
    scanf("%i",&edad1);

Algo nuevo que hay que tener en cuenta cuando hacemos la entrada de datos por teclado es que luego de cargar la edad1 mediante la función scanf queda en el buffer de teclado el valor de la tecla "enter", luego para eliminarlo y que no se cargue en la siguiente variable debemos llamar a la función fflush:

    fflush(stdin);

Luego de haber liberado el buffer de teclado procedemos a cargar el segundo nombre y su edad:

    printf("Ingrese el nombre de la segundo persona:");
    gets(nombre2);
    printf("Ingrese la edad:");
    scanf("%i",&edad2);

Si probamos de no llamar a la función fflush podremos comprobar que en la variable "nombre2" se carga una cadena vacía.