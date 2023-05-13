Hemos visto en el concepto anterior que en el lenguaje C para trabajar con cadenas de caracteres debemos definir un vector de caracteres de un tamaño definido, por ejemplo:

char palabra[31];

En la línea anterior estamos definiendo un vector de caracteres que nos permite almacenar hasta 30 caracteres (recordar que una posición del vector se requiere para el terminador de cadena '\0')

Desarrollamos anteriormente un algoritmo para contar la cantidad de letras que almacena una cadena de caracteres, básicamente recorrimos mediante un while y contamos cada caracter hasta que encontramos el terminador de cadena '\0'.

Hay muchos algoritmos de uso común para trabajar con cadenas de caracteres, el lenguaje C viene con un conjunto de funciones que nos permiten administrar las cadenas de caracteres.

Para facilitar el trabajo con las cadenas de caracteres debemos incluir el archivo string.h y a partir de esto usar sus funcionalidades:

``#include<string.h>``
# Funcion: strlen
## Problema
Ingresar por teclado una palabra. Mostrar luego por pantalla la cantidad de letras que tiene.
### Codigo
```Ejercicio77.c
#include<stdio.h>
#include<conio.h>
#include<string.h>

int main()
{
    char palabra[31];
    printf("Ingrese una palabra:");
    gets(palabra);
    int cant=strlen(palabra);
    printf("La palabra %s tiene %i letras",palabra,cant);
    getch();
    return 0;
}
```
### Explicacion
Lo primero que hacemos ahora es incluir los archivos stdio.h, conio.h y string.h:

`#include<stdio.h>`
`#include<conio.h>`
`#include<string.h>`

La función strlen se le pasa como parámetro una variable de tipo cadena de caracteres (vector de tipo char) y nos retorna un entero que representa la cantidad de caracteres almacenados en dicha cadena:

    int cant=strlen(palabra);

El valor devuelto por la función strlen se carga en la variable cant que procedemos a mostrarla seguidamente:

    printf("La palabra %s tiene %i letras",palabra,cant);

Tener en cuenta que la llamada a la función strlen nos evita implementar el while con el contador de caracteres:

`#include<stdio.h>`
`#include<conio.h>`

```
int main()
{
    char palabra[31];
    printf("Ingrese una palabra:");
    gets(palabra);
    int cant=0;
    while (palabra[cant]!='\0')
    {
        cant++;
    }
    printf("La palabra %s tiene %i letras",palabra,cant);
    getch();
    return 0;
}
```

# Funcion: strcmp
 el lenguaje C tiene otra función llamada `strcmp` que le pasamos dos cadenas a comparar y nos retorna un entero:
**int strcmp(cadena1,cadena2)**

**Retorna un cero**`(==0)` si las dos cadenas son exactamente iguales.

**Retorna un valor mayor a cero** `(>0)` si la cadena1 es mayor alfabéticamente que la segunda.

**Retorna un valor menor a cero** `(<0)` si la cadena2 es mayor alfabéticamente que la primera.
## Problema 2:
Ingresar dos nombres por teclado. Mostrar un mensaje si son iguales y sino mostrar el que es mayor alfabéticamente.
### Codigo
```Ejercicio78.c
#include<stdio.h>
#include<conio.h>
#include<string.h>

int main()
{
    char nombre1[31];
    char nombre2[31];
    printf("Ingrese primer nombre:");
    gets(nombre1);
    printf("Ingrese segundo nombre:");
    gets(nombre2);
    if (strcmp(nombre1,nombre2)==0)
    {
        printf("Los dos nombres son iguales");
    }
    else
    {
        if (strcmp(nombre1,nombre2)>0)
        {
            printf("%s es mayor alfabeticamente",nombre1);
        }
        else
        {
            printf("%s es mayor alfabeticamente",nombre2);
        }
    }
    getch();
    return 0;
}
```
### Explicacion
Primero definimos dos vectores de caracteres y los cargamos por teclado:

    char nombre1[31];
    char nombre2[31];
    printf("Ingrese primer nombre:");
    gets(nombre1);
    printf("Ingrese segundo nombre:");
    gets(nombre2);

Para controlar si los dos nombres son iguales verificamos si la función strcmp retorna un cero:

    if (strcmp(nombre1,nombre2)==0)
    {
        printf("Los dos nombres son iguales");
    }

En el caso que retorne un valor mayor a cero significa que el primer nombre es mayor alfabéticamente:

        if (strcmp(nombre1,nombre2)>0)
        {
            printf("%s es mayor alfabeticamente",nombre1);
        }

### Funcionamiento interno de strcmp.

Definimos dos cadenas de 7 elementos y las inicializamos:

	char cadena1[7]="Bien";
	char cadena2[7]="Bueno";

El contenido de las componente de cada una de las cadenas es el siguiente:

| componente | [0] | [1] | [2] | [3] | [4]  | [5]  | [6] |
| ---------- | --- | --- | --- | --- | ---- | ---- | --- |
| cadena1    | ‘B’ | ‘i’ | ‘e’ | ‘n’ | ‘\0’ | ---  | --- |
| cadena2    | ‘B’ | ‘u’ | ‘e’ | ‘n’ | ‘o’  | ‘\0’ |     |

El contenido de las componente de cada una de las cadenas (en valores ASCII) es el siguiente:

