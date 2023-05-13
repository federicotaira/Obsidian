Una matriz es una estructura de datos que permite almacenar un CONJUNTO de datos del MISMO tipo.  
Con un único nombre se define la matriz y por medio de DOS subíndices hacemos referencia a cada elemento de la misma (componente)
![[Pasted image 20230214142948.png]]
Hemos graficado una matriz de 3 filas y 5 columnas. Para hacer referencia a cada elemento debemos indicar primero la fila y luego la columna, por ejemplo en la componente (1,4) se almacena el valor 97.  
En este ejemplo almacenamos valores enteros. Todos los elementos de la matriz deben ser del mismo tipo.  
Las filas y columnas comienzan a numerarse a partir de cero, similar a los vectores.
# Problema 1
Crear una matriz de 3 filas por 5 columnas con elementos de tipo int, cargar sus componentes y luego imprimirlas.
## Codigo
```ejercicio114.c
#include<stdio.h>
void cargar(int matriz[3][5])
{
    int i,j;
    for(i=0;i<3;i++)
    {
        for(j=0;j<5;j++)
        {
            printf("ingrese los componentes: ");
            scanf("%i", &matriz[i][j]);
        }
    }
}
void imprimir(int matriz[3][5])
{
    int i,j;
    for(i=0;i<3;i++)
    {
       for(j=0;j<5;j++)
        {
            printf("%i", matriz[i][j]);
        }
        printf("\n");
    }    
}

int main()

{
    int matriz[3][5];
    cargar(matriz);
    imprimir(matriz);
    return 0;
}
```
Definimos en la función main una matriz.  
Para definir una matriz en el lenguaje C debemos disponer como primer subíndice la cantidad de filas y como segundo subíndice la cantidad de columnas:

    int mat[3][5];

Para cargar sus 15 componentes (cada fila almacena 5 componentes y tenemos 3 filas), lo más cómodo es utilizar un for anidado, el primer for que incrementa el contador "i" lo utilizamos para recorrer las filas y el contador interno llamado "j" lo utilizamos para recorrer las columnas.

Cada vez que se repite en forma completa el for interno se carga una fila completa, primero se carga la fila cero en forma completa, luego la fila uno y finalmente la fila 2.  
Siempre que accedemos a una posición de la matriz debemos disponer dos subíndices que hagan referencia a la fila y columna mat[f][c]):

```
void cargar(int mat[3][5])
{
    int f,c;
    for(f=0;f<3;f++)
    {
        for(c=0;c<5;c++)
        {
            printf("Ingrese componente:");
            scanf("%i",&mat[f][c]);
        }
    }
}
```
Para imprimir la matriz de forma similar utilizamos dos for para acceder a cada elemento de la matriz:

```
void imprimir(int mat[3][5])
{
    int f,c;
    for(f=0;f<3;f++)
    {
        for(c=0;c<5;c++)
        {
            printf("%i ",mat[f][c]);
        }
        printf("\n");
    }
}
```
Cada vez que se ejecuta todas las vueltas del for interno tenemos en pantalla una fila completa de la matriz, por eso pasamos a ejecutar un salto de línea (con esto logramos que en pantalla los datos aparezcan en forma matricial):

```
printf("\n");
```
# Problema 2-Imprimir la diagonal principal.


Crear y cargar una matriz de 4 filas por 4 columnas. Imprimir la diagonal principal.

              x    -    -    -
              -    x    -    -
              -    -    x    -
              -    -    -    x

