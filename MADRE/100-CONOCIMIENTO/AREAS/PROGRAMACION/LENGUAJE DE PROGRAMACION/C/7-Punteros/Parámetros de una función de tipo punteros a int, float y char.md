Hemos visto que cuando llamamos a una [[Funciones con parámetros de tipo int,float y char|función]] y le pasamos un tipo de dato char, int o float lo que sucede es que se hace una copia de dicha variable en el parámetro de la función.

Ahora veremos el primer uso real de [[Variables de tipo puntero|punteros]] en el lenguaje C. **Cuando uno quiere modificar una variable char, int o float en una función lo que hacemos es pasar la dirección de la variable y que lo reciba un puntero.**
# Problema 1: Función para cargar dos enteros a través de la dirección de dos variables enteras
Confeccionar una función que reciba como parámetros las direcciones de dos variables enteras y le cargue a lo apuntado por dichas variables dos enteros.
## Codigo
```Ejercicio146.c
#include<stdio.h>
#include<conio.h>

void cargar(int *pe1,int *pe2)
{
    *pe1=100;
    *pe2=200;
}

int main()
{
    int x1,x2;
    cargar(&x1,&x2);
    printf("%i   %i",x1,x2);
    getch();
    return 0;
}
```
## Explicacion
En este problema necesitamos desarrollar una función que cargue dos enteros. Como sabemos si le pasamos las variables definidas en la main lo que sucede es que se hace una copia, entonces lo que le pasamos a la función son las dos direcciones de dichas variables enteras (**antecedemos el caracter & al nombre de la variable entera para indicar que le pasamos su dirección**):

int main()
{
    int x1,x2;
    cargar(**&** x1, **&** x2);

Como a la función le pasamos la dirección de variables enteras los parámetros deben ser de tipo puntero:


void cargar(int ==*pe1== ,int ==*pe2== )


Dentro de la función utilizamos los punteros para acceder a los contenidos de las variables pasadas:

{
    ==(*pe1)== =100;
    ==(*pe2)== =200;
}

Cuando le asignamos a lo apuntado por el puntero pe1 el valor 100 en realidad estamos accediendo al espacio reservado a la variable x1:

    *pe1=100;

# Importante

Después de mucho avanzar en este curso del lenguaje C podemos entender completamente porque es la siguiente sintaxis para cargar un entero por teclado:

int x1;
scanf("%i",&x1);

---

**El & significa que le pasamos la dirección de la variable x1 para que la función por medio de un puntero cargue un entero en x1.**
La función recibe una dirección de una variable entera y mediante este puntero modifica lo apuntado:
```
void incrementar(int *pe)
{
    *pe=*pe+1;
}
```
**Es importante decir si queremos utilizar el operador ++ para incrementar en 1 lo apuntado por pe debemos utilizar la sintaxis:**

```
void incrementar(int *pe)
{
    (*pe)++;
}
```

Si hacemos *pe++ estamos modificando la dirección de memoria almacenada en pe con lo que luego de esto pe no apunta a la variable x.

# Problema 2: Función para incrementar en 1 una variable entera a través de su dirección
Elaborar una función que se le pase la dirección de una variable entera e incremente en 1 lo apuntado por dicha variable.
## Codigo 
```Ejercicio147.c
#include<stdio.h>
#include<conio.h>

void incrementar(int *pe)
{
    *pe=*pe+1;
}

int main()
{
    int x=0;
    printf("%i\n",x); // 0
    incrementar(&x);
    printf("%i\n",x); // 1
    incrementar(&x);
    printf("%i\n",x); // 2
    incrementar(&x);
    printf("%i\n",x); // 3
    getch();
    return 0;
}
```
## Explicacion
En este caso implementamos una función que se le pase la dirección de una variable entera y le incremente en 1 su contenido. Cada vez que llamamos la función el contenido de la variable x se incrementa en 1:

```
    int x=0;
    printf("%i\n",x); // 0
    incrementar(&x);
    printf("%i\n",x); // 1
    incrementar(&x);
    printf("%i\n",x); // 2
    incrementar(&x);
    printf("%i\n",x); // 3
```
La función recibe una dirección de una variable entera y mediante este puntero modifica lo apuntado:
```
void incrementar(int *pe)
{
    *pe=*pe+1;
}
```
**Es importante decir si queremos utilizar el operador ++ para incrementar en 1 lo apuntado por pe debemos utilizar la sintaxis:**

```
void incrementar(int *pe)
{
    (*pe)++;
}
```

Si hacemos *pe++ estamos modificando la dirección de memoria almacenada en pe con lo que luego de esto pe no apunta a la variable x.
# Problema 3 Intercambiar el contenido de 2 variables enteras utilizando punteros
Implementar una función que intercambie el contenido de dos variables enteras, utilizar punteros para solucionarlo.
## Codigo
```ejercicio 148.c
#include<stdio.h>

void intercambiar(int *pe1,int *pe2)
{
    int aux = *pe1;
    *pe1 = *pe2;
    *pe2 = aux;
}

int main()
{
    int x1=10;
    int x2=20;
    printf("%i - %i\n",x1, x2);
    intercambiar(&x1,&x2);
    printf("%i - %i\n",x1, x2);
    return 0;
}
```
Si necesitamos modificar el contenido de dos variables enteras en una función le pasamos sus direcciones y procedemos a modificar lo apuntado, como son enteros los que tenemos que intercambiar definimos una variable auxiliar de tipo int:

```
void intercambiar(int *pe1,int *pe2)
{
    int aux=*pe1;
    *pe1=*pe2;
    *pe2=aux;
}
```
Cuando llamamos a la función le pasamos las direcciones de las variables enteras para que puedan moficarlas:

```
int main()
{
    int x1=10;
    int x2=20;
    printf("%i  %i\n",x1,x2);
    intercambiar(&x1,&x2);
    printf("%i  %i\n",x1,x2);
```
# Problema 4 Hallar el mayor y menor
Confeccionar un programa que permita cargar un vector de 5 enteros y obtenga el mayor y el menor.  
Implementar dos funciones:  
1-Carga del vector  
2-Otra función que reciba el vector y retorne el mayor y menor elemento del vector por medio de dos parámetros de tipo puntero:
```
void mayorMenor(int vec[TAMANO],int *pmayor,int *pmenor)
```
## Codigo
```ejercicio149.c
#include<stdio.h>

#define TAMANO 5

void cargar(int vec[TAMANO])
{
    int i;
    for ( i = 0; i < TAMANO; i++)
    {
        printf("Ingrese el valor:");
        scanf("%i", &vec[i]);
    }
}

void imprimir(int vec[TAMANO])
{
    int i;
    printf("Lista:");
    printf("\n");
    for ( i = 0; i < TAMANO; i++)
    {
        printf("%i\n",vec[i]);
    }
    printf("\n");    
}

void mayorMenor(int vec[TAMANO],int *pmayor, int *pmenor)
{
    int i;
    *pmayor=vec[0];
    *pmenor=vec[0];
    for ( i = 1; i < TAMANO; i++)
    {
        if(vec[i]>*pmayor)
        {
            *pmayor = vec[i];
        }
        else
        {
            if (vec[i]<*pmenor)
            {
                *pmenor = vec[i];
            }
        }
    }
}

int main()
{
    int vec[TAMANO];
    cargar(vec);
    imprimir(vec);
    int may,men;
    mayorMenor(vec, &may, &men);
    printf("Mayor elemento del vector: %i\n", may);
    printf("Menor elemento del vector: %i\n", men);
    return 0;
}
```