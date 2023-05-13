Cuando se tienen [[Vectores y matrices paralelas]] y se ordena uno de ellos hay que tener la precaución de intercambiar los elementos de los vectores o matrices paralelas.
# Problema 1
Confeccionar un programa que permita cargar los nombres de 5 alumnos y sus notas respectivas. Luego ordenar las notas de mayor a menor. Imprimir las notas y los nombres de los alumnos.
## Codigo
```Ejercicio127.c
#include<stdio.h>
#include<string.h>

void cargar(char nombres[5][41],int nota[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese los nombres de los alumnos:");
        fgets(nombres[i],41,stdin);
        printf("Ingrese la nota:");
        scanf("%i", &nota[i]);
        fflush(stdin);
    }
}

void ordenNotaMayorMenor(int nota[5],char nombres[5][41])
{
    int i,j;
    int auxnota;
    char auxnombres[41];
    for (i=0; i<4;i++)
    {
        for ( j = 0; j < 4; j++)
        {
            if (nota[j] < nota[j+1]) //ordena de mayor a menor
            {
                auxnota=nota[j];
                nota[j]=nota[j+1];
                nota[j+1]=auxnota;
                strcpy(auxnombres,nombres[j]);
                strcpy(nombres[j], nombres[j+1]);
                strcpy(nombres[j+1], auxnombres);
            }
        }
    }
}

void impresion(char nombres[5][41], int nota[5])
{
    printf("listado:\n");
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("%i - %s", nota[i], nombres[i]);
    }
}

int main()
{
    char nombres[5][41];
    int nota[5];
    cargar(nombres,nota);
    ordenNotaMayorMenor(nota,nombres);
    impresion(nombres,nota);
    return 0;
}
```
## Explicacion
En la función main definimos la matriz y el vector:
```
int main()
{
    char nombres[5][40];
    int notas[5];
```
La función cargar recibe la matriz y el vector para proceder a su carga por teclado:

```
void cargar(char nombres[5][41],int nota[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese los nombres de los alumnos:");
        fgets(nombres[i],41,stdin);
        printf("Ingrese la nota:");
        scanf("%i", &nota[i]);
        fflush(stdin);
    }
}
```

En el proceso de [[Ordenamiento Burbuja#Problema 2: Ordenar Mayor a menor y Menor a Mayor|ordenamiento]] dentro de los dos for verificamos si debemos intercambiar los elementos del vector notas:

```
    for (i=0; i<4;i++)
    {
        for ( j = 0; j < 4; j++)
        {
            if (nota[j] < nota[j+1]) //ordena de mayor a menor
            
```
En el caso que la nota de la posición 'j' sea menor a de la posición siguiente 'j+1' procedemos a intercambiar las notas:

```
	auxnota=nota[j];
	nota[j]=nota[j+1];
	nota[j+1]=auxnota;
```

y simultánemamente procedemos a intercambiar los elementos de la matriz paralela (con esto logramos que el vector continúen siendo paralelo a la matriz):

```
                strcpy(auxnombres,nombres[j]);
                strcpy(nombres[j], nombres[j+1]);
                strcpy(nombres[j+1], auxnombres);
```

Como vemos utilizamos dos auxiliares distintos porque los elementos de la matriz y el vector son de distinto tipo (int y cadena de caracteres)

Si deseamos ordenar alfabéticamente la condición dependerá de la matriz nombres.
# Problema 2
- Cargar en una matriz los nombres de 5 países y en un vector paralelo la cantidad de habitantes del mismo.
- Ordenar alfabéticamente e imprimir los resultados. 
- Por último ordenar con respecto a la cantidad de habitantes (de mayor a menor) e imprimir nuevamente.
## Codigo
```Ejercicio128.c
#include<stdio.h>
#include<string.h>
void cargar(char pais[5][41],int habitantes[5])
{
    int i;
    printf("Carga de paises y habitanes:\n");
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese el nombre del Pais:");
        gets(pais[i]);
        printf("Ingrese la cantidad de habitantes: ");
        scanf("%i",&habitantes[i]);
        fflush(stdin);
    }
}

void ordenAlfabetico(char pais[5][41], int habitantes[5])
{
    int i,j;
    char auxpais[41];
    int auxhabitantes;
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4-i; j++)
        {
            if (strcmp(pais[j],pais[j+1]) > 0)
            {
                strcpy(auxpais,pais[j]);
                strcpy(pais[j],pais[j+1]);
                strcpy(pais[j+1],auxpais);
                auxhabitantes=habitantes[j];
                habitantes[j]=habitantes[j+1];
                habitantes[j+1]=auxhabitantes;
            }
        }
    }
}

void ordenCantidadHabitanteMayor(char pais[5][41], int habitantes[5])
{
    int i,j;
    char auxpais[41];
    int auxhabitantes;
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4-i ; j++)
        {
            if (habitantes[j] < habitantes[j+1])
            {
                strcpy(auxpais,pais[j]);
                strcpy(pais[j],pais[j+1]);
                strcpy(pais[j+1],auxpais);
                auxhabitantes=habitantes[j];
                habitantes[j]=habitantes[j+1];
                habitantes[j+1]=auxhabitantes;
            }
        }
    }
}

void impresion(char pais[5][41], int habitantes[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
       printf("%s - %i \n",pais[i],habitantes[i]);
    }
}

int main()
{
    char pais[5][41];
    int habitantes[5];
    cargar(pais,habitantes);
    ordenAlfabetico(pais,habitantes);
    printf("Lista de paises ordenado alfabeticamente\n");
    impresion(pais,habitantes);
    ordenCantidadHabitanteMayor(pais,habitantes);
    printf("Lista de paises ordenado por cantidad de habitantes\n");
    impresion(pais,habitantes);
    return 0;
}
```
## Importante!
 [[Estructura de datos tipo matriz (elementos char)#Problema 3-Ordenar alfabéticamente los nombres.#Explicacion|Order alfabéticamente tipo char]]

```
void ordenAlfabetico(char pais[5][41], int habitantes[5])
{
    int i,j;
    char auxpais[41];
    int auxhabitantes;
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4-i; j++)
        {
            if (strcmp(pais[j],pais[j+1]) > 0)
            {
                strcpy(auxpais,pais[j]);
                strcpy(pais[j],pais[j+1]);
                strcpy(pais[j+1],auxpais);
                auxhabitantes=habitantes[j];
                habitantes[j]=habitantes[j+1];
                habitantes[j+1]=auxhabitantes;
            }
        }
    }
}
```
[[Ordenamiento Burbuja#Problema 2: Ordenar Mayor a menor y Menor a Mayor#Funcion de ordenamiento de mayor a menor|Orden Mayor/Menor tipo int]]
```
void ordenCantidadHabitanteMayor(char pais[5][41], int habitantes[5])
{
    int i,j;
    char auxpais[41];
    int auxhabitantes;
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4-i ; j++)
        {
            if (habitantes[j] < habitantes[j+1])
            {
                strcpy(auxpais,pais[j]);
                strcpy(pais[j],pais[j+1]);
                strcpy(pais[j+1],auxpais);
                auxhabitantes=habitantes[j];
                habitantes[j]=habitantes[j+1];
                habitantes[j+1]=auxhabitantes;
            }
        }
    }
}
```