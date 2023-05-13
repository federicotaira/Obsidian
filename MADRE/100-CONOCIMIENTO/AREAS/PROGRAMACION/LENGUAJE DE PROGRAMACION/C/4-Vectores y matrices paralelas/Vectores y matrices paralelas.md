Este concepto se da cuando hay una relación entre las componentes de igual subíndice (misma posición) de un vector y otro, o de un vector con una matriz de caracteres.
![[Pasted image 20230215115234.png]]
Si tenemos una matriz de caracteres de 5 filas y 30 columnas en la que se almacenan los nombres de personas y un vector de 5 enteros en la que se almacenan las edades de dichas personas, decimos que la matriz nombres es paralelo al vector edades si en la componente 0 del vector y la fila 0 de la matriz se almacena información relacionada a una persona (Juan - 12 años)

Es decir hay una relación entre cada componente del vector y la fila de la matriz.

Esta relación la conoce únicamente el programador y se hace para facilitar el desarrollo de algoritmos que procesen los datos almacenados en las estructuras de datos.

# Problema 1
Desarrollar un programa que permita:
- cargar 5 nombres de personas y sus edades respectivas.
- Luego de realizar la carga por teclado de todos los datos [[Algoritmo de busqueda- Vectores (mayor y menor elemento)#Problema 1- Imprimir elemento mayor y su posicion.|imprimir los nombres de las personas mayores de edad]] (mayores o iguales a 18 años)
## Codigo
```Ejercicio123.c
#include<stdio.h>
#include<string.h>

void cargar(char nombres[5][31], int edad[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese nombre:");
        fgets(nombres[i],31,stdin);
        printf("Edad:");
        scanf("%i",&edad[i]);
        fflush(stdin);
    }    
}

void imprimirMayorEdad(char nombres[5][31], int edad[5])
{
    int i;
    printf("Personas mayores de edad. \n");
    for ( i = 0; i < 5; i++)
    {
        if (edad[i]>=18)
        {
            printf("%s\n",nombres[i]);
        }
    }
}

int main()
{
    int edad[5];
    char nombres[5][31];
    cargar(nombres,edad);
    imprimirMayorEdad(nombres,edad);
    return 0;
}
```
En la main definimos la matriz y el vector paralelo (los dos tienen 5 elementos):

int main()
{
    int edad[5];
    char nombres[5][31];
 
Como debemos cargar 5 nombres, luego debemos indicar en el primer subíndice un 5 y un 31 para indicar la cantidad de caracteres que se reserva por nombre ocupando uno de ellos el terminador de cadena.

Mediante un for procedemos a la carga de cada fila de la matriz y cada elemento del vector edades:

```
void cargar(char nombres[5][31], int edad[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese nombre:");
        fgets(nombres[i],31,stdin);
        printf("Edad:");
        scanf("%i",&edad[i]);
        fflush(stdin);
    }    
}
```
Luego de la carga de la edad de la persona se procede a cargar otro nombre por lo que debemos llamar a la `función fflush (esto debido a que en el buffer de teclado queda un enter después de cargar un entero)`

Para imprimir los nombres de las personas mayores de edad verificamos cada componente del vector de edades, en caso que sea igual o igual a 18 procedemos a mostrar la fila de la misma posición de la matriz:

```
void imprimirMayorEdad(char nombres[5][31], int edad[5])
{
    int i;
    printf("Personas mayores de edad. \n");
    for ( i = 0; i < 5; i++)
    {
        if (edad[i]>=18)
        {
            printf("%s\n",nombres[i]);
        }
    }
}
```
# Problema 2
-. Ingresar el nombre de 5 productos en una matriz de caracteres y sus respectivos precios en un vector paralelo de tipo float.  
-. Mostrar cuantos productos tienen un precio mayor al primer producto ingresado (se debe contar)
## Codigo
```Ejercicio124.c
#include<stdio.h>
#include<string.h>

void cargar(char nombres[5][41],float precio[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Ingrese el nombre del producto:");
        fgets(nombres[i],41,stdin);
        printf("Asignar un precio:");
        scanf("%f",&precio[i]);
        fflush(stdin);
    }
    printf("__________________________\n");  
}
void precioMayorPrimero(float precio[5])
{
    int i;
    int count=0;
    for ( i = 0; i < 5; i++)
    {
        if(precio[i]<precio[0])
        {
            count++;
        }
    }    
    printf("La cantidad de productos con un precio mayor al primero ingresado son:%i\n",count);
    printf("__________________________\n");
}

void imprimir(char nombres[5][41],float precio[5])
{
    int i;
    for ( i = 0; i < 5; i++)
    {
        printf("Producto: %s - Precio: %0.2f\n",nombres[i],precio[i]);
    }
    printf("__________________________\n");
}

int main()

{
    char nombres[5][41];
    float precio[5];
    cargar(nombres,precio);
    imprimir(nombres,precio);
    precioMayorPrimero(precio);
    return 0;
    
}
```
# Problema 3
En un curso de 4 alumnos se registraron las notas de sus exámenes y se deben procesar de acuerdo a lo siguiente:  
1. Ingresar Nombre y Nota de cada alumno (almacenar los datos en estructuras paralelas)  
2. Realizar un listado que muestre los nombres, notas y condición del alumno:
	1.  En la condición colocar:
		1. "Muy Bueno" si la nota es mayor o igual a 8.
		2. "Bueno" si la nota está entre 4 y 7.
		3. "Insuficiente" si la nota es inferior a 4.  
3.  Imprimir cuantos alumnos tienen nota “Muy Bueno”.