```Ejercicio115.c
#include<stdio.h>
#include<conio.h>

void cargar(int mat[4][4])
{
    int f,c;
    for(f=0;f<4;f++)
    {
        for(c=0;c<4;c++)
        {
            printf("Ingrese componente:");
            scanf("%i",&mat[f][c]);
        }
    }
}

void imprimirDiagonalPrincipal(int mat[4][4])
{
    int k;
    for(k=0;k<4;k++)
    {
        printf("%i ",mat[k][k]);
    }
}

void imprimir(int mat[4][4])
{
    int f,c;
    for(f=0;f<4;f++)
    {
        for(c=0;c<4;c++)
        {
            printf("%i ",mat[f][c]);
        }
        printf("\n");
    }
}


int main()
{
    int mat[4][4];
    cargar(mat);
    imprimir(mat);
    printf("Los elementos de la diagonal principal son:");
    imprimirDiagonalPrincipal(mat);
    getch();
    return 0;
}
```
La definición de la matriz en la main, su carga e impresión completa no varían con el ejemplo anterior.

Para imprimir la diagonal principal de la matriz lo más conveniente es utilizar un for que se repita 4 veces y disponer como subíndice dicho contador (los elementos de la diagonal principal coinciden los valores de la fila y columna):

void imprimirDiagonalPrincipal(int mat[4][4])
{
    int k;
    for(k=0;k<4;k++)
    {
        printf("%i ",mat[k][k]);
    }
}

Recordar que si bien es muy común utilizar dos for para procesar matrices, en muchas situaciones si tenemos que acceder a una sola fila, una sola columna, una diagonal etc. se utilice un solo for o inclusive sin for.
# Problema 3- Imprimir fila y columnas
Crear y cargar una matriz de 3 filas por 4 columnas. Imprimir la primer fila. Imprimir la última fila e imprimir la primer columna.
## Codigo
```Ejercicio116.c
#include<stdio.h>

void cargar(int mat[3][4])

{

    int f,c;

    for(f=0;f<3;f++)

    {

        for(c=0;c<4;c++)

        {

            printf("Ingrese elemento:");

            scanf("%i",&mat[f][c]);

        }

    }

}

  

void primeraFila(int matriz[3][4])

{   int i;

printf("Primera fila de la matriz: ");

    for ( i = 0; i < 4; i++)

    {

        printf("%i ", matriz[0][i]);        

    }

    printf("\n");

}

void ultimaFila(int matriz[3][4])

{   int i;

printf("Ultima fila de la matriz: ");

    for ( i = 0; i < 4; i++)

    {

        printf("%i ", matriz[2][i]);        

    }

    printf("\n");

}

void primerColumna(int matriz[3][4])

{

    printf("Primer columna de la matriz: ");

    int i;

    for(i=0;i<3;i++)

    {

        printf("%i ", matriz[i][0]);

    }

}

void imprimir(int matriz[3][4])

{

    int i,j;

    printf("Matriz completa\n");

    for(i=0;i<3;i++)

    {

        for(j=0;j<4;j++)

        {

            printf("%i ", matriz[i][j]);

        }

        printf("\n");

    }

}

  

int main()

{

    int matriz[3][4];

    cargar(matriz);

    imprimir(matriz);

    primeraFila(matriz);

    ultimaFila(matriz);

    primerColumna(matriz);

    return 0;

}
```
En la función main definimos la matriz y se la pasamos a las distintas funciones para procesarla:

```
int main()
{
    int mat[3][4];
    cargar(mat);
    imprimir(mat);
    primerFila(mat);
    ultimaFila(mat);
    primerColumna(mat);
    getch();
    return 0;
}
```

Luego de cargarla por teclado el primer método que codificamos es el que imprime la primer fila. Disponemos un for para recorrer las columnas, ya que la fila siempre será la cero. Como son cuatro los elementos de la primer fila el for se repite esta cantidad de veces:

```
void primerFila(int mat[3][4])
{
    int c;
    printf("Primer fila de la matriz:");
    for(c=0;c<4;c++)
    {
        printf("%i ",mat[0][c]);
    }
    printf("\n");
}
```
Para imprimir la última fila el algoritmo es similar, disponemos un for que se repita 4 veces y en el subíndice de la fila disponemos el valor 2 (ya que la matriz tiene 3 filas):
```
void ultimaFila(int mat[3][4])
{
    int c;
    printf("Ultima fila de la matriz:");
    for(c=0;c<4;c++)
    {
        printf("%i ",mat[2][c]);
    }
    printf("\n");
}
```
Para imprimir la primer columna el for debe repetirse 3 veces ya que la matriz tiene 3 filas. Dejamos constante el subíndice de la columna con el valor cero:

