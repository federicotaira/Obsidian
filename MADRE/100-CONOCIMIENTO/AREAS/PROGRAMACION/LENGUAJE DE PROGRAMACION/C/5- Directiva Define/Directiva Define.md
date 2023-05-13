La directiva `#define` especifica un nombre que será reemplazado por un cierto valor en todos los lugares del programa donde se haga referencia a dicho nombre.

Se crean fuera de cualquier función normalmente en la parte inicial del archivo luego de las directivas `#include`. La sintaxis es la siguiente:

`#define` [nombre de la macro]  [valor de la macro]

Un ejemplo concreto es:

`#define` TAMANO 20
`#define` MENSAJEFIN "Presione una tecla para finalizar"

El nombre de la macro es común que se lo escriba con caracteres mayúsculas (es una regla que utilizan muchos programadores pero no es obligatoria)

Luego de especificar el nombre de la macro hay uno o más espacios en blanco y debemos especificar el valor de la macro.

El primer paso del compilador es reemplazar todas las partes del programa donde hay declaradas macros por los valores de dichas macros.

El uso excesivo de macros puede tornar muy difícil el mantenimiento de programas.

# Problema 1-Definir una macro para indicar el tamaño del vector
Se desea guardar los sueldos de 5 [[Estructura de datos tipo vector (elementos int y float)|operarios]] .  
Desarrollar dos funciones una donde se los ingrese por teclado y otra función donde se los imprima.  
Definir una macro para indicar el tamaño del vector.
## Codigo
```ejercicio129.c
#include<stdio.h>

#define TAMANO 5
#define FINPROGRAMA "Gracias por utilizar nuestro programa"

void carga(float sueldo[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
    printf("Ingrese los sueldos:");
    scanf("%f", &sueldo[i]);
    }
}

void imprimir(float sueldo[TAMANO])
{
    int i;
    printf("Lista de sueldos:");
    for ( i = 0; i < TAMANO; i++)
    {
        printf("%0.2f\n", sueldo[i]);
    }
}

int main()
{
    float sueldo[TAMANO];
    carga(sueldo);
    imprimir(sueldo);
    printf(FINPROGRAMA);
    return 0;
}
```
## Explicacion

En este programa hemos definido dos macros llamadas TAMANO y FINPROGRAMA:

```
#define TAMANO 5
#define FINPROGRAMA "Gracias por utilizar nuestro programa"
```
En la función main definimos un vector cuyo subíndice disponemos el nombre de la macro y un printf que pasamos en su parámetro la macro FINPROGRAMA:

```
int main()
{
    float sueldo[TAMANO];
    carga(sueldo);
    imprimir(sueldo);
    printf(FINPROGRAMA);
    return 0;
}
```

Hay que tener en cuenta que cuando compilamos este programa lo primero que hace es sustituir todas las macros por los valores de las macros, es decir la función main en la primer etapa de sustitución queda igual a:

```
int main()
{
    float sueldos[5];
    cargar(sueldos);
    imprimir(sueldos);
    printf("Gracios por utilizar nuestro programa");
    return 0;
}
```

Es decir el subíndice del vector es sustituido por un 5 y el parámetro del printf es sustituido por el valor "Gracias por utilizar nuestro programa"

En las otras funciones también utilizamos la macro TAMANO:

```
void carga(float sueldo[TAMANO])
{
    int i;
    for (i=0; i<TAMANO; i++)
    {
    printf("Ingrese los sueldos:");
    scanf("%f", &sueldo[i]);
    }
}
```

```
void imprimir(float sueldo[TAMANO])
{
    int i;
    printf("Lista de sueldos:");
    for ( i = 0; i < TAMANO; i++)
    {
        printf("%0.2f\n", sueldo[i]);
    }
}
```

**Como ventaja si tenemos que modificar nuestro programa y trabajar con los sueldos de 8 empleados el único cambio que tenemos que hacer es modificar el valor de la macro TAMANO.**
# Problema Complejo
Se tiene la siguiente información:  
- Nombres de 4 empleados ([[Estructura de datos tipo matriz (elementos char)|matriz de tipo char]])  
- Ingresos en concepto de sueldo, cobrado por cada empleado, en los últimos 3 meses ([[Estructura de datos tipo matriz (elementos int y float)|matriz de tipo float]])  
Confeccionar el programa para:  
1. Realizar la carga de la información mencionada.  
2. Generar un vector que contenga el ingreso acumulado en sueldos en los últimos 3 meses para cada empleado. 
3. Mostrar por pantalla el total pagado en sueldos a todos los empleados en los últimos 3 meses  
4. Obtener el nombre del empleado que tuvo [[Estructura de datos tipo matriz (elementos int y float)#Problema 5-imprimir el elemento mayor.|el mayor ingreso acumulado]]
Utilizar macros para definir la cantidad de filas y columnas de las estructuras de datos. 
![[Pasted image 20230215230412.png]]

## Codigo
``` Ejercicio130.c
#include<stdio.h>
#include<string.h>

#define CANTEMPLEADOS 4
#define MESES 3
#define ESPACIO "__________________________________________________\n"

void cargar(char empleado[CANTEMPLEADOS][41], float sueldo[CANTEMPLEADOS][MESES])
{
    int i,j;
    printf("Ingreso de nombre de empleado y sueldos\n");
    for ( i = 0; i < CANTEMPLEADOS; i++)
    {
        printf("Ingrese el nombre del empleado:");
        gets(empleado[i]);
        for ( j = 0; j < MESES; j++)
        {
            printf("Ingrese sueldo: ");
            scanf("%f", &sueldo[i][j]);
            fflush(stdin);
        }
    }
}

void sumarSueldoTotal(float sueldo[CANTEMPLEADOS][MESES], float sueldoTotal[MESES])
{
    int i,j;
    float suma;
    for ( i = 0; i < CANTEMPLEADOS; i++)
    {
        suma = 0;
        for ( j = 0; j < MESES; j++)
        {
            suma = suma + sueldo[i][j];
        }
        sueldoTotal[i] = suma;
    }
}

void imprimirSueldo(char empleado[CANTEMPLEADOS][41], float sueldoTotal[MESES])
{
    int i,j;
    printf("Sueldo total\n");
    for ( i = 0; i < CANTEMPLEADOS; i++)
    {
        printf("%s - %0.2f\n", empleado[i], sueldoTotal[i]);
    }
}

void mayorIngreso(char empleado[CANTEMPLEADOS][41], float sueldoTotal[MESES])
{
    int i,j;
    float mayor=sueldoTotal[0];
    char auxEmpleado[41];
    strcpy(auxEmpleado,empleado[0]);
    for ( i = 0; i < CANTEMPLEADOS-1; i++)
    {
        if(sueldoTotal[i]>mayor)
        {
            mayor=sueldoTotal[i];
            strcpy(auxEmpleado,empleado[i]);
        }
    }
     printf("%s Tuvo el mayor ingreso con %f\n",auxEmpleado,mayor);
}

int main()
{
    char empleado[CANTEMPLEADOS][41];
    float sueldo [CANTEMPLEADOS][MESES];
    float sueldoTotal[CANTEMPLEADOS];
    cargar(empleado,sueldo);
    printf(ESPACIO);
    sumarSueldoTotal(sueldo,sueldoTotal);
    imprimirSueldo(empleado,sueldoTotal);
    printf(ESPACIO);
    mayorIngreso(empleado,sueldoTotal);
    printf(ESPACIO);
    return 0;
}
```