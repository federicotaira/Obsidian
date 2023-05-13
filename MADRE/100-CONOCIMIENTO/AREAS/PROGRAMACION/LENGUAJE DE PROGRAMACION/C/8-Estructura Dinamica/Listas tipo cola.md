  
Una lista se comporta como una cola si las inserciones las hacemos al final y las extracciones las hacemos por el frente de la lista. También se las llama listas FIFO (First In First Out - primero en entrar primero en salir)

Confeccionaremos un programa que permita administrar una lista tipo cola. Desarrollaremos las funciones de insertar, extraer, vacia, imprimir y liberar.
# Problema1
## Codigo
```c
//ejercicio164.c
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>

struct nodo {
    int info;
    struct nodo *sig;
};

struct nodo *raiz=NULL;
struct nodo *fondo=NULL;

int vacia()
{
    if (raiz == NULL)
        return 1;
    else
        return 0;
}


void insertar(int x)
{
    struct nodo *nuevo;
    nuevo=malloc(sizeof(struct nodo));
    nuevo->info=x;
    nuevo->sig=NULL;
    if (vacia())
    {
        raiz = nuevo;
        fondo = nuevo;
    }
    else
    {
        fondo->sig = nuevo;
        fondo = nuevo;
    }
}

int extraer()
{
    if (!vacia())
    {
        int informacion = raiz->info;
        struct nodo *bor = raiz;
        if (raiz == fondo)
        {
            raiz = NULL;
            fondo = NULL;
        }
        else
        {
            raiz = raiz->sig;
        }
        free(bor);
        return informacion;
    }
    else
        return -1;
}

void imprimir()
{
    struct nodo *reco = raiz;
    printf("Listado de todos los elementos de la cola.\n");
    while (reco != NULL)
    {
        printf("%i - ", reco->info);
        reco = reco->sig;
    }
    printf("\n");
}


void liberar()
{
    struct nodo *reco = raiz;
    struct nodo *bor;
    while (reco != NULL)
    {
        bor = reco;
        reco = reco->sig;
        free(bor);
    }
}

void main()
{
    insertar(5);
    insertar(10);
    insertar(50);
    imprimir();
    printf("Extraemos uno de la cola: %i\n", extraer());
    imprimir();
    liberar();
    getch();
    return 0;
}
```
## Explicacion
La declaración del nodo es igual a la [[Listas tipo Pila|Pila]] . Luego definimos dos punteros externos llamados raiz y fondo:

```c
struct nodo {
    int info;
    struct nodo *sig;
};

struct nodo *raiz=NULL;
struct nodo *fondo=NULL;
```
struct:
**raíz** = apunta al **principio** de la lista 
**fondo** =apunta al  **final** de la lista.

Utilizar dos punteros tiene como ventaja que cada vez que tengamos que insertar un nodo al final de la lista no tengamos que recorrerla. Por supuesto que es perfectamente válido implementar una cola con un único puntero externo a la lista.

La [[Listas tipo Pila#Verificacion: Funcion Vacia()|función vacía]] retorna:
- 1 si la lista no tiene nodos 
-  0 en caso contrario:

```c
int vacia()
{
    if (raiz == NULL)
        return 1;
    else
        return 0;
}
```

En la [[Listas tipo Pila#Funcion insertar()|inserción]] luego de crear el nodo tenemos dos posibilidades: que la cola esté vacía, en cuyo caso los dos punteros externos a la lista deben apuntar al nodo creado, o que haya nodos en la lista.

```c
void insertar(int x)
{
    struct nodo *nuevo;
    nuevo=malloc(sizeof(struct nodo));
    nuevo->info=x;
    nuevo->sig=NULL;
    if (vacia())
    {
        raiz = nuevo;
        fondo = nuevo;
    }
    else
    {
        fondo->sig = nuevo;
        fondo = nuevo;
    }
}
```

Recordemos que definimos un puntero llamado nuevo, luego creamos el nodo con la [[Asignación dinámica de memoria (malloc y free)#Funcion malloc|función malloc]] y cargamos los dos atributos, el de información con lo que llega en el parámetro y el puntero con NULL ya que se insertará al final de la lista, es decir no hay otro después de este.

Si la lista está vacía:
![[Pasted image 20230221153313.png]]
En caso de no estar vacía:
![[Pasted image 20230221153328.png]]
Debemos enlazar el puntero sig del último nodo con el nodo recién creado:
```c
fondo->sig = nuevo;
```
![[Pasted image 20230221153403.png]]
Y por último el puntero externo fondo debe apuntar al nodo apuntado por nuevo:
```c
fondo = nuevo;
```
![[Pasted image 20230221153430.png]]
Con esto ya tenemos correctamente enlazados los nodos en la lista tipo cola. Recordar que el puntero nuevo desaparece cuando se sale del método insertar, pero el nodo creado no se pierde porque queda enlazado en la lista.

El funcionamiento del método [[Listas tipo Pila#Funcion extraer()|extraer()]] es similar al de la pila:
```c
int extraer()
{
    if (!vacia())
    {
        int informacion = raiz->info;
        struct nodo *bor = raiz;
        if (raiz == fondo)
        {
            raiz = NULL;
            fondo = NULL;
        }
        else
        {
            raiz = raiz->sig;
        }
        free(bor);
        return informacion;
    }
    else
        return -1;
}
```
Si la lista no está vacía guardamos en una variable local la información del primer nodo (el operador lógico **!** invierte el valor devuelto por la función vacía, si retorna un **1** luego el operador **!** lo convierte a **0** y viceversa y retorna un **1**):

```c
int informacion = raiz->info;
```
Definimos un puntero y lo inicializamos con el primero de la lista:
```c
struct nodo *bor = raiz;
```
Para saber si hay un solo nodo verificamos si los dos punteros raiz y fondo apuntan a la misma dirección de memoria:
```c
if (raiz == fondo)
{
```
![[Pasted image 20230221153801.png]]
Luego hacemos :
```c
raiz = NULL;
fondo == NULL;
```
![[Pasted image 20230221153838.png]]
En caso de haber 2 o mas nodos debemos avanzar el puntero raiz al siguiente nodo:
![[Pasted image 20230221153900.png]]
```c
raiz = raiz->sig;
```
![[Pasted image 20230221153959.png]]
Ya tenemos la lista correctamente enlazada(raiz apunta al primer nodo y fondo continua apuntando al ultimo nodo).

Finalmente eliminamos el nodo y retornamos la informacio:
```c
free(bor);
return informacion;
```

Si la lista tipo cola esta vacia retornamos un -1 (que representa la cola esta vacia, no debermos insertar este valor -1 en la lista).

La [[Listas tipo Pila#Funcion Liberar()|funcion liberar]] es necesaria llamarla cuando no necesite nuestro programa trabajar mas con la lista y tiene por objetivo liberar la ram de todos los nodos actuales que tiene la lista tipo cola:
```c
void liberar()
{
    struct nodo *reco = raiz;
    struct nodo *bor;
    while (reco != NULL)
    {
        bor = reco;
        reco = reco->sig;
        free(bor);
    }
}
```
En la funcion main llamamos a las distintas funciones que hemos codificado en un orden logico:
```c
void main()
{
    insertar(5);
    insertar(10);
    insertar(50);
    imprimir();
    printf("Extraemos uno de la cola: %i\n", extraer());
    imprimir();
    liberar();
    getch();
    return 0;
}
```