```
void primerColumna(int mat[3][4])
{
    int f;
    printf("Primer columna:");
    for(f=0;f<3;f++)
    {
        printf("%i ",mat[f][0]);
    }
}
```
# Problema 4-carga de componentes por columna 
Crear una matriz de 2 filas y 5 columnas. Realizar la carga de componentes por columna (es decir primero ingresar toda la primer columna, luego la segunda columna y así sucesivamente) Imprimir luego la matriz.
## Codigo
```ejercicio117.c
#include<stdio.h>

void cargar(int matriz[2][5])
{
   int i,j;
    for ( j = 0; j < 5; j++)
    {
        for ( i = 0; i < 2; i++)
        {
           printf("Ingrese elemento de la fila %i y columna %i: ",i,j);
           scanf("%i", &matriz[i][j]);
        }
    }
}

void imprimir(int matriz[2][5])
{
    int i,j;
    for ( i = 0; i < 2; i++)
    {
        for(j = 0; j < 5; j++)
        {
            printf("%i",matriz[i][j]);
        }
        printf("\n");
    }
}

int main()
{
    int matriz[2][5];
    cargar(matriz);
    imprimir(matriz);
    return 0;
}
```
# Problema 5-imprimir el elemento mayor.
Crear una matriz de 3x4. Realizar la carga y luego imprimir el elemento mayor.
## Codigo
```ejercicio118.c
#include<stdio.h>

  

void cargar(int matriz[3][4])

{

    int i,j;

    for ( i = 0; i < 3; i++)

    {

        for ( j = 0; j < 4; j++)

        {

           printf("Ingrese elementos:");

           scanf("%i", &matriz[i][j]);

        }        

    }    

}

void imprimir(int mat[3][4])

{

    int f,c;

    printf("Matriz completa\n");

    for(f=0;f<3;f++)

    {

        for(c=0;c<4;c++)

        {

            printf("%i ",mat[f][c]);

        }

        printf("\n");

    }

}

void mayor(int mat[3][4])

{

    int f,c;

    int may=mat[0][0];

    for(f=0;f<3;f++)

    {

        for(c=0;c<4;c++)

        {

            if (mat[f][c]>may)

            {

                may=mat[f][c];

            }

        }

    }

    printf("Elemento mayor de la matriz: %i",may);

}

int main()

{

    int matriz[3][4];

    cargar(matriz);

    imprimir(matriz);

    mayor(matriz);

    return 0;

}

```
# Problema 6-Intercambio de elementos
Definir una matriz de 2 filas y 5 columnas. Realizar su carga e impresión.  
Intercambiar los elementos de la primera fila con la segunda y volver a imprimir la matriz.
## Codigo
```ejercicio119.c
#include<stdio.h>
#include<conio.h>
void cargar(int mat[2][5])

{

    int f,c;

    for(f=0;f<2;f++)

    {

        for(c=0;c<5;c++)

        {

            printf("Ingrese elemento:");

            scanf("%i",&mat[f][c]);

        }

    }

}

  

void imprimir(int mat[2][5])

{

    int f,c;

    printf("Matriz completa\n");

    for(f=0;f<2;f++)

    {

        for(c=0;c<5;c++)

        {

            printf("%i ",mat[f][c]);

        }

        printf("\n");

    }

}

  

void intercambiarFilas(int mat[2][5])

{

    int c;

    int aux;

    for(c=0;c<5;c++)

    {

        aux=mat[0][c];

        mat[0][c]=mat[1][c];

        mat[1][c]=aux;

    }

}

  
  

int main()

{

    int mat[2][5];

    cargar(mat);

    imprimir(mat);

    intercambiarFilas(mat);

    imprimir(mat);

    getch();

    return 0;

}
```