oyUn algoritmo muy común es la búsqueda del mayor y menor elemento de un vector, lo mismo que su posición.

Tiene sentido sobre todo trabajando con vectores que almacenan valores enteros o flotantes.

Si tenemos un vector que almacena los siguientes datos:
![[Pasted image 20230214120349.png]]
Luego el mayor del vector es el número 250 que se encuentra en la posición 4.:
# Problema 1- Imprimir elemento mayor y su posicion.
Confeccionar un programa que defina en la main un vector de 5 elementos de tipo entero. Cargar e imprimir el mayor elemento y su posción.
## Codigo
```Ejercicio110.c
#include<stdio.h>
#include<conio.h>

void cargar(int vector[5])
{
    int x;
    for(x=0;x<5;x++)
    {
        printf("Ingrese elemento:");
        scanf("%i",&vector[x]);
    }
}

void mayor(int vector[5])
{
    int may=vector[0];
    int pos=0;
    int x;
    for(x=1;x<5;x++)
    {
        if (vector[x]>may)
        {
            may=vector[x];
            pos=x;
        }
    }
    printf("Mayor elemento del vector:%i\n",may);
    printf("Se encuentra en la posicion:%i",pos);
}


int main()
{
    int vector[5];
    cargar(vector);
    mayor(vector);
    getch();
    return 0;
}
```
En la función main procedemos a definir el vector y llamar a la función de cargar:

```
int main()
{
    int vector[5];
    cargar(vector);
}
```
Para obtener el mayor elemento y la posición del mismo realizar los siguientes pasos:

```
void mayor(int vector[5])
{
    int may=vector[0];
    int pos=0;
    int x;
    for(x=1;x<5;x++)
    {
        if (vector[x]>may)
        {
            may=vector[x];
            pos=x;
        }
    }
    printf("Mayor elemento del vector:%i\n",may);
    printf("Se encuentra en la posicion:%i",pos);
}
```

Inicializamos una variable llamada may con la primer componente del vector:

    int may=vector[0];

Inicializamos una variable llamada pos con el valor 0, ya que consideramos que el mayor es la primer componente del vector:

    int pos=0;

Recorremos las componentes del vector que faltan analizar, o sea, de la 1 a la 4:

    for(x=1;x<5;x++)

Accedemos a cada componente para controlar si supera lo que tiene la variable may:

        if (vector[x]>may)

En caso de ser verdadera la condición asignamos a la variable may este nuevo valor de vector[x]:

            may=vector[x];

y a la variable pos le cargamos la variable x que indica la posición de la componente que estamos analizando:

            pos=x;

Cuando salimos de la estructura repetitiva imprimimos la variable may que contiene el mayor valor del vector y la variable pos almacena la posición donde se almacena dicho mayor en el vector:

    printf("Mayor elemento del vector:%i\n",may);
    printf("Se encuentra en la posicion:%i",pos);

# Problema 2- Imprimir el menor elemento 
Cargar un vector de 5 elementos enteros. Imprimir el menor y un mensaje si se repite dentro del vector
## Codigo
```Ejercicio111.c
#include<stdio.h>

void carga(int vec[5])

{

  

    for (int i = 0; i < 5; i++)

    {

        printf("Ingrese los elementos: ");

        scanf("%i", &vec[i]);

    }

}

  

int menor(int vec[5])

{

    int menor=vec[0];

    for (int i = 1; i < 5; i++)

    {

        if(vec[i]<menor)

        {

            menor = vec[i];

        }

    }

    return menor;

}

void verificarRepite(int vec[5], int men)

{

    int cant = 0;

    for (int i=0;i<5;i++)

    {

        if(vec[i]==men)

        {

            cant++;

        }

    }

    if(cant==1)

    {

        printf("El menor elemento del vecetor no se repite");

    }

    else

    {

        printf("El menor elemento se encuentra repetido dentro del vector");

    }

}

int main()

{

    int vector[5];

    int men;

    carga(vector);

    men=menor(vector);

    printf("El elemento menor del vector es %i\n", men);

    verificarRepite(vector,men);

    return 0;

}
```