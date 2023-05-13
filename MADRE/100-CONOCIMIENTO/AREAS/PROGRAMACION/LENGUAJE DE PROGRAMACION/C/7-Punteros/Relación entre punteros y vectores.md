Es importante entender que un [[Vectores y matrices paralelas|vector]] es un [[Variables de tipo puntero|puntero]] que contiene la dirección de la primera componente.

```
int vec[5]={10,20,30,40,50};

int *pe;
```

Si luego igualamos pe con vec:

	pe=vec;

Luego tenemos en memoria:]
![[Pasted image 20230220150813.png]]
La variable pe almacena la misma dirección de memoria que la variable vec.

Ahora podemos trabajar perfectamente con la variable pe para acceder a las componentes del vector:
```
#include<stdio.h>

int main()
{
    int vec[5]={10,20,30,40,50};
    int *pe;
    int f;
    pe=vec;
    for(f=0;f<5;f++)
    {
        printf("%i ",pe[f]);
    }
    getch();
    return 0;
}
```
Como vemos después de la asignación de pe=vec podemos acceder a las componentes del vector con cualquiera de las dos variables.

Es decir desde que presentamos el concepto de vectores estábamos trabajando con punteros.

Ahora podemos entender porque cuando pasamos un vector o una matriz a una función se modifica la variable que le pasamos desde la función main, esto debido a que le pasamos la dirección donde se almacenan las componentes del vector o la matriz.

Podemos inclusive utilizar la sintaxis de punteros en los parámetros de una función que recibe un vector.

# Problema 1
Confeccionar un programa que permita cargar e imprimir un vector de 5 elementos de tipo float. Utilizar la sintaxis de punteros en los parámetros de las funciones.
## Codigo
```ejercicio153.c
#include<stdio.h>

#define TAMANO 5

void cargar(float *p)
{
    int i;
    for ( i = 0; i < TAMANO; i++)
    {
        printf("Ingrese los valores:");
        scanf("%f", &p[i]);
    }
}

void imprimir(float *p)
{
    int i;
    for ( i = 0; i < TAMANO; i++)
    {
        printf("%0.2f\n", p[i]);
    }
}

int main()
{
    float vec[TAMANO];
    cargar(vec);
    imprimir(vec);
    return 0;
}
```
## Explicacion
Anteriormente para indicar la sintaxis de un parámetro de tipo vector era:

	void cargar(float p[TAMANO]) 

Ahora que sabemos que el nombre del vector es un puntero podemos utilizar la sintaxis:

	void cargar(float *p)

Cuando se compila nuestro programa el funcionamiento es exactamente igual.