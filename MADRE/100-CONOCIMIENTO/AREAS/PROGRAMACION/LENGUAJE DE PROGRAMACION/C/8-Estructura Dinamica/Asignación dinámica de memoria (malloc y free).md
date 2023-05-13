Una característica esencial del lenguaje C es la capacidad de requerir bloques de memoria variable durante la ejecución del programa.

Hasta ahora hemos visto que se reserva espacio para variables en el momento que las definimos:

int x;       // reservamos espacio para almacenar un entero
float z;     //reservamos espacio para almacenar un valor real
char c;      //reservamos espacio para almacenar un caracter
int vec[10]; //reservamos espacio para almacenar 10 enteros
int *pe;     //reservamos espacio para almacenar un puntero

El lenguaje C nos permite en tiempo de ejecución solicitar espacio mediante la [[Asignación dinámica de memoria (malloc y free)#Funcion malloc|función malloc]] (memory  allocate = Asignar memoria) y luego de usarla en forma obligada debemos devolverla llamando a la [[Asignación dinámica de memoria (malloc y free)#Funcion free|función free]] .

Estas dos funciones se encuentran en el archivo de inclusión:

**#include<stdlib.h>**

# Problema 1:
Ingresar por teclado un entero que represente la cantidad de elementos que debe crearse un vector. 
Crear el vector en forma dinámica, cargar e imprimir sus datos.
Hacer todo en la main.
## Codigo
```c
//Ejercicio157.c
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>

int main()
{
    int *pe;
    int tam;
    int i;
    printf("Cuantos elementos tendra el vector:");
    scanf("%i",&tam);
    pe = malloc(tam*sizeof(int));
    for(i=0; i<tam; i++)
    {
        printf("Ingrese elementos:");
        scanf("%i",&pe[i]);
    }
    
    printf("Contenido del vector dinamico:\n");
    for(i=0; i<tam; i++)
    {
        printf("%i\n", pe[i]);
    }
    free(pe);
    return 0;
}
```
## Explicacion
Para trabajar con la memoria dinámica en el lenguaje C es obligatorio trabajar con punteros.  

Lo primero que hacemos es definir un puntero a entero:
```c
int *pe;
```
Solicitamos al operador de nuestro programa que ingrese un entero que representa la cantidad de elementos que tendrá el vector:
```c
    printf("Cuantos elementos tendra el vector:");
    scanf("%i",&tam);
```
No esta permitido indicar en el subíndice de un vector una variable:

`~~int vec[cant];~~`
### Funcion malloc
Pero este problema lo resolvemos solicitando espacio mediante la función **malloc**, debemos indicar el número de bytes a reservar:
```c
pe=malloc(tam*sizeof(int));
```
El operador **sizeof** cuando le pasamos como parámetro un tipo de dato lo que nos devuelve es la cantidad de bytes que se requieren para almacenar dicho tipo de dato. Si compilamos nuestro programa en un compilador de 32bits luego un int requiere 4 bytes. Finalmente multiplicamos lo que retorna sizeof por el contenido de la variable tam, cuyo resultado es la cantidad de bytes que se necesitan reservar para un vector de "tam" elementos.

Una vez que hemos reservado espacio trabajamos como ya sabemos con el vector para cargar, recorrer e imprimir sus datos:
```c
    for(f=0;f<tam;f++)
    {
        printf("Ingrese elemento:");
        scanf("%i",&pe[f]);
    }
    printf("Contenido del vector dinamico:");
    for(f=0;f<tam;f++)
    {
        printf("%i ",pe[f]);
    }
```
### Funcion free
Finalmente una vez que no necesitamos más dicho vector procedemos a liberar el espacio llamando a la **función free** y pasando el nombre del puntero:
```c
    free(pe);
```

El empleo de la memoria dinámica nos permite acceder a bloques de memoria que no se utilizarían si no es a través de las funciones malloc y free.

# Problema 2:
Se tiene la siguiente declaración de registro:

```c
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; 
```

Definir un puntero de tipo producto y luego mediante la función malloc crear un registro en la pila dinámica.
Cargar el registro, imprimirlo y finalmente liberar el espacio reservado mediante la función free.

## Codigo
```c
//Ejercicio158.c
#include<stdio.h>
#include<conio.h>
#include<string.h>
#include<stdlib.h>

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
};


int main()
{
    struct producto *prod;
    
    prod=malloc(sizeof(struct producto));
    
    prod->codigo=1;
    strcpy(prod->descripcion,"papas");
    prod->precio=10.50;
    
    printf("Codigo del articulo:%i\n",prod->codigo);
    printf("Descripcion:%s\n",prod->descripcion);
    printf("Precio:%0.2f",prod->precio);
    
    free(prod);
    
    getch();
    return 0;
}
```
## Explicacion
En este problema reservamos espacio para un registro en la pila dinámica, trabajamos con el mismo y por último liberamos el espacio reservado.

Lo primero es definir un puntero de tipo struct producto:
```c
 struct producto *prod;
```
Seguidamente obtenemos un bloque de memoria, para esto utilizamos la función malloc y mediante el operador sizeof calculamos cuantos bytes se requieren para almacenar un registro de tipo struct producto:
```c
prod=malloc(sizeof(struct producto));

```
Una vez reservado el espacio utilizamos el registro creado para cargar sus datos, como se trata de un puntero debemos utlizar el operador -> para acceder a sus campos:
```c
    prod->codigo=1;
    strcpy(prod->descripcion,"papas");
    prod->precio=10.50;
```
Luego de cargar los campos los mostramos por pantalla:
```c
   printf("Codigo del articulo:%i\n",prod->codigo);
    printf("Descripcion:%s\n",prod->descripcion);
    printf("Precio:%0.2f",prod->precio);
```
Cuando no necesitamos más el registro que creamos procedemos a liberar el espacio reservado mediante la llamada a la función free:
```c
free(prod);
```
Por ejemplo si seleccionamos una función del código fuente del programa VLC media player(que se encuentra codificado en gran parte en C) veremos que se utilizan estas funciones para administrar la memoria dinámica:
![[Pasted image 20230220200344.png]]
# Problema 3:
Pedir ingresar por teclado cuantas letras tiene una palabra. Seguidamente crear un vector en forma dinámica que reserve el espacio mínimo para ingresar dicha palabra.  
Cargar por teclado la palabra, mostrarla y finalmente liberar el espacio requerido
```c
//ejercicio159.c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<conio.h>

int main()
{
    char *palabra;
    int cant;
    printf("Cuantas letras tiene la palabra que ingresara:");
    scanf("%i",&cant);
    fflush(stdin);
    palabra=malloc(cant*sizeof(char)+1);
    printf("Ingrese ahora la palabra:");
    gets(palabra);
    printf("La palabra ingresada es:%s",palabra);
    free(palabra);
    getch();
    return 0;
}
```
