El ordenamiento de un vector en el lenguaje C se logra intercambiando las componentes de manera que:  
vec[0] <= vec[1] <= vec[2] etc.

El contenido de la componente vec[0] sea menor o igual al contenido de la componente vec[1] y así sucesivamente.  
Si se cumple lo dicho anteriormente decimos que el vector está ordenado de menor a mayor. Igualmente podemos ordenar un vector de mayor a menor.

Se puede ordenar tanto vectores con componentes de tipo int, float y char
**Importante**
	if (vector[j]**>** vector[j+1]) => ordena de menor a mayor
	if (vector[j]**<** vector[j+1]) => ordena de mayor a menor

# Problema 1: Almacenar sueldos y ordenar de menor a mayor
Se debe crear un vector donde almacenar 5 sueldos. Ordenar el vector sueldos de menor a mayor.
![[Pasted image 20230214123330.png]]
Esta primera aproximación tiene por objetivo analizar los intercambios de elementos dentro del vector.

El algoritmo consiste en comparar si la primera componente es mayor a la segunda, en caso que la condición sea verdadera, intercambiamos los contenidos de las componentes.

Vamos a suponer que se ingresan los siguientes valores por teclado:

1200
750
820
550
490

En este ejemplo: ¿es 1200 mayor a 750? La respuesta es verdadera, por lo tanto intercambiamos el contenido de la componente 0 con el de la componente 1.  
Luego comparamos el contenido de la componente 1 con el de la componente 2: ¿Es 1200 mayor a 820?  
La respuesta es verdadera entonces intercambiamos.  
Si hay 5 componentes hay que hacer 4 comparaciones, por eso el for se repite 4 veces.  
Generalizando: si el vector tiene N componentes hay que hacer N-1 comparaciones.  

| Cuando | f = 0 | f = 1 | f  = 2 | f = 3 |
| ------ | ----- | ----- | ------ | ----- |
| ---    | 750   | 750   | 750    | 750   |
|        | 1200  | 820   | 820    | 820   |
|        | 820   | 1200  | 550    | 550   |
|        | 550   | 550   | 1200   | 490   |
|        | 490   | 490   | 490    | 1200  |
|        |       |       |        |       |
Podemos ver cómo el valor más grande del vector desciende a la última componente. Empleamos una variable auxiliar (aux) para el proceso de intercambio:

```
aux=sueldos[f];
sueldos[f]=sueldos[f+1];
sueldos[f+1]=aux;
```

Al salir del for en este ejemplo el contenido del vector es el siguiente:

750
820
550
490
1200

Analizando el algoritmo podemos comprobar que el elemento mayor del vector se ubica ahora en el último lugar.  
Podemos definir otros vectores con distintos valores y comprobar que siempre el elemento mayor queda al final.

Pero todavía con este algoritmo no se ordena un vector. Solamente está ordenado el último elemento del vector.

Ahora bien, con los 4 elementos que nos quedan podemos hacer el mismo proceso visto anteriormente, con lo cual quedará ordenado otro elemento del vector. Este proceso lo repetiremos hasta que quede ordenado por completo el vector.

Como debemos repetir el mismo algoritmo podemos englobar todo el bloque en otra estructura repetitiva.
![[Pasted image 20230214133947.png]]
Realicemos una prueba del siguiente algoritmo:
| cuando i=0 | j = 1 | j = 2 | j = 3 | j = 4 |
| ---------- | ----- | ----- | ----- | ----- |
| ---------- | 750   | 750   | 750   | 750   |
|            | 1200  | 820   | 820   | 820   |
|            | 820   | 1200  | 550   | 550   |
|            | 550   | 550   | 1200  | 490   |
|            | 490   | 490   | 490   | 1200  |

| cuando i=1 | j = 1 | j = 2 | j = 3 | j = 4 |
| ---------- | ----- | ----- | ----- | ----- |
| ---------- | 750   | 750   | 750   | 750   |
|            | 820   | 550   | 550   | 550   |
|            | 550   | 820   | 490   | 490   |
|            | 490   | 490   | 820   | 820   |
|            | 1200  | 1200  | 1200  | 1200  |

| cuando i=2 | j = 1 | j = 2 | j = 3 | j = 4 |
| ---------- | ----- | ----- | ----- | ----- |
| ---------- | 550   | 550   | 550   | 550   |
|            | 750   | 490   | 490   | 490   |
|            | 490   | 750   | 750   | 750   |
|            | 820   | 820   | 820   | 820   |
|            | 1200  | 1200  | 1200  | 1200  |

| cuando i=3 | j = 1 | j = 2 | j = 3 | j = 4 |
| ---------- | ----- | ----- | ----- | ----- |
| ---------- | 490   | 490   | 490   | 490   |
|            | 550   | 550   | 550   | 550   |
|            | 750   | 750   | 750   | 750   |
|            | 820   | 820   | 820   | 820   |
|            | 1200  | 1200  | 1200  | 1200  |
¿Porque repetimos 4 veces el for externo?  
Como sabemos cada vez que se repite en forma completa el for interno queda ordenada una componente del vector. A primera vista diríamos que deberíamos repetir el for externo la cantidad de componentes del vector, en este ejemplo el vector sueldos tiene 5 componentes.

