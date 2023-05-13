Cuando se presenta la elección tenemos la opción de realizar una actividad u otra. Es decir tenemos actividades por el verdadero y por el falso de la condición. Lo más importante que hay que tener en cuenta que se realizan las actividades de la rama del verdadero o las del falso, NUNCA se realizan las actividades de las dos ramas.
![[Pasted image 20230204125857.png]]
En una estructura condicional compuesta tenemos entradas, salidas, operaciones, tanto por la rama del verdadero como por la rama del falso.
# Problema
Realizar un programa que solicite al operador ingresar dos números y muestre por pantalla el mayor de ellos.
## Diagrama de flujo
![[Pasted image 20230204125931.png]]
Se hace la entrada de num1 y num2 por teclado. Para saber cual variable tiene un valor mayor preguntamos si el contenido de num1 es mayor (>) que el contenido de num2, si la respuesta es verdadera vamos por la rama de la derecha e imprimimos num1, en caso que la condición sea falsa vamos por la rama de la izquierda (Falsa) e imprimimos num2.  
Como podemos observar nunca se imprimen num1 y num2 simultáneamente.

Estamos en presencia de una ESTRUCTURA CONDICIONAL COMPUESTA ya que tenemos actividades por la rama del verdadero y del falso.
# Codigo
```C
#include<stdio.h>
#include<conio.h>

void main()
{
    int num1, num2;
    printf("Ingrese primer valor:");
    scanf("%i",&num1);
    printf("Ingrese segundo valor:");
    scanf("%i",&num2);
    if (num1 > num2)
    {
        printf("%i",num1);
    }
    else
    {
        printf("%i",num2);
    }
    getch();
    return 0;
}
```
Cotejemos el diagrama de flujo y la codificación y observemos que el primer bloque de llaves después del if representa la rama del verdadero y el segundo bloque de llaves representa la rama del falso.  

Compilemos el programa, si hubo errores sintácticos corrijamos y carguemos dos valores, como por ejemplo:

Ingrese el primer valor: 10
Ingrese el segundo valor: 4
10

Si ingresamos los valores 10 y 4 la condición del if retorna verdadero y ejecuta el primer bloque.  
Un programa se controla y corrige probando todos sus posibles resultados.  
Ejecutemos nuevamente el programa e ingresemos:

Ingrese el primer valor: 10
Ingrese el segundo valor: 54
54

Cuando a un programa le corregimos todos los errores sintácticos y lógicos ha terminado nuestra tarea y podemos entregar el mismo al USUARIO que nos lo solicitó.

