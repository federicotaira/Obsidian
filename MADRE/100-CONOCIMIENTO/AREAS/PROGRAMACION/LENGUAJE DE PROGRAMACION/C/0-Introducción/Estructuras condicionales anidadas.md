Decimos que una estructura condicional es anidada cuando por la rama del verdadero o el falso de una estructura condicional hay otra estructura condicional.
![[Pasted image 20230206183232.png]]
El diagrama de flujo que se presenta contiene dos estructuras condicionales. La principal se trata de una [[estructuras condicionales compuestas|estructura condicional compuesta]] y la segunda es una [[estructuras condicionales simples|estructura condicional simple]] y está contenida por la rama del falso de la primer estructura.  
Es común que se presenten estructuras condicionales anidadas aún más complejas.
# Problema
Confeccionar un programa que pida por teclado tres notas de un alumno, calcule el promedio e imprima alguno de estos mensajes:  
Si el promedio es >=7 mostrar "Promocionado".  
Si el promedio es >=4 y <7 mostrar "Regular".  
Si el promedio es <4 mostrar "Reprobado".
## Diagrama de flujo
![[Pasted image 20230206183317.png]]
Analicemos el siguiente diagrama. Se ingresan tres valores por teclado que representan las notas de un alumno, se obtiene el promedio sumando los tres valores y dividiendo por 3 dicho resultado (Tener en cuenta que si el resultado es un valor real solo se almacena la parte entera).  
Primeramente preguntamos si el promedio es superior o igual a 7, en caso afirmativo va por la rama del verdadero de la estructura condicional mostramos un mensaje que indica "Promocionado" (con comillas indicamos un texto que debe imprimirse en pantalla).  
En caso que la condición nos de falso, por la rama del falso aparece otra estructura condicional, porque todavía debemos averiguar si el promedio del alumno es superior o igual a cuatro o inferior a cuatro.  
Estamos en presencia de dos estructuras condicionales compuestas.
# Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    int nota1,nota2,nota3,promedio;
    printf("Ingrese primer nota:");
    scanf("%i",&nota1);
    printf("Ingrese segunda nota:");
    scanf("%i",&nota2);
    printf("Ingrese tercer nota:");
    scanf("%i",&nota3);
    promedio=(nota1 + nota2 + nota3) / 3;
    if (promedio >= 7)
    {
        printf("Promocionado");
    }
    else
    {
        if (promedio >= 4)
        {
            printf("Regular");
        }
        else
        {
            printf("Reprobado");
        }
    }
    getch();
    return 0;
}
```
