Las matrices de tipo de dato char se utilizan fundamentalmente para guardar un conjunto de cadenas de caracteres (varios nombres de personas, nombres de artículos etc.)

Cuando trabajamos una matriz de tipo char para almacenar cadenas de caracteres cada fila almacena un string:
![[Pasted image 20230215091247.png]]
Para trabajar una matriz de tipo char se parece a trabajar con un [[Funciones que facilitan el trabajo con cadenas de caracteres (string.h)|vector]] , debemos indicar un solo subíndice y estaremos accediendo a toda la fila.
# Problema 1
Confeccionar un programa que permita ingresar en una matriz de tipo char los nombres de artículos para la venta. Hacer luego una función que imprima los nombres de dichos artículos.
## Codigo
```Ejercicio120.c
#include<stdio.h>
void cargar(char articulos[3][31])
{
    int i;
    for(i=0; i<3; i++)
    {
        printf("Ingrese el nombre del articulo:");
        fgets(articulos[i],31,stdin);
    }
}
void imprimir(char articulos[3][31])
{
    int i;
    printf("Listado completo de articulos\n");
    for(i=0; i<3; i++)
    {
        printf("%s\n",articulos[i]);
    }
}

int main()
{
    char articulos[3][31];
    cargar(articulos);
    imprimir(articulos);
    return 0;
}
```
En la función main definimos una matriz de 3 filas y 31 columnas de tipo char. Con esta definición podemos almacenar tres nombres de artículos de hasta 30 caracteres de información cada uno, recordemos que se requiere el '\0' (terminador de cadena) para cada string:

```
int main()
{
    char articulos[3][31];
    cargar(articulos);
    imprimir(articulos);
    getch();
    return 0;
}
```

En la función de carga disponemos un solo for ya que vamos a cargar una fila completa cada vuelta del for, debemos indicar un solo subíndice a la matriz que representa la fila que estamos cargando:

```
void cargar(char articulos[3][31])
{
    int f;
    for(f=0;f<3;f++)
    {
        printf("Ingrese el nombre del articulo:");
        gets(articulos[f]);
    }
}
```

De forma similar en la impresión nuevamente indicamos solo un subíndice:

```
void imprimir(char articulos[3][31])
{
    int f;
    printf("Listado completo de articulos\n");
    for(f=0;f<3;f++)
    {
        printf("%s\n",articulos[f]);
    }
}
```
# Problema 2-Ingresar otro nombre y verificar si se encuentra almacenado en la matriz.
Confeccionar un programa que permita :  
1-Almacenar en una matriz los datos de 5 personas.  
2-Imprimir los nombres.  
3-Ingresar otro nombre y verificar si se encuentra almacenado en la matriz.
## Codigo
```Ejercicio121.c
#include<stdio.h>
#include<string.h>

void cargar(char nombres[5][31])
{
    int i;
    for(i=0;i<5;i++)
    {
        printf("Ingrese los nombres:");
        fgets(nombres[i],31,stdin);
    }
}
void imprimir(char nombres[5][31])
{
    int i;
    printf("Lista de nombres ingresados:\n");
    for(i=0;i<5;i++)
    {
        printf("%s\n", nombres[i]);
    }
}
void consulta(char nombres[5][31])
{
    int i;
    char nom[31];
    int existe=0;
    printf("Ingrese un nombre para buscarlo:");
    fgets(nom,31,stdin);
    for ( i = 0; i < 5; i++)
    {
        if(strcmp(nom,nombres[i])==0)
        {
            existe=1;
        }
    }
    if(existe==1)
    {
        printf("El nombre se encuentra almacenado en la matriz.");
    }
    else
    {
        printf("El nombre no se encuentra almacenado en la matriz.");
    }  
}

int main()
{
    char nombres[5][31];
    cargar(nombres);
    imprimir(nombres);
    consulta(nombres);
    return 0;
}
```
## Explicacion
En la función main definimos la matriz de tipo char que permite almacenar 5 palabras de hasta 31 caracteres de información:

```
int main()
{
    char nombres[5][31];
    cargar(nombres);
    imprimir(nombres);
    consulta(nombres);
    return 0;
}
```

Tanto cuando cargamos como cuando imprimimos los cinco nombres hacemos referencia a un solo subíndice:

```
void cargar(char nombres[5][31])
{
    int f;
    for(f=0;f<5;f++)
    {
        printf("Ingrese el nombre de persona:");
        gets(nombres[f]);
    }
}
```

```
void imprimir(char nombres[5][41])
{
    int f;
    printf("Listado completo de nombres\n");
    for(f=0;f<5;f++)
    {
        printf("%s\n",nombres[f]);
    }
}
```

Lo más interesante de este problema esta en la búsqueda de un nombre dentro de la matriz:  