Si observamos, cuando quedan dos elementos por ordenar, al ordenar uno de ellos queda el otro automáticamente ordenado (podemos imaginar que si tenemos un vector con 2 elementos no se requiere el for externo, porque este debería repetirse una única vez)

Una última consideración a este ALGORITMO de ordenamiento es que los elementos que se van ordenando continuamos comparándolos.

Ejemplo: En la primera ejecución del for interno el valor 1200 queda ubicado en la posición 4 del vector. En la segunda ejecución comparamos si el 820 es mayor a 1200, lo cual seguramente será falso.  
Podemos concluir que la primera vez debemos hacer para este ejemplo 4 comparaciones, en la segunda ejecución del for interno debemos hacer 3 comparaciones y en general debemos ir reduciendo en uno la cantidad de comparaciones.  
Si bien el algoritmo planteado funciona, un algoritmo más eficiente, que se deriva del anterior es el plantear un for interno con la siguiente estructura: (j=0 ; j<4-i; j++)  
Es decir restarle el valor del contador del for externo.
## Codigo
```ejercicio112.c
#include<stdio.h>
void carga(int sueldos[5])
{
    for (int i = 0; i < 5; i++)
    {
        printf("Ingrese los sueldos: ");
        scanf("%i", &sueldos[i]);
    }
}

void ordenar(int sueldos[5])
{
    for ( int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            if(sueldos[j]>sueldos[j+1])
            {
                int aux = sueldos[j];
                sueldos[j] = sueldos[j+1];
                sueldos[j+1] = aux;

              }
        }
    }
}

void imprimir(int sueldos[5])
{
    printf("lista de sueldos:\n");
    for (int i = 0; i < 5; i++)
    {
       printf("%i\n", sueldos[i]);
    }
}

int main()
{
    int sueldos[5];
    int may;
    carga(sueldos);
    ordenar(sueldos);
    imprimir(sueldos);
    return 0;

}
```

# Problema 2: Ordenar Mayor a menor y Menor a Mayor
Cargar un vector de 5 elementos enteros. Ordenarlo de mayor a menor y mostrarlo por pantalla, luego ordenar de menor a mayor e imprimir nuevamente.

**Importante**
	if (vector[j]**>** vector[j+1]) => ordena de menor a mayor
	if (vector[j]**<** vector[j+1]) => ordena de mayor a menor
## Codigo
```Ejercicio113.c

#include<stdio.h>

void cargar(int vector[5])

{

  

    for (int i = 0; i < 5; i++)

    {

        printf("Ingrese los elementos: ");

        scanf("%i", &vector[i]);

    }

}

void ordenMenorMayor(int vector[5])

{

    for (int i = 0; i < 4; i++)

    {

        for (int j = 0; j < 4-i; j++)

        {

            if (vector[j]>vector[j+1])

            {

                int aux = vector[j];

                vector[j] = vector[j+1];

                vector[j+1] = aux;

            }

        }

    }

}

void imprimir(int vector[5])

{

    for (int i = 0; i < 5; i++)

    {

        printf("%i\n", vector[i]);

    }

}

void ordenMayorMenor(int vector[5])

{

    for (int i = 0; i < 4; i++)

    {

        for (int j = 0; j < 4-i; j++)

        {

            if (vector[j]<vector[j+1])

            {

                int aux = vector[j];

                vector[j] = vector[j+1];

                vector[j+1] = aux;

            }

        }

    }

}

/*void imprimirDesc(int vector[5])

{

    printf("lista de elementos orden descendente:\n");

    for (int i = 0; i < 5; i++)

    {

        printf("%i\n", vector[i]);

    }

}*/

  

int main()

{

    int vector[5];

    cargar(vector);

    ordenMenorMayor(vector);

    printf("lista de orden de menor a mayor:\n");

    imprimir(vector);

    ordenMayorMenor(vector);

    printf("lista de orden de mayor a menor:\n");

    imprimir(vector);

    return 0;

}
```
## Importante!
### Funcion de ordenamiento de menor a mayor
if (vector[j]**>** vector[j+1]) => ordena de menor a mayor
```
void ordenMenorMayor(int vector[5])

{
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4-i; j++)
        {
            if (vector[j]>vector[j+1])
            {
                int aux = vector[j];
                vector[j] = vector[j+1];
                vector[j+1] = aux;
            }
        }
    }
}

```
### Funcion de ordenamiento de mayor a menor
if (vector[j]**<** vector[j+1]) => ordena de menor a mayor
```
void ordenMayorMenor(int vector[5])
{
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4-i; j++)
        {
            if (vector[j]<vector[j+1])
            {
                int aux = vector[j];
                vector[j] = vector[j+1];
                vector[j+1] = aux;
            }
        }
    }
}
```