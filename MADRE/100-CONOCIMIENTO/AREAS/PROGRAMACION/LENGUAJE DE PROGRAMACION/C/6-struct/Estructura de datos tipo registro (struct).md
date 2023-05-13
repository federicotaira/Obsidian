Un registro es una estructura de datos que **permite almacenar un conjunto de elementos no necesariamente del mismo tipo.**

Un registro normalmente almacena un conjunto de datos que están relacionados entre si.

Ejemplos de registros podrían ser los datos de un alumno (nro de legajo, apellido y nombre, carrera que cursa), una historia clínica de un paciente (nro de documento, obra social que tiene, enfermedades), etc.

A un [[Vectores y matrices paralelas|vector o matriz]] accedemos a sus elementos por medio de subíndices, a los elementos de un registro se los llama campos y tienen cada uno un nombre.

Por ejemplo si definimos el registro "producto" sus campos pueden ser el "codigo","descripcion" y "precio"

A diferencia de vectores y matrices los registros deben declararse y luego definir variables de dicho tipo.

Veamos la sintaxis para declarar un registro en el lenguaje C:
```
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
} ; //obligatorio el punto y coma
```
Hemos declarado un registro llamado "producto" con tres campos uno de tipo int llamado codigo, otro de tipo cadena de caracteres llamado descripcion y finalmente el campo "precio" de tipo float.

Como vemos los tres campos están relacionados y hacen referencia a las características que puede tener todo producto que vende una empresa.

Pero si solo declaramos el registro no nos sirve de nada, debemos definir una o más variables de dicho tipo.

# Problema: Declarar un struct que permita almacenar el codigo
Declarar un registro que permita almacenar el codigo, descripcion y precio de un producto. Luego definir dos variables de dicho tipo, cargarlas e imprimir el nombre del producto que tiene mayor precio.
## Codigo
```Ejercicio131.c
#include<stdio.h>
#include<string.h>

struct producto
{
    int codigo;
    char descripcion[41];
    float precio;
};

int main()
{
    struct producto pro1,pro2;
    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro1.codigo);
    fflush(stdin);
    printf("Ingrese la descripcion: ");
    gets(pro1.descripcion);
    printf("Ingrese el precio: ");
    scanf("%i",&pro1.precio);
    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro2.codigo);
    fflush(stdin);
    printf("Ingrese la descripcion:");
    gets(pro2.descripcion);
    printf("Ingrese el precio:");
    scanf("%i",&pro2.precio);
  
    if(pro1.precio>pro2.precio)
    {
        printf("El producto (%s) tiene un precio mayor.",pro1.descripcion);
    }
    else
    {
        if (pro2.precio>pro1.precio)
        {
            printf("El producto (%s) tiene un precio mayor.",pro2.descripcion);
        }
        else
        {
            printf("Tienen igual precio.");
        }
    }
    return 0;
}
```
## Explicacion
Es común declarar el registro fuera de cualquier función para luego poder utilizarlo en muchas funciones (de todos modos la declaración del struct podría estar dentro de la función main):

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; //obligatorio el punto y coma

Para definir una variable de tipo "producto" le antecedemos la palabra clave struct junto con el nombre del struct:

    struct producto pro1,pro2;

Estamos definiendo dos variables llamadas pro1 y pro2 de tipo producto. No hay ningún problema de definir cada variable en una línea distinta:

    struct producto pro1;
    struct producto pro2;

Cada una de las variables pro1 y pro2 reserva espacio para almacenar tres campos cada una.

![[Pasted image 20230217104411.png]]

Para cargar la variable pro1 debemos cargar campo a campo:

    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro1.codigo);
    fflush(stdin);
    printf("Ingrese la descripcion:");
    gets(pro1.descripcion);
    printf("Ingrese el precio:");
    scanf("%f",&pro1.precio);

Es decir primero indicamos el nombre de la variable (pro1) y seguidamente el campo que estamos cargando (codigo)

La carga del segundo registro llamado pro2 es idéntico a la carga del primer registro:

    printf("Ingrese el codigo del producto:");
    scanf("%i",&pro2.codigo);
    fflush(stdin);
    printf("Ingrese la descripcion:");
    gets(pro2.descripcion);
    printf("Ingrese el precio:");
    scanf("%f",&pro2.precio);

Para verificar cual de los dos productos tiene un precio mayor debemos acceder al campo precio de cada uno de los dos productos:

    if (pro1.precio>pro2.precio)

Lo mismo cuando procedemos a imprimir por pantalla debemos indicar que campo debemos mostrar y no podemos pasar el registro completo:

        printf("El producto %s tiene un precio mayor",pro1.descripcion);]

# Problema 2:
Se tiene la siguiente declaración de registro:

```
struct pais {
    char nombre[40];
    int cantidadhab;
};
```

Definir tres variables de tipo país y almacenar los nombres de los países y la cantidad de habitantes de dichos países.  
Mostrar seguidamente el nombre del país con mayor cantidad de habitantes (considerar que los tres países tienen cantidades distintas)
## Codigo
```
#include <stdio.h>
#include <string.h>
struct pais 
{
    char nombre[41];
    int habitantes;
};

int main() {
    struct pais pais1, pais2, pais3;
    //carga
    printf("Ingrese el nombre del pais:");
    fgets(pais1.nombre, 41, stdin);
    printf("Ingrese la cantidad de habitantes:");
    scanf("%i", &pais1.habitantes);
    fflush(stdin);
    printf("Ingrese el nombre del pais:");
    fgets(pais2.nombre, 41, stdin);
    printf("Ingrese la cantidad de habitantes:");
    scanf("%i", &pais2.habitantes);
    fflush(stdin);
    printf("Ingrese el nombre del pais:");
    fgets(pais3.nombre, 41, stdin);
    printf("Ingrese la cantidad de habitantes:");
    scanf("%i", &pais3.habitantes);
    fflush(stdin);
    
    //comparación

    if (pais1.habitantes > pais2.habitantes && pais1.habitantes > pais3.habitantes)
    {
        printf("%s tiene mayor cantidad de habitantes", pais1.nombre);
    }
    else
    {
        if (pais2.habitantes > pais3.habitantes)
        {
            printf("Pais con mayor cantidad de habitantes: %s", pais2.nombre);
        }
        else
        {
            printf("Pais con mayor cantidad de habitantes: %s", pais3.nombre);
        }
    }
    return 0;
}
```