`void consulta(char nombres[5][41])
`
Definimos un vector de caracteres donde el operador ingresará el nombre a buscar:

    char nom[41];

Como se trata de un vector de caracteres no indicamos un subíndice en la carga por teclado:

    gets(nom);

Disponemos un for para controlar cada una de las filas de la matriz si coincide con el nombre ingresado previo al for:

```
    for(f=0;f<5;f++)
    {
        if (strcmp(nom,nombres[f])==0)
        {
            existe=1;
        }
    }
```

Es importante recordar que debemos utilizar [[Funciones que facilitan el trabajo con cadenas de caracteres (string.h)#Funcion: strcmp]| funcion strcmp]] para comparar dos cadenas de caracteres (el vector nom y la fila de la matriz)

Utilizamos una variable entera donde almacenamos un cero previo a buscarlo y si encontramos la palabra dentro de la matriz procedemos a cargar en la variable existe un uno.  
Cuando sale del for según el estado de la variable podemos indicar si la palabra se encuentra en la matriz:

```
    if (existe==1)
    {
        printf("El nombre se encuentra almacenado en la matriz.");
    }
    else
    {
        printf("El nombre no se encuentra almacenado en la matriz.");
    }
```
# Problema 3-Ordenar alfabéticamente los nombres.
Confeccionar un programa que permita :  
1-Almacenar en una matriz los datos de 5 personas.  
2-Imprimir los nombres.  
3-Ordenar alfabéticamente los nombres.
## Codigo
```Ejercicio122.c
#include<stdio.h>
#include<string.h>
void cargar(char nombres[5][31])
{
    int i;
    for(i=0;i<5;i++)
    {
        printf("Ingrese los nombres:");
        fgets(nombres[i],31,stdin);
    }
}

void imprimir(char nombres[5][31])
{
    int i;
    printf("Lista de nombres ingresados:\n");
    for(i=0;i<5;i++)
    {
        printf("%s\n", nombres[i]);
    }
}

void ordenar(char nombres[5][31])
{
    int i,j;
    char aux[31];
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4; j++)
        {
            if(strcmp(nombres[j],nombres[j+1]) > 0)//Ordenamiento Burbuja
            {
                strcpy(aux,nombres[j]);
                strcpy(nombres[j],nombres[j+1]);
                strcpy(nombres[j+1],aux);
            }
        }
    }
}

int main()
{
    char nombres[5][31];
    cargar(nombres);
    ordenar(nombres);
    imprimir(nombres);
    return 0;
}
```
## Explicacion
Para ordenar todas las filas de una matriz de caracteres debemos plantear [[Ordenamiento Burbuja|el algoritmo de ordenamiento ]] que vimos anteriormente.  
Como no está definido el operador == utilizamos la función strcmp:

if(strcmp(nombres[j],nombres[j+1]) **>** 0)

**Recordemos que strcmp retorna un valor mayor a cero(>0) si el primer parámetro es mayor alfabéticamente que el segundo.**

Tampoco tenemos definido el operador de asignación con string por lo que utilizamos la [[Funciones que facilitan el trabajo con cadenas de caracteres (string.h)#Funcion: strcpy|función strcpy]] para el proceso de intercambio de nombres en la matriz:

```
strcpy(aux,nombres[f]);
strcpy(nombres[f],nombres[f + 1]);
strcpy(nombres[f + 1],aux);
```
# Problema 4
Confeccionar un programa que permita :  
1-Almacenar en una matriz los datos de 5 personas.  
2-Imprimir el nombre alfabéticamente menor.
## Codigo
```
#include<stdio.h>
#include<string.h>

void cargar(char nombres[5][31])
{
    int i;
    for(i=0;i<5;i++)
    {
        printf("Ingrese los nombres:");
        fgets(nombres[i],31,stdin);
    }
}

void imprimir(char nombres[5][31])
{
    int i;
    printf("Lista de nombres ingresados:\n");
    for(i=0;i<5;i++)
    {
        printf("%s\n", nombres[i]);
    }
}

void ordenarMenor(char nombres[5][31])
{
    int i,j;
    char aux[31];
    for ( i = 0; i < 4; i++)
    {
        for ( j = 0; j < 4; j++)
        {
            if (strcmp(nombres[j],nombres[j+1]) < 0)
            {
                strcpy(aux,nombres[j]);
                strcpy(nombres[j],nombres[j+1]);
                strcpy(nombres[j+1],aux);
            }
        }
    }
}

int main()
{
    char nombres[5][31];
    cargar(nombres);
    ordenarMenor(nombres);
    imprimir(nombres);
    return 0;
}
```
## Explicacion
Para Ordenarlo de menor a mayor hay que  cambiar  el (**<**) `if(strcmp(nombres[j],nombres[j+1]) < 0)`
