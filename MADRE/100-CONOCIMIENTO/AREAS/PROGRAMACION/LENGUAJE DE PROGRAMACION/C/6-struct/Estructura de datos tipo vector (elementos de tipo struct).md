# Problema 
Se tiene la siguiente declaración de registro:

```
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; 
```

Definir un vector de 4 elementos de tipo producto.  
Implementar las funciones:

-   Carga del vector.
-   Impresión del vector.
-   Mostrar el nombre del articulo con precio mayor.
## Codigo
```Ejercicio137.c
#include <stdio.h>
#include <string.h>

#define TAMANO 4

struct producto
{
    int codigo;
    char descripcion[41];
    float precio;
};
void cargar(struct producto vec[TAMANO])
{
    int i;
    for (i = 0; i < TAMANO; i++)
    {
        printf("Ingrese el codigo del producto:");
        scanf("%i", &vec[i].codigo);
        fflush(stdin);
        printf("Ingrese la Descripcion del producto:");
        fgets(vec[i].descripcion, 41, stdin);
        printf("Ingrese el precio:");
        scanf("%f", &vec[i].precio);
        printf("_________________________________\n");
    }
}
 
void imprimirLista(struct producto vec[TAMANO])
{
    int i;
    for (i = 0; i < TAMANO; i++)
    {
        printf("Coodigo del producto: %i\n", vec[i].codigo);
        printf("Descripcion: %s\n", vec[i].descripcion);
        printf("Precio: %0.2f\n", vec[i].precio);
        printf("_________________________________\n");
    }
    printf("\n");
}
void precioMayor(struct producto vec[TAMANO])
{
    int i;
    int mayor = 0;
    for (i = 0; i < TAMANO; i++)
    {
        if (vec[i].precio > vec[i].precio)
        {
            mayor = i;
        }
    }
    printf("El producto mas caro: %s", vec[mayor].descripcion);
}
  
int main()
{
    struct producto vector[TAMANO];
    cargar(vector);
    imprimirLista(vector);
    precioMayor(vector);
    return 0;
}
```
## Explicacion
En la función main definimos un vector de cuatro elementos de tipo registro, llamamos a las distintas funciones y le pasamos el vector para cargarlo, imprimirlo y ver el producto más caro:

```
int main()
{
    struct producto vector[TAMANO];
    cargar(vector);
    imprimir(vector);
    precioMayor(vector);
    return 0;
}
```

La función de carga recibe el vector (como sabemos los [[Funciones con parametros de tipo vector|vectores como parámetros]] si son modificados luego la [[Directiva Define|variable definida]] en la main es modificada.

```
void cargar(struct producto vec[TAMANO])
{
    int i;
    for (i = 0; i < TAMANO; i++)
    {
        printf("Ingrese el codigo del producto:");
        scanf("%i", &vec[i].codigo);
        fflush(stdin);
        printf("Ingrese la Descripcion del producto:");
        fgets(vec[i].descripcion, 41, stdin);
        printf("Ingrese el precio:");
        scanf("%f", &vec[i].precio);
        printf("_________________________________\n");
    }
}
```
### Carga del vector
Debemos indicar el nombre del vector, el subíndice que queremos acceder y finalmente a que campo del registro de dicha componente:

        scanf("%i",&vec[f].codigo);
### Impresión del vector
La función de imprimir accedemos a cada componente del vector y a cada uno de los campos de cada componente:
```
void imprimirLista(struct producto vec[TAMANO])
{
    int i;
    for (i = 0; i < TAMANO; i++)
    {
        printf("Coodigo del producto: %i\n", vec[i].codigo);
        printf("Descripcion: %s\n", vec[i].descripcion);
        printf("Precio: %0.2f\n", vec[i].precio);
        printf("_________________________________\n");
    }
    printf("\n");
}
```
```Alternativa
void imprimirLista(struct producto vec[TAMANO])
 {
    int f;
    for(f=0;f<TAMANO;f++)
    {
		printf("%i %s %0.2f\n", vec[f].codigo, vec[f].descripcion, vec[f].precio);
    }
  }
```

Para obtener el nombre del producto con mayor precio procedemos a inicializar la variable pos con el valor cero suponiendo que en dicha posición se encuentra el artículo con mayor precio. Luego en cada vuelta del for verificamos el precio del artículo si supera al que hemos considerado mayor hasta ese momento, en caso afirmativo actualizamos la variable pos:
### Mostrar el nombre del articulo con precio mayor
```
void precioMayor(struct producto vec[TAMANO])
{
    int i;
    int mayor = 0;
    for (i = 0; i < TAMANO; i++)
    {
        if (vec[i].precio > vec[i].precio)
        {
            mayor = i;
        }
    }
    printf("El producto mas caro: %s", vec[mayor].descripcion);
}
```
# Problema propuesto
Se tiene la siguiente declaración de registro:
```
struct libro{
    int codigo;
    char titulo[40];
    char autor[40];
};
```
[[Directiva Define#Problema 1-Definir una macro para indicar el tamaño del vector#Explicacion|Definir]] un vector de cuatro elementos de tipo libro.  
Codificar las funciones:  
1. Carga del vector.  
2. Listado completo.  
3. Ingresar por teclado un nombre de autor y luego mostrar todos los títulos de libros que ha escrito o un mensaje si no tiene.