Una [[Listas tipo Pila|lista]] se comporta como una pila si las inserciones y extracciones las hacemos por un mismo lado de la lista. También se las llama listas LIFO (Last In First Out - último en entrar primero en salir)

**Importante**: Una pila al ser una lista puede almacenar en el campo de información cualquier tipo de valor (int, char, float, vector de caracteres, un registro, etc.)

Para estudiar el mecanismo de utilización de una pila supondremos que en el campo de información almacena un entero (para una fácil interpretación y codificación)

Inicialmente la PILA está vacía y decimos que el puntero raiz apunta a NULL (Si apunta a NULL decimos que no tiene una dirección de memoria, en realidad este valor NULL es el valor cero):
![[Pasted image 20230221110847.png]]
Insertamos un valor entero en la pila: insertar(10)
![[Pasted image 20230221110904.png]]
Luego de realizar la inserción la lista tipo pila queda de esta manera: un nodo con el valor 10 y raiz apunta a dicho nodo. El puntero del nodo apunta a NULL ya que no hay otro nodo después de este.

Insertamos luego el valor 4: insertar(4)
![[Pasted image 20230221110933.png]]
Ahora el primer nodo de la pila es el que almacena el valor cuatro. raiz apunta a dicho nodo. Recordemos que raiz es el puntero externo a la lista que almacena la dirección del primer nodo.  
El nodo que acabamos de insertar en el campo puntero guarda la dirección del nodo que almacena el valor 10.

Ahora qué sucede si extraemos un nodo de la pila. ¿Cuál se extrae? Como sabemos en una pila se extrae el último en entrar.

