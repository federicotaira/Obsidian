Hemos visto que una función **cuando no retorna dato** definimos la palabra clave void previo al nombre de la función:

**void** cargar()

Cuando devuelve una función un tipo de dato simple puede ser un int, float o char:

**int** cargar()

En este concepto veremos que una función puede **retornar** un registro:

**struct producto** cargar()

# Problema 1:
Se tiene la siguiente declaración de registro:
```c
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; //obligatorio el punto y coma
```
Plantear dos funciones:
1. cargar un registro de tipo producto.
2. imprimir.

En la función main definir dos variables de tipo producto llamar a las funciones anteriores.
## Codigo
```c
Ejercicio135.c
#include<stdio.h>
#include<string.h>

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; //obligatorio el punto y coma

struct producto cargar()
{
    struct producto pro;
    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro.codigo);
    fflush(stdin);
    printf("Descripcion:");
    fgets(pro.descripcion,41,stdin);
    printf("Ingrese el precio:");
    scanf("%f",&pro.precio);
    return pro;
};

void imprimir(struct producto pro)
{
    printf("Datos del producto.\n");
    printf("Codigo del producto:%i\n",pro.codigo);
    printf("Descripcion:%s\n",pro.descripcion);
    printf("Precio:%0.2f\n",pro.precio);
};

int main()
{
    struct producto pro1, pro2;
    pro1=cargar();
    pro2=cargar();
    imprimir(pro1);
    imprimir(pro2);
    return 0;
}
```
## Explicacion
En la función main definimos dos variables de tipo producto:

```
int main()
{
    struct producto pro1,pro2;
```

Seguidamente llamamos a la función cargar y le asignamos a cada registro el dato que retorna la función:

```
    pro1=cargar();
    pro2=cargar();
```

La función cargar define una variable local de tipo producto y se cargan los datos por teclado y luego mediante el comando return devuelve a la main el valor de la variable local:

```
struct producto cargar()
{
    struct producto pro;
    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro.codigo);
    fflush(stdin);
    printf("Descripcion:");
    fgets(pro.descripcion,41,stdin);
    printf("Ingrese el precio:");
    scanf("%f",&pro.precio);
    return pro;
};
```

Tener en cuenta que en la main se hace una copia de la variable local devuelta por la función por lo que no se pierden los datos cargados por teclado:

```
    pro1=cargar();
```
# Problema propuesto
Se tiene la siguiente declaración de registro:

```
struct punto {
    int x;
    int y;
};
```

Definir tres variables de tipo punto y cargarlas llamando a una función que retorne valores de tipo punto.  
Finalmente crear otra función que imprima en que cuadrante se encuentra cada punto (tener en cuenta que si x>0 e y>0 se encuentra en el primer cuadrante, si x<0 e y>0 se encuentra en el segundo cuadrante y así sucesivamente)
## Codigo
```Ejercicio136.c
#include<stdio.h>
#include<string.h>

struct punto {
    int x;
    int y;
};

struct punto cargar()
{
    struct punto p;
    printf("Ingrese la coordenada x del punto:");
    scanf("%i",&p.x);
    printf("ingrese la coordenada y del punto:");
    scanf("%i",&p.y);
    return p;
}

void imprimirCuadrante(struct punto p)
{
    if(p.x>0 && p.y>0)
    {
        printf("La coordenada(%i,%i) se encuentra en el primer cuadrante",p.x,p.y);
    }
    else
    {
        if (p.x<0 && p.y>0)
        {
            printf("La coordenada(%i,%i) se encuentra en el segundo cuadrante",p.x,p.y);
        }
        else
        {
            if (p.x<0 && p.y<0)
            {
                printf("La coordenada(%i,%i) se encuentra en el tercer cuadrante",p.x,p.y);
            }
            else
            {
                if (p.x>0 && p.y<0)
                {
                    printf("La coordenada(%i,%i) se encuentra en el cuarto cuadrante",p.x,p.y);
                }
            }
        }
    }
    printf("\n");
};

int main()
{
    //Estructura simple
    /*struct punto p1,p2,p3;
    p1 = cargar();
    p2 = cargar();
    p3 = cargar();
    imprimirCuadrante(p1);
    imprimirCuadrante(p2);
    imprimirCuadrante(p3);*/

    //Estructura repetitiva
    struct punto puntos[3];
    int i;
    for (i=0; i<3; i++)
    {
        puntos[i]=cargar();
        imprimirCuadrante(puntos[i]);
    }
    return 0;
}
```
## Uso de estructura repetitiva for en struct
```    struct punto puntos[3];
    int i;
    for (i=0; i<3; i++)
    {
        puntos[i]=cargar();
        imprimirCuadrante(puntos[i]);
    }
    return 0;
```