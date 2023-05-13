Hasta ahora hemos visto como desarrollar los algoritmos para administrar una lista [[Listas tipo Pila|tipo Pila]] , hemos visto que hay bastante complejidad en el manejo de [[Variables de tipo puntero|punteros]] pero todo esto acarrea ventajas en la solución de problemas que requieren una estructura de tipo Pila.

# Planteo del problema:

Este práctico tiene por objetivo mostrar la importancia de las pilas en las Ciencias de la Computación y más precisamente en la programación de software de bajo nivel.

Todo compilador o intérprete de un lenguaje tiene un módulo dedicado a analizar si una expresión está correctamente codificada, es decir que los paréntesis estén abiertos y cerrados en un orden lógico y bien balanceados.

Se debe desarrollar un programa que tenga las siguientes responsabilidades :

- Ingresar una fórmula que contenga paréntesis, corchetes y llaves.
- Validar que los ( ) [] y {} estén correctamente balanceados. 

Veamos como nos puede ayudar el empleo de una pila para solucionar este problema.  
Primero cargaremos la fórmula por teclado.

Ejemplo de fórmula: (2+[3-12]*{8/3})

El algoritmo de validación es el siguiente:

Analizamos caracter a caracter la presencia de los paréntesis, corchetes y llaves.  
- Si vienen **símbolos de apertura** los almacenamos en la pila.  
- Si vienen **símbolos de cerrado** extraemos de la pila y verificamos si está el mismo símbolo pero de apertura: en caso negativo podemos inferir que la fórmula no está correctamente balanceada.  
- Si al finalizar el análisis del último caracter de la fórmula la pila está vacía podemos concluir que está correctamente balanceada.

Ejemplos de fórmulas no balanceadas:

- `}(2+[3-12]*{8/3})`
Incorrecta: llega una `}` de cerrado y la pila está vacía.
- `{[2+4}]`
Incorrecta: llega una llave `}` y en el tope de la pila hay un corchete `[`.
- `{[2+4]`
Incorrecta: al finalizar el análisis del último caracter en la pila queda pendiente una llave `{`.

## Codigo
```c 
//ejercicio163.c
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
#include<string.h>


struct nodo {
    char simbolo;
    struct nodo *sig;
};

struct nodo *raiz=NULL;

void insertar(char x)
{
    struct nodo *nuevo;
    nuevo = malloc(sizeof(struct nodo));
    nuevo->simbolo = x;
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


char extraer()
{
    if (raiz != NULL)
    {
        char informacion= raiz->simbolo;
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

int vacia()
{
    if (raiz == NULL)
        return 1;
    else
        return 0;
}

void cargarFormula(char *formula)
{
    printf("Ingrese la formula:");
    gets(formula);
}

int verificarBalanceada(char *formula)
{
    int f;
    for (f=0;f<strlen(formula);f++)
    {
        if (formula[f]=='(' || formula[f]=='[' || formula[f]=='{')
        {
            insertar(formula[f]);
        }
        else
        {
            if (formula[f]==')')
            {
                if (extraer()!='(')
                {
                    return 0;
                }
            }
            else
            {
                if (formula[f]==']')
                {
                    if (extraer()!='[')
                    {
                        return 0;
                    }
                }
                else
                {
                    if (formula[f]=='}')
                    {
                        if (extraer()!='{')
                        {
                            return 0;
                        }
                    }
                }
            }
        }
    }
    if (vacia())
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
    char formula[100];
    cargarFormula(formula);
    if (verificarBalanceada(formula))
    {
        printf("La formula esta correctamente balanceada");
    }
    else
    {
        printf("La formula no esta correctamente balanceada");
    }
    liberar();
    getch();
    return 0;
}

```
## Explicacion
Primero definimos una serie de funciones para administrar la Pila. Almacenamos en cada nodo un caracter y llamamos al campo de información símbolo.

No es necesario implementar las funciones imprimir, cantidad, etc. Porque no se requieren para este problema.

```c
struct nodo {
    char simbolo;
    struct nodo *sig;
};
```

```c
struct nodo *raiz=NULL;
```

Se declara el registro y se define una variable global llamada raiz que almacenará la dirección del primer nodo de la lista tipo pila.

La función insertar recibe como parámetro un caracter y la función extraer retorna un caracter:

```c
void insertar(char x)
{
    struct nodo *nuevo;
    nuevo = malloc(sizeof(struct nodo));
    nuevo->simbolo = x;
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

```c
char extraer()
{
    if (raiz != NULL)
    {
        char informacion= raiz->simbolo;
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

Definimos una función para cargar la fórmula por teclado:

```c
void cargarFormula(char *formula)
{
    printf("Ingrese la formula:");
    gets(formula);
}
```
### Funcion verificarBalanceada()
La función más importante para este problema es verificar si la fórmula tiene los paréntesis, corchetes y llaves correctamente balanceados:
```c
int verificarBalanceada(char *formula)
{
    int f;
    for (f=0;f<strlen(formula);f++)
    {
        if (formula[f]=='(' || formula[f]=='[' || formula[f]=='{')
        {
            insertar(formula[f]);
        }
        else
        {
            if (formula[f]==')')
            {
                if (extraer()!='(')
                {
                    return 0;
                }
            }
            else
            {
                if (formula[f]==']')
                {
                    if (extraer()!='[')
                    {
                        return 0;
                    }
                }
                else
                {
                    if (formula[f]=='}')
                    {
                        if (extraer()!='{')
                        {
                            return 0;
                        }
                    }
                }
            }
        }
    }
    if (vacia())
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
```
La función retorna 1 en caso de ser correcta y 0 en caso contrario.

El for se repite tantas veces como caracteres tenga la cadena de caracteres (la función strlen nos retorna la cantidad de caracteres de información ingresados por el usuario en la fórmula):

```c
    for (f=0;f<strlen(formula);f++)
```

Se deben procesar sólo los símbolos ( [ { y ) ] }.

Si el símbolo es un ( [ { de apertura procedemos a cargarlo en la pila:

```c
        if (formula[f]=='(' || formula[f]=='[' || formula[f]=='{')
        {
            insertar(formula[f]);
        }
```
En caso de ser un ) cerrado debemos extraer un carácter de la pila y verificar si no coincide con el paréntesis de apertura ( la fórmula está incorrecta:
```c
            if (formula[f]==')')
            {
                if (extraer()!='(')
                {
                    return 0;
                }
            }
```
El mismo proceso es para los símbolos ] }.

Al finalizar el análisis de toda la cadena si la pila está vacía podemos afirmar que la fórmula está correctamente balanceada, en caso contrario quiere decir que faltan símbolos de cerrado y es incorrecta:

```c
    if (vacia())
    {
        return 1;
    }
    else
    {
        return 0;
    }
```
En la main definimos un vector donde se almacenará la fórmula, la cargamos por teclado y llamamos seguidamente a la función verificarBalanceada:

```c
int main()
{
    char formula[100];
    cargarFormula(formula);
    if (verificarBalanceada(formula))
    {
        printf("La formula esta correctamente balanceada");
    }
    else
    {
        printf("La formula no esta correctamente balanceada");
    }
    liberar();
    getch();
    return 0;
}
```
Tener en cuenta en el if si retornar un 1 luego el if se verifica verdadero:

```c
    if (verificarBalanceada(formula))
```
No es necesario escribir (si bien funciona también):

```c
    if (verificarBalanceada(formula)==1)
```