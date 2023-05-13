Hemos visto ya varias estructuras de datos fundamentales en el lenguaje C:

[[Estructura de datos tipo vector (elementos int y float)|vectores]]

[[Estructura de datos tipo matriz (elementos int y float)|matriz]]

[[Estructura de datos tipo registro (struct)|registros]]

También hemos visto que podemos definir [[Estructura de datos tipo vector (elementos de tipo struct)|vectores y matrices cuyos elementos sean de tipo registro.]]

En este concepto veremos que **los campos de un registro pueden ser también de tipo registro, es decir anidar un registro dentro de otro registro.**
# Problema Carga e Impresion de 2 registros
Se tienen las siguientes declaraciones de registros:

```
struct fecha {
    int dd;
    int mm;
    int aa;
};
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
    struct fecha fechavencimiento;
}; 

```
Definir un vector de 3 elementos de tipo producto, realizar su carga e impresión.

## Codigo
```Ejercicio139.c
#include<stdio.h>
#include<string.h>

#define TAMANO 3

struct fecha {
    int dd;
    int mm;
    int aa;
};

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
    struct fecha fechaVencimiento;
};

 void cargar(struct producto pro[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
        printf("Ingrese el codigo del producto:");
        scanf("%i", &pro[i].codigo);
        fflush(stdin);
        printf("Ingrese la descripcion del producto:");
        fgets(pro[i].descripcion,41,stdin);
        printf("Ingrese el precio del producto:");
        scanf("%f", &pro[i].precio);
        printf("Ingrese la fecha de vencimiento del producto:\n");
        printf("Ingrese dia:");
        scanf("%i",&pro[i].fechaVencimiento.dd);
        printf("Ingrese mes:");
        scanf("%i",&pro[i].fechaVencimiento.mm);
        printf("Ingrese ano:");
        scanf("%i",&pro[i].fechaVencimiento.aa);
        printf("__________________________\n")
    }
}

void imprimir(struct producto pro[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
        printf("Codigo:%i\n", pro[i].codigo);
        printf("Descripcion:%s\n", pro[i].descripcion);
        printf("Precio:%0.2f\n", pro[i].precio);
        printf("Fecha de vencimiento:%i/%i/%i\n", pro[i].fechaVencimiento.dd,pro[i].fechaVencimiento.mm,pro[i].fechaVencimiento.aa);
        printf("__________________________\n")
    }
}  

int main()
{
    struct producto pro[TAMANO];
    cargar(pro);
    imprimir(pro);
    return 0;
}
```
## Explicacion
En este problema declaramos dos registros, **el orden en que se los declara es importante en el lenguaje C** , primero debemos declarar el struct fecha ya que el struct producto tiene un campo de tipo fecha:
```
struct fecha {
    int dd;
    int mm;
    int aa;
};

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
    struct fecha fechaVencimiento;
};
```

Si los declaramos al revés aparecerá un error sintáctico.

Cuando definimos un campo de un registro que es de otro tipo de dato registro debemos anteceder la palabra clave struct y el nombre de tipo registro:

    struct fecha fechavencimiento;

Cuando procedemos a cargar el vector y como sabemos cada campo de cada registro debe cargarse por separado:

```
 void cargar(struct producto pro[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
        printf("Ingrese el codigo del producto:");
        scanf("%i", &pro[i].codigo);
        fflush(stdin);
        printf("Ingrese la descripcion del producto:");
        fgets(pro[i].descripcion,41,stdin);
        printf("Ingrese el precio del producto:");
        scanf("%f", &pro[i].precio);
        printf("Ingrese la fecha de vencimiento del producto:\n");
        printf("Ingrese dia:");
        scanf("%i",&pro[i].fechaVencimiento.dd);
        printf("Ingrese mes:");
        scanf("%i",&pro[i].fechaVencimiento.mm);
        printf("Ingrese ano:");
        scanf("%i",&pro[i].fechaVencimiento.aa);
        printf("__________________________\n")
    }
}
```

Como vemos cuando cargamos el campo fechavencimiento que es un campo del struct producto debemos hacerlo también campo a campo:

```
        printf("Ingrese la fecha de vencimiento del producto:\n");
        printf("Ingrese dia:");
        scanf("%i",&pro[i].fechaVencimiento.dd);
        printf("Ingrese mes:");
        scanf("%i",&pro[i].fechaVencimiento.mm);
        printf("Ingrese ano:");
        scanf("%i",&pro[i].fechaVencimiento.aa);
```

En la función main definimos un vector con componentes de tipo producto, en ningún momento definimos un vector o registro de tipo fecha:

```
int main()
{
    struct producto pro[TAMANO];
    cargar(pro);
    imprimir(pro);
    return 0;
}
```

Lo mismo para imprimir todos los datos de los productos e inclusive el día, mes y año de vencimiento de cada producto debemos indicarlos uno por uno:

```
void imprimir(struct producto pro[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
        printf("Codigo:%i\n", pro[i].codigo);
        printf("Descripcion:%s\n", pro[i].descripcion);
        printf("Precio:%0.2f\n", pro[i].precio);
        printf("Fecha de vencimiento:%i/%i/%i\n", pro[i].fechaVencimiento.dd,pro[i].fechaVencimiento.mm,pro[i].fechaVencimiento.aa);
        printf("__________________________\n")
    }
} 

opcion 2
void imprimir(struct producto vec[TAMANO])
{
    int f;
    for(f=0;f<TAMANO;f++)
    {
        printf("%i %s %0.2f %i/%i/%i\n",vec[f].codigo, vec[f].descripcion, vec[f].precio,
               vec[f].fechavencimiento.dd, vec[f].fechavencimiento.mm, vec[f].fechavencimiento.aa);
    }
}
```
# Problema propuesto

Se tienen las siguientes declaraciones de registros:

```
struct punto {
    int x;
    int y;
};

struct triangulo {
    struct punto punto1;
    struct punto punto2;
    struct punto punto3;
};
```

Definir en la main un registro de tipo triangulo.  
Codificar las funciones:  
1. Una función que retorne un registro de tipo triangulo.  
2. Impresión del registro.