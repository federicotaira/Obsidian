Los operadores ++ y -- con[[Variables de tipo puntero]] tienen un funcionamiento distinto que cuando incrementamos o decrementamos una variable entera.

Cuando utilizamos el operador ++ con un puntero incrementa la variable según el tamaño del tipo de dato que apunta. Si es un puntero a entero incrementa la dirección en 4 bytes (en un sistema operativo de 32bits), si es un puntero a tipo char incrementa la dirección en 1 byte etc.

Un ejemplo de como utilizar la sintaxis de punteros:

```
#include<stdio.h>
#include<conio.h>

int main()
{
    int vec[5]={10,20,30,40,50};
    int *pe;
    pe=vec;
    printf("%i\n",*pe);  // 10
    pe++;
    printf("%i\n",*pe);  // 20
    pe++;
    printf("%i\n",*pe);  // 30
    pe--;
    printf("%i\n",*pe);  // 20
    getch();
    return 0;
}
```

Como vec y pe son punteros a enteros luego podemos asignar vec a la variable pe:

    pe=vec;

En este momento pe tiene la dirección de la primer componente del vector, luego si imprimimos lo que apunta pe tenemos el valor 10:

    printf("%i\n",*pe);  // 10

El operador ++ incrementa el contenido de la variable pe tantos bytes como sea el tamaño de las variables int en nuestro sistema operativo, por eso luego si accedemos a lo apuntado por pe tenemos el valor 20:

    pe++;
    printf("%i\n",*pe);  // 20

Un punto importante es que el vector vec si bien es un puntero no podemos utilizar el operador ++ o --, siempre apuntará a la primera componente del vector.

# Problema 1:
Desarrollar un programa para administrar un vector de 5 enteros.  
En la función de carga e impresión utilizar la sintaxis de punteros para acceder a sus elementos (no utilizar la sintaxis de subíndice)
## Codigo
```Ejercicio155.c
#include<stdio.h>

#define TAMANO 5

void cargar(int *pe)
{
    int i;
    for (i=0; i <TAMANO; i++)
    {
        printf("Ingrese valor:");
        scanf("%i", &*pe);
        pe++;
    }
}

void imprimir(int *pe)
{
    int i;
    for ( i = 0; i < TAMANO; i++)
    {
        printf("%i\n", *pe);
        pe++;
    }
}

int main()

{
    int vec[TAMANO];
    cargar(vec);
    imprimir(vec);
    return 0;
}
```
## Explicacion
En la función de carga le pasamos en cada vuelta del for la dirección que apunta la variable pe:

        scanf("%i",&*pe);

Y seguidamente incrementamos el puntero pe con el operador ++, con esto logramos que apunte a la siguiente componente del vector.

De forma similar en la impresión utilizamos la sintaxis de puntero para acceder a cada componente:

```
void imprimir(int *pe)
{
    int f;
    for(f=0;f<TAMANO;f++)
    {
        printf("%i ",*pe);
        pe++;
    }
}
```
# Problema 2
Implementar la función:

```
int largo(char *cadena)
```
Debe retornar el largo de la cadena utilizando la sintaxis de punteros para acceder a sus componentes. 
Recordar que el caracter '\0' indica el fin de la parte de información de la cadena.  
No podemos utilizar la función strlen, ya que en realidad estamos pidiendo implementar el algoritmo de dicha función.
## Codigo
```ejercicio156.c
#include<stdio.h>
#include<string.h>

int largo(char *cadena)
{
    int count = 0;
    while(*cadena!='\0')
    {
        count++;
        cadena++;
    }
     return count;
}

int main()
{
    char nombre[10]= "Maria";
    printf("el nombre %s, tiene %i caracteres", nombre, largo(nombre));
}

```