| componente | [0] | [1] | [2] | [3] | [4]  | [5]  | [6] |
| ---------- | --- | --- | --- | --- | ---- | ---- | --- |
| cadena1    | 66 | 105 | 101 | 110 | 0 | ---  | --- |
| cadena2    | 66 | 117 | 101 | 110 | 111  | 0 |     |


Seguidamente llamamos a la función strcmp, la cual devuelve un valor entero que le asignamos a una variable resultado:

	int resultado=strcmp(cadena1,cadena2);

Internamente la función hace lo siguiente:  
Resta al valor ASCII de la primer componente de cadena1 (66) el valor de la primer componente de cadena2 (66). Como el resultado es cero (0), continúa con la siguiente componente. Resta al valor de la segunda componente de cadena1 (105) el valor de la segunda componente de cadena2 (117), como el resultado es diferente de cero no sigue comparando y devuelve ese valor (-12):

| componente | [0] | [1] | [2] | [3] | [4] | [5] | [6] |
| ---------- | --- | --- | --- | --- | --- | --- | --- |
| cadena1    | 66  | 105 | 101 | 110 | 0   | --- | --- |
| cadena2    | 66  | 117 | 101 | 110 | 111 | 0   |     |
| Resultado  | 0   | -12    |     |     |     |     |     |

Esa es la explicación de porque strcmp retorna 0 si son iguales las dos cadenas y un valor mayor a 0 si la primer cadena es mayor a la segunda y viceversa.

# Funcion: strcpy

La función strcpy tiene dos parámetros de tipo string, al primer parámetro se le copia el string del segundo parámetro:

`strcpy(cadena1,cadena2)
`
Es decir que en cadena1 se copia el contenido de cadena2, si ya tenía información previa cadena1 se le borra el contenido anterior.
## Problema
Cargar por teclado dos nombres de personas que tengan distinta cantidad de caracteres. Almacenar en un tercer vector de caracteres el nombre que tenga más caracteres. Luego imprimir dicho vector.
### Codigo
```Ejercicio79
#include<stdio.h>
#include<conio.h>
#include<string.h>

int main()
{
    char nombre1[31];
    char nombre2[31];
    char nombreLargo[31];
    printf("Ingrese primer nombre:");
    gets(nombre1);
    printf("Ingrese segundo nombre:");
    gets(nombre2);
    if (strlen(nombre1)>strlen(nombre2))
    {
        strcpy(nombreLargo,nombre1);
    }
    else
    {
        strcpy(nombreLargo,nombre2);
    }
    printf("El nombre %s tiene mas caracteres",nombreLargo);
    getch();
    return 0;
}
```
### Explicacion

Definimos tres string, dos los cargamos por teclado y el tercero lo inicializamos mediante la función strcpy:

    char nombre1[31];
    char nombre2[31];
    char nombreLargo[31];

Mediante la función strlen verificamos cual de los dos string tiene más caracteres y copiamos en la variable de tipo string nombreLargo el contenido de nombre1 o nombre2 según corresponda:

    if (strlen(nombre1)>strlen(nombre2))
    {
        strcpy(nombreLargo,nombre1);
    }
    else
    {
        strcpy(nombreLargo,nombre2);
    }

Tener en cuenta que no existe la asignación:

~~nombreLargo=nombre1;~~

Si en un problema necesitamos dejar un string vacío podemos utilizar la función strcpy pasando un string sin caracteres (no debe haber ni un espacio en blanco):

strcpy(cadena,"");

Otra forma de hacer lo mismo sin utilizar la función strpy es asignar el terminador de cadena a la primer posición del string:

cadena[0]='\0';
# Funcion: strcat
Si necesitamos agregar a un string otro string podemos utilizar la función strcat.

La función strcat tiene dos parámetros de tipo string, al primer parámetro se le añade o agrega al final el string del segundo parámetro, es obligatorio que el primer parámetro esté inicializado:

`strcat(cadena1,cadena2)`
## Problema
Cargar por teclado en dos variables de tipo string el nombre y el apellido de una persona. Definir un tercer string y guardar la concatenación del nombre y apellido.
### Codigo
```Ejercicio80
#include<stdio.h>
#include<conio.h>
#include<string.h>

int main()
{
    char nombre[31];
    char apellido[31];
    char nomape[62];
    printf("Ingrese el nombre:");
    gets(nombre);
    printf("Ingrese el apellido:");
    gets(apellido);
    strcpy(nomape,nombre);
    strcat(nomape," ");
    strcat(nomape,apellido);
    printf("%s",nomape);
    getch();
    return 0;
}
```
### Explicacion
Definimos dos variables donde almacenar el nombre y apellido:

    char nombre[31];
    char apellido[31];

Como sabemos podemos guardar como máximo 30 caracteres en el nombre y 30 caracteres en el apellido, luego el string que almacenará los dos datos juntos más un espacio en blanco entre el nombre y apellido debe reservar espacio para 62 caracteres:

    char nomape[62];

Luego de cargar los dos string primero copiamos en la variable nomape el nombre de la persona empleando la función strcpy:

    strcpy(nomape,nombre);

Seguidamente le agregamos al string nomape un string con un único caracter:

    strcat(nomape," ");

Y por último le agregamos el string con el apellido:

    strcat(nomape,apellido);

Cuando ejecutamos el programa podemos ver que al imprimir la variable nomape aparecen todos los datos cargados en las variables nombre y apellido con un espacio de separación:
![[Pasted image 20230215101754.png]]