Al extraer de la pila tenemos: extraer()
![[Pasted image 20230221111008.png]]
La pila ha quedado con un nodo.  
Hay que tener cuidado que si se extrae un nuevo nodo la pila quedará vacía y no se podrá extraer otros valores (avisar que la pila está vacía)
# Problema 1- Administración de una pila de datos con funciones de inserción, extracción e impresión
Confeccionar una programa que administre una lista tipo pila (se debe poder [[Listas tipo Pila#Funcion insertar()|insertar]] , [[Listas tipo Pila#Funcion extraer()|extraer]] e [[Listas tipo Pila#Funcion Imprimir()|imprimir]] los datos de la pila) 
## Codigo
```c
//ejercicio160.c
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>

struct nodo {

    int info;

    struct nodo *sig;

};

//variable global que apunta al primer nodo de la lista NULL = 0
struct nodo *raiz =NULL;

void insertar(int x)
{
    struct nodo *nuevo;
    nuevo = malloc(sizeof(struct nodo));
    nuevo->info=x;
    if(raiz == NULL)
    {
        raiz = nuevo;
        nuevo->sig = NULL;
    }
    else
    {
        nuevo->sig = raiz;
        raiz = nuevo;
    }
}

void imprimir()
{
    struct nodo *reco = raiz;
    printf("Lista completa.\n");
    while (reco != NULL)
    {
        printf("%i ", reco->info);
        reco = reco->sig;
    }
    printf("\n");
}

int extraer()
{
    if (raiz != NULL)
    {
        int informacion = raiz->info;
        struct nodo *bor = raiz;
        raiz = raiz->sig;
        free(bor);
        return informacion;
    }
    else
    {
        return -1;
    }
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

int main()
{
    insertar(10);
    insertar(4);
    insertar(5);
    imprimir();
    printf("Extraemos de la pila: %i\n", extraer());
    imprimir();
    liberar();
}
```
## Explicacion
Analicemos las distintas partes de este programa:

Declaramos un registro con dos campos. Un entero que es la información que guardará cada nodo y un puntero al siguiente nodo:

```c
struct nodo {
    int info;
    struct nodo *sig;
};
```

Algo nuevo que no habíamos hecho hasta este momento es la definición de una **variable global** en nuestro programa, si bien en programas grandes puede traer inconvenientes las presentamos en este momento para facilitar la comprensión del concepto de listas:

```c
//variable global que apunta al primer nodo de la lista
struct nodo *raiz=NULL;
```

La **variable raiz es un puntero que apunta a registros de tipo nodo**. Dijimos que un [[Variables de tipo puntero|puntero]] es una variable que puede almacenar una dirección de memoria, si guardamos la macro NULL estamos indicando que la variable no almacena en este momento una dirección.

**El valor NULL nos permite identificar si un puntero tiene almacenada una dirección o no.**

La variable raiz al ser una variable global no es necesaria pasarla como parámetro a cada función que la necesite.

Todo programa en C comienza a ejecutarse desde la función main, previo a iniciar la main reserva espacio para todas las variables globales (en nuestro caso raiz) desde la main llamamos a las distintas funciones para administrar la lista:

```c
int main()
{
    insertar(10);
    insertar(40);
    insertar(3);
    imprimir();
    printf("Extraemos de la pila:%i\n",extraer());
    imprimir();
    liberar();
    getch();
    return 0;
}
```
### Funcion insertar()
La función insertar nos **permite agregar un nodo al principio de la lista**:

```c
void insertar(int x)
{
    struct nodo *nuevo;
    nuevo = malloc(sizeof(struct nodo));
    nuevo->info = x;
    if (raiz == NULL)
    {
        raiz = nuevo;
        nuevo->sig = NULL;
    }
    else
    {
        nuevo->sig = raiz;
        raiz = nuevo;
    }
}
```

A la función llega la información a insertar, en este caso en particular es un valor entero.

La creación de un nodo requiere dos pasos:

- **Definición de un puntero o referencia a un tipo de dato modo:**

```c
struct nodo *nuevo;
```

- **Creación del nodo (reserva de espacio en la memoria dinámica):**

```c
nuevo = malloc(sizeof(struct nodo));
```

Cuando se ejecuta la [[Asignación dinámica de memoria (malloc y free)#Funcion malloc|función malloc]] se reserva espacio para el nodo. Realmente se crea el nodo cuando se ejecuta la función malloc.
![[Pasted image 20230221122507.png]]
Paso seguido debemos guardar la información del nodo (el campo info lo accedemos mediante el operador ->):
```c
nuevo->info = x;
```
En el campo info almacenamos lo que llega en el parámetro x. Por ejemplo si llega un 5 el nodo queda:
![[Pasted image 20230221122541.png]]
Por último queda enlazar el nodo que acabamos de crear al principio de la lista.  
Si la lista está vacía debemos guardar en el campo sig del nodo el valor NULL para indicar que no hay otro nodo después de este, y hacer que raiz apunte al nodo creado (sabemos si una lista esta vacía si raiz almacena un NULL, es decir el valor que le almacenamos cuando definimos la variable)  
Tener en cuenta que podemos acceder a raiz sin haberla pasado como parámetro ya que es una variable global y puede ser accedida por cualquier función luego de su definición:
```c
    if (raiz == NULL)
    {
        raiz = nuevo;
        nuevo->sig = NULL;
    }
```
![[Pasted image 20230221122616.png]]
Gráficamente podemos observar que cuando indicamos raiz=nuevo, el puntero raiz guarda la dirección del nodo apuntado por nuevo.  
Tener en cuenta que cuando finaliza la ejecución de la función el puntero "nuevo" desaparece, pero no el nodo creado mediante la función malloc.

En caso que la lista no esté vacía, el puntero sig del nodo que acabamos de crear debe apuntar al que es hasta este momento el primer nodo, es decir al nodo que apunta raiz actualmente.
```c
    else
    {
        nuevo->sig = raiz;
        raiz = nuevo;
    }
```
Como primera actividad cargamos en el puntero sig del nodo apuntado por nuevo la dirección de raiz, y posteriormente raiz apunta al nodo que acabamos de crear, que será ahora el primero de la lista.

Antes de los enlaces tenemos:
![[Pasted image 20230221122718.png]]
Luego de ejecutar la línea:
```c
nuevo->sig = raiz;
```
Ahora tenemos:
![[Pasted image 20230221122750.png]]
Por último asignamos a raiz la dirección que almacena el puntero nuevo.
```c
 raiz = nuevo;
```
La lista queda:
![[Pasted image 20230221122817.png]]
### Funcion extraer()
```c
int extraer()
{
    if (raiz != NULL)
    {
        int informacion = raiz->info;
        struct nodo *bor = raiz;
        raiz = raiz->sig;
        free(bor);
        return informacion;
    }
    else
    {
        return -1;
    }
}
```
El objetivo de la función extraer es [[Funciones con retorno de un struct|retornar]] la información del primer nodo y además borrarlo de la lista.

1. Si la lista no está vacía (es decir raiz tiene un valor distinto a NULL) guardamos en una variable local la información del primer nodo:
```c
  if (raiz != NULL)
    {
        int informacion = raiz->info;
```
2. Creamos un puntero auxiliar y hacemos que apunte al nodo que vamos a borrar:
```c
struct nodo *bor = raiz;
```
3. Avanzamos raiz al segundo nodo de la lista, ya que borraremos el primero (si no hay otro nodo más adelante en raiz se guarda NULL ya que el último nodo de la lista tiene en el puntero sig dicho valor):
```c
raiz = raiz->sig;
```
4. Procedemos a eliminar el primer nodo de la lista llamando a la [[Asignación dinámica de memoria (malloc y free)#Funcion free|función free]] (si no hacemos esto no se genera error en el programa pero dicho espacio de memoria no se podrá asignar cuando hagamos otra llamada a la función malloc):
```c
 free(bor);
```
5. Retornamos la información:
```c
return informacion;
```
6. En caso de estar vacía la pila retornamos el número -1 y lo tomamos como código de error (es decir nunca debemos guardar el entero -1 en la pila, podemos utilizar cualquier otro entero que indique lista vacía)
```c
   else
    {
        return -1;
    }
```
Es muy importante entender gráficamente el manejo de las listas. La interpretación gráfica nos permitirá plantear inicialmente las soluciones para el manejo de listas.
![[Pasted image 20230221123610.png]]
### Funcion Imprimir()
```c
void imprimir()
{
    struct nodo *reco=raiz;
    printf("Lista completa.\n");
    while (reco!=NULL)
    {
        printf("%i ",reco->info);
        reco=reco->sig;
    }
    printf("\n");
}
```
1. Definimos un puntero auxiliar reco y hacemos que apunte al primer nodo de la lista:
```c
struct nodo *reco=raiz;
```
2. Disponemos una estructura repetitiva que se repetirá mientras reco sea distinto a NULL. Dentro de la estructura repetitiva hacemos que reco avance al siguiente nodo:
```c
    while (reco!=NULL)
    {
        printf("%i ",reco->info);
        reco=reco->sig;
    }
```

Es muy **importante** entender la línea:
```c
 reco=reco->sig;
```
Estamos diciendo que reco almacena la dirección que tiene el puntero sig del nodo apuntado actualmente por reco.

Gráficamente:
![[Pasted image 20230221124127.png]]
Al analizarse la condición:
```c
 while (reco!=NULL)
```
Es verdadero ya que reco apunta a un nodo y se vuelve a ejecutar la línea:
```c
 reco=reco->sig;
```
Ahora reco apunta al siguiente nodo:
![[Pasted image 20230221124246.png]]
La condición del while nuevamente se valúa en verdadera y avanza el puntero reco al siguiente nodo:
```c
reco=reco->sig;
```
![[Pasted image 20230221124319.png]]
Ahora sí reco apunta a NULL (tiene almacenado un NULL) y ha llegado el final de la lista (Recordar que el último nodo de la lista tiene almacenado en el puntero sig el valor NULL, con el objetivo de saber que es el último nodo)

### Funcion Liberar()
Tiene por objetivo liberar el espacio ocupado por los nodos de la lista, esta actividad es fundamental:
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
1. Definimos dos punteros auxiliares: reco y bor. A reco lo inicializamos con el primer nodo de la lista (en forma similar a como hicimos el imprimir donde recorremos toda la lista):
```c
struct nodo *reco = raiz;
struct nodo *bor;
```
2.Mediante un [[Estructura repetitiva while|while]] recorreremos toda la lista:
```c
while (reco != NULL)
```
3. Dentro del while inicializamos el puntero bor con la dirección del nodo que apunta reco
```c
 bor = reco;
```
4. Inmediatamente avanzamos reco al siguiente nodo:
```c
reco = reco->sig;
```
5. procedemos a borrar el nodo apuntado por bor:
```c
free(bor);
```

Este while se repite mientras haya nodos en la lista.
# Problema 2 - Administracion pila de datos con funciones adicionales de contar nodos y verificar si está vacía
Agregar al problema anterior otras dos funciones: que retorne la cantidad de nodos y otro que indique si esta vacía (1=vacía y 0=no vacía).
## Codigo
```c
//Ejercicio161.c
#include<stdio.h>
#include<stdlib.h>

struct nodo
{
    int info;
    struct nodo *sig;
};
//variable global que apunta al primer nodo de la lista
struct nodo *raiz=NULL;

void insertar(int x)
{
    struct nodo *nuevo;
    nuevo = malloc(sizeof(struct nodo)); //reserva de espacio en la memoria dinamica
    nuevo->info=x; // en el campo info almacenamos lo que llega en el parametro x
    if (raiz == NULL) //lista esta vacía si raiz almacena un NULL, es decir el valor que le almacenamos cuando definimos la variable
    {
        raiz = nuevo; //el puntero raiz guarda la direccion del nodo apuntado por nuevo
        nuevo->sig = NULL; //el campo sig del nodo el valor NULL para indicar que no hay otro nodo después de este
    }
    else // si no esta vacia
    {
        nuevo->sig = raiz; //el puntero sig del nodo que acabamos de crear debe apuntar al que es hasta este momento el primer nodo
        raiz = nuevo; //es decir al nodo que apunta raiz actualmente
    }
}

void imprimir()
{
    struct nodo *reco = raiz; //Definir puntero auxiliar reco para apuntar al primer nodo de la lista
    printf("Lista completa.\n");
    while (reco != NULL) //while hasta que reco sea distinto a NULL
    {
        printf(" %i ", reco->info);
        reco = reco->sig;// reco almacena la direccion que tiene el puntero *sig del nodo apuntado actualmente por reco
    }
    printf("\n");
}

int extraer() // en este caso tiene que retornar un valor entero(INT)
{
    if (raiz != NULL) // si la lista no esta vacia
    {
        int informacion = raiz->info; // guardamos la informacion del primero nodo en la variable local int informacion
        struct nodo *bor = raiz; //creacion de puntero auxiliar y hacer que apunte al nodo que vamos a extraer.
        raiz = raiz->sig; //Avanzar al segundo nodo de la lista
        free(bor); //funcion free para eliminar el primer nodo de la lista
        return informacion; // retornar valor int informacion
    }
    else
    {
        return -1; // si la pila esta vacia, retornamos -1 y se lo considera como codigo de error
    }
}

void liberar()
{
    struct nodo *reco;
    struct nodo *bor;
    while (reco != NULL)
    {
        bor = reco;
        reco = reco->sig;
        free(bor);
    }
}

int cantidad()
{
    struct nodo *reco = raiz;
    int cant = 0;
    while (reco != NULL)
    {
        cant++;
        reco = reco->sig;
    }
    return cant;
}

int vacia()
{
    if (raiz == NULL)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}

int main()
{
    insertar(10);
    insertar(40);
    insertar(3);
    imprimir();
    printf("Extraemos de la pila:%i\n",extraer());
    imprimir();
    printf("La cantidad de nodos de la pila es:%i\n",cantidad());
    while (vacia() == 0)
    {
        printf("Extraemos de la pila:%i\n",extraer());
    }
    liberar();
    return 0;
}
```
## Explicacion
### Verificacion: Funcion Vacia()
Para verificar si la pila esta vacía controlamos el contenido de la variable raiz, si tiene NULL luego la lista esta vacía y por lo tanto retornamos un 1 (normalmente en el lenguaje C el **0 representa falso** y un **valor distinto a 0 representa el verdadero**):

```c
int vacia()
{
    if (raiz == NULL)
        return 1;
    else
        return 0;
}
```
### Cantidad de nodos
El algoritmo para saber la cantidad de nodos es similar al imprimir, pero en lugar de mostrar la información del nodo procedemos a incrementar un contador:

```c
int cantidad()
{
    struct nodo *reco = raiz;
    int cant = 0;
    while (reco != NULL)
    {
        cant++;
        reco = reco->sig;
    }
    return cant;
}
```
Para probar este programa en la main llamamos a insertar tres enteros:

```c
int main()
{
    insertar(10);
    insertar(40);
    insertar(3);
```
Imprimimos la pila (nos muestra los tres datos):

```c
imprimir();
```

Llamamos a la función cantidad (nos retorna un 3):
```c
printf("La cantidad de nodos de la pila es:%i\n",cantidad());
```
Luego mientras la función vacía nos retorne un cero (lista no vacía) procedemos a llamar a la función extraer:
```c

    while (vacia() == 0)
    {
        printf("Extraemos de la pila:%i\n",extraer());
    }
}
```
# Problema 3 - retornar la información del primer nodo de la Pila sin borrarlo
Agregar otra función al programa desarrollado para administrar una pila que retorne la información del primer nodo de la Pila sin borrarlo.
## Codigo
```c
//Ejercicio162.c


```