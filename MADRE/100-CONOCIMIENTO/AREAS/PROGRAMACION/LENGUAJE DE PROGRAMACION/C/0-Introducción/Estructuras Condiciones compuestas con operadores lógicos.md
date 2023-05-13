Hasta ahora hemos visto los operadores:

relacionales (>, <, >=, <= , = =, !=)
matemáticos (+, -, *, /, %)

pero nos están faltando otros operadores imprescindibles:

lógicos (&&, ||).

Estos dos operadores se emplean fundamentalmente en las estructuras condicionales para agrupar varias condiciones simples.
# Operador &&
![[Pasted image 20230206184211.png]]
Traducido se lo lee como “Y”. Si la Condición 1 es verdadera Y la condición 2 es verdadera luego ejecutar la rama del verdadero.  
Cuando vinculamos dos condiciones con el operador “&&”, las dos condiciones deben ser verdaderas para que el resultado de la condición compuesta de Verdadero y continúe por la rama del verdadero de la estructura condicional.  

La utilización de operadores lógicos permiten en muchos casos plantear algoritmos más cortos y comprensibles.
## Problema
Confeccionar un programa que lea por teclado tres números distintos y nos muestre el mayor.
### Diagrama de flujo
![[Pasted image 20230206184251.png]]
Este ejercicio está resuelto sin emplear operadores lógicos en un concepto anterior del tutorial.  
La primera estructura condicional es una ESTRUCTURA CONDICIONAL COMPUESTA con una CONDICION COMPUESTA.  
Podemos leerla de la siguiente forma:  
Si el contenido de la variable num1 es mayor al contenido de la variable num2 Y si el contenido de la variable num1 es mayor al contenido de la variable num3 entonces la CONDICION COMPUESTA resulta Verdadera.  
Si una de las condiciones simples da falso la CONDICION COMPUESTA da Falso y continua por la rama del falso.  
Es decir que se mostrará el contenido de num1 si y sólo si num1 > num2 y num1 > num3.  
En caso de ser Falsa la condición, analizamos el contenido de num2 y num3 para ver cual tiene un valor mayor.  
En esta segunda estructura condicional no se requieren operadores lógicos al haber una condición simple.
## Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    int num1,num2,num3;
    printf("Ingrese primer valor:");
    scanf("%i",&num1);
    printf("Ingrese segundo valor:");
    scanf("%i",&num2);
    printf("Ingrese tercer valor:");
    scanf("%i",&num3);
    if (num1 > num2 && num1 > num3)
    {
        printf("%i",num1);
    }
    else
    {
        if (num2 > num3)
        {
            printf("%i",num2);
        }
        else
        {
            printf("%i",num3);
        }
    }
    getch();
    return 0;
}
```
# Operador ||
![[Pasted image 20230206184402.png]]
Traducido se lo lee como “O”. Si la condición 1 es Verdadera o la condición 2 es Verdadera, luego ejecutar la rama del Verdadero.  
Cuando vinculamos dos o más condiciones con el operador “Or" que en C se escribe con los caracteres ||, con que una de las dos condiciones sea Verdadera alcanza para que el resultado de la condición compuesta sea Verdadero.
## Problema
Se carga una fecha (día, mes y año) por teclado. Mostrar un mensaje si corresponde al primer trimestre del año (enero, febrero o marzo) Cargar por teclado el valor numérico del día, mes y año.  
Ejemplo: dia:10 mes:2 año:2017.
### Diagrama de flujo
![[Pasted image 20230206184435.png]]
La carga de una fecha se hace por partes, ingresamos las variables dd, mm y aa. (no podemos definir variables acentuadas o con la letra eñe)  
Mostramos el mensaje "Corresponde al primer trimestre" en caso que el mes ingresado por teclado sea igual a 1, 2 ó 3.  
En la condición no participan las variables dd y aa.
## Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    int dd,mm,aa;
    printf("Ingrese nro de dia:");
    scanf("%i",&dd);
    printf("Ingrese nro de mes:");
    scanf("%i",&mm);
    printf("Ingrese nro de año:");
    scanf("%i",&aa);
    if (mm==1 || mm==2 || mm==3)
    {
        printf("Corresponde al primer trimestre");
    }
    getch();
    return 0;
}
```