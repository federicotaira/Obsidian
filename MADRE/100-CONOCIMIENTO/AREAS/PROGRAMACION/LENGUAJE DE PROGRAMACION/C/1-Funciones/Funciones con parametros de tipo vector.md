Vimos en conceptos anteriores que un vector en el lenguaje C es una estructura de datos que permite almacenar un conjunto de datos del mismo tipo. Con un unico nombre se define el vector y por medio de un subindice hacemos referencia a cada elemento del mismo componente.

**Importante**
Lo mass importante de un vector que cuando se passa como parametro a una funcion no se hace una copia como sucede con [[Funciones con parámetros de tipo int,float y char| los tipos de datos char, int y float]] sino que se pasa la direccion de memoria donde se almacena el vector original.
Esta forma de pasaje de datos de tipo vector hace que si modificamos el parametro dentro de la funcion lo que sucede es que se modifica la variable definida en la funcion main o donde se haya definido

# Problema
Confeccionar un programa que defina dos funciones, una que permita cargar un vector de 5 elementos enteros y otra que muestre un vector de 5 elementos enteros.  
En la función main definir una variable de tipo vector y seguidamente llamar a las dos funciones.
## Codigo
```Ejercicio101.c
#include<stdio.h>
#include<conio.h>

void cargar(int vec[5])
{
    int x;
    for(x=0;x<5;x++)
    {
        printf("Ingrese elemento:");
        scanf("%i",&vec[x]);
    }
}

void imprimir(int vec[5])
{
    int x;
    printf("Contenido completo del vector:");
    for(x=0;x<5;x++)
    {
        printf("%i ",vec[x]);
    }
}


int main()
{
    int vector[5];
    cargar(vector);
    imprimir(vector);
    getch();
    return 0;
}
```
La sintaxis para definir un parámetro de tipo vector es similar a cuando definimos un vector indicando entre corchetes la cantidad de elementos del mismo:

void cargar(int vec[5])

En la main definimos un vector de 5 elementos y se lo pasamos a la función cargar:

    int vector[5];
    cargar(vector)

Debe quedar claro que cuando pasamos la variable vector el parámetro vec no recibe una copia de vector sino que guarda la referencia de memoria de la variable vector (cuando cargamos enteros en el parámetro vec estamos cargando la variable vector definida en la main):
![[Pasted image 20230214104522.png]]
El algoritmo completo de la función cargar:

void cargar(int vec[5])
{
    int x;
    for(x=0;x<5;x++)
    {
        printf("Ingrese elemento:");
        scanf("%i",&vec[x]);
    }
}

La función imprimir recibe el vector (si bien este algoritmo no lo modifica en el caso que lo haga estará modificando la variable definida en la función main):

void imprimir(int vec[5])
{
    int x;
    printf("Contenido completo del vector:");
    for(x=0;x<5;x++)
    {
        printf("%i ",vec[x]);
    }
}

La función main define un vector y llama primero a la función de cargar el vector y seguidamente la función que lo imprime:

int main()
{
    int vector[5];
    cargar(vector);
    imprimir(vector);
    getch();
    return 0;
}

# Problema 2
Guardar los datos de 6 sueldos de empleados en un vector de tipo float. Confeccionar las siguientes funciones:  
1-Carga de sueldos.  
2-Impresión de los sueldos.  
3-Gasto total de la empresa en sueldos.
## Codigo
```Ejercicio102.c
#include<stdio.h>
#include<conio.h>

void cargar(float sueldos[6])
{
    int x;
    for(x=0;x<6;x++)
    {
        printf("Ingrese el sueldo:");
        scanf("%f",&sueldos[x]);
    }
}

void imprimir(float sueldos[6])
{
    int x;
    printf("Listado completo de sueldos\n");
    for(x=0;x<6;x++)
    {
        printf("%0.2f\n",sueldos[x]);
    }
}

void calcularGastos(float sueldos[6])
{
    float total=0;
    int x;
    for(x=0;x<6;x++)
    {
        total=total+sueldos[x];
    }
    printf("Gasto total en sueldos de la empresa:%0.2f",total);
}


int main()
{
    float sueldos[6];
    cargar(sueldos);
    imprimir(sueldos);
    calcularGastos(sueldos);
    getch();
    return 0;
}
```
En la función main definimos un vector de 6 elementos de tipo float. Este vector se lo pasamos sucesivamente a las distintas funciones para que lo cargue, imprima y sume:

int main()
{
    float sueldos[6];
    cargar(sueldos);
    imprimir(sueldos);
    calcularGastos(sueldos);
    getch();
    return 0;
}

La función que carga el vector de sueldos tiene un parámetro de tipo float de 6 elementos que se llama sueldos, no hay problemas si el parámetro tiene otro nombre:

void cargar(float sueldos[6])
{
    int x;
    for(x=0;x<6;x++)
    {
        printf("Ingrese el sueldo:");
        scanf("%f",&sueldos[x]);
    }
}

Luego tenemos la función que imprime todas sus componentes:

void imprimir(float sueldos[6])
{
    int x;
    printf("Listado completo de sueldos\n");
    for(x=0;x<6;x++)
    {
        printf("%0.2f\n",sueldos[x]);
    }
}

Finalmente tenemos otra función que recibe el vector y procede a sumar todas sus componentes y mostrar dicho valor:

void calcularGastos(float sueldos[6])
{
    float total=0;
    int x;
    for(x=0;x<6;x++)
    {
        total=total+sueldos[x];
    }
    printf("Gasto total en sueldos de la empresa:%0.2f",total);
}
# Problema 3
Definir tres vectores de tipo entero. Realizar la carga de los dos primeros por teclado. Definir una única función que realice la carga de un vector y llamar a dicha función dos veces pasando el primer y segundo vector definido.  
Plantear otra función que reciba tres vectores y proceda a sumar elemento a elementos los dos primeros vectores y se carguen en el tercer vector.  
Imprimir los tres vectores.
## Codigo
```Ejercicio103.c
#include<stdio.h>
#include<conio.h>

void cargar(int vec[5])
{
    int x;
    printf("Carga de un vector.\n");
    for(x=0;x<5;x++)
    {
        printf("Cargar elemento:");
        scanf("%i",&vec[x]);
    }
}

void generarVector(int vec1[5],int vec2[5],int vecsuma[5])
{
    int x;
    for(x=0;x<5;x++)
    {
        vecsuma[x]=vec1[x]+vec2[x];
    }
}

void imprimir(int vec[5])
{
    int x;
    printf("Impresion del vector:");
    for(x=0;x<5;x++)
    {
        printf("%i ",vec[x]);
    }
    printf("\n");
}


int main()
{
    int vector1[5];
    int vector2[5];
    int vecsuma[5];
    cargar(vector1);
    cargar(vector2);
    generarVector(vector1,vector2,vecsuma);
    imprimir(vector1);
    imprimir(vector2);
    imprimir(vecsuma);
    getch();
    return 0;
}
```
Es interesante ver en este problema que la función cargar la podemos reutilizar para cargar distintos vectores de 5 enteros, desde la función main llamamos dos veces a dicha función pasando primero a vector1 y luego a vector2:

```
int main()
{
    int vector1[5];
    int vector2[5];
    int vecsuma[5];
    cargar(vector1);
    cargar(vector2);
}
```

Acá queda claro que el nombre del parámetro de la función puede tener un nombre distinto a la variable que le pasamos desde la main:

```
void cargar(int vec[5])
{
    int x;
    printf("Carga de un vector.\n");
    for(x=0;x<5;x++)
    {
        printf("Cargar elemento:");
        scanf("%i",&vec[x]);
    }
}
```

Otro de los puntos que debe resolver el problema es la generación de un tercer vector a partir de la suma de las componentes de la misma posición de los dos primeros vectores:

```
void generarVector(int vec1[5],int vec2[5],int vecsuma[5])
{
    int x;
    for(x=0;x<5;x++)
    {
        vecsuma[x]=vec1[x]+vec2[x];
    }
}
```

Nuevamente la función imprimir nos sirva para mostrar cualquier vector de 5 elementos de tipo entero:

```
void imprimir(int vec[5])
{
    int x;
    printf("Impresion del vector:");
    for(x=0;x<5;x++)
    {
        printf("%i ",vec[x]);
    }
    printf("\n");
}
```

Desde la main llamamos a esta función tres veces pasando uno a uno cada vector que queremos imprimir:

```
int main()
{
    int vector1[5];
    int vector2[5];
    int vecsuma[5];
    cargar(vector1);
    cargar(vector2);
    generarVector(vector1,vector2,vecsuma);
    imprimir(vector1);
    imprimir(vector2);
    imprimir(vecsuma);
    return 0;
}
```
# Problema 4
Confeccionar dos funciones:  
1-Permita ingresar por teclado una palabra en un vector de caracteres que llega como parámetro.  
2-Retornar la cantidad de vocales que tiene la palabra.
## Codigo
```Ejercicio104.c
#include<stdio.h>
#include<conio.h>

void cargar(char palabra[40])
{
    printf("Ingrese una palabra:");
    gets(palabra);
}

int cantidadDeVocales(char palabra[40])
{
    int cant=0;
    int x=0;
    while(palabra[x]!='\0')
    {
        if (palabra[x]=='a' || palabra[x]=='e' || palabra[x]=='i' || palabra[x]=='o' || palabra[x]=='u' ||
            palabra[x]=='A' || palabra[x]=='E' || palabra[x]=='I' || palabra[x]=='O' || palabra[x]=='U')
        {
            cant++;
        }
        x++;
    }
    return cant;
}


int main()
{
    char pal[40];
    cargar(pal);
    printf("La cantidad de vocales que tiene la palabra: %s es de %i",pal, cantidadDeVocales(pal));
    getch();
    return 0;
}
```
Los parámetros de tipo vector con elementos de tipo char funcionan igual que los vectores con elementos int o float. Si modificamos el parámetro estamos modificando la variable que le pasamos desde la main.

La función que carga la palabra recibe como parámetro el vector de caracteres y lo modifica:

```
void cargar(char palabra[40])
{
    printf("Ingrese una palabra:");
    gets(palabra);
}
```

La segunda función recibe un vector de caracteres con información y procede a analizar cada caracter contando cada vez que hay una vocal. La función finalmente mediante un return devuelve la cantidad de vocales de la palabra:

```
int cantidadDeVocales(char palabra[40])
{
    int cant=0;
    int x=0;
    while(palabra[x]!='\0')
    {
        if (palabra[x]=='a' || palabra[x]=='e' || palabra[x]=='i' || palabra[x]=='o' || palabra[x]=='u' ||
            palabra[x]=='A' || palabra[x]=='E' || palabra[x]=='I' || palabra[x]=='O' || palabra[x]=='U')
        {
            cant++;
        }
        x++;
    }
    return cant;
}
```

Desde la función main llamamos a las dos funciones:

```
int main()
{
    char pal[40];
    cargar(pal);
    printf("La cantidad de vocales que tiene la palabra: %s es de %i",pal, cantidadDeVocales(pal));
    getch();
    return 0;
}
```