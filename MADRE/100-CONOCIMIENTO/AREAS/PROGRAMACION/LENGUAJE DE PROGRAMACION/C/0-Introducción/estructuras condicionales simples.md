Cuando se presenta la elección tenemos la opción de realizar una actividad o no realizar ninguna.  
Representación gráfica:
![[Captura de pantalla_20230204_125329.png]]
Podemos observar: El rombo representa la condición. Hay dos opciones que se pueden tomar. Si la condición da verdadera se sigue el camino del verdadero, o sea el de la derecha, si la condición da falsa se sigue el camino de la izquierda.  
Se trata de una estructura CONDICIONAL SIMPLE porque por el camino del verdadero hay actividades y por el camino del falso no hay actividades.  
Por el camino del verdadero pueden existir varias operaciones, entradas y salidas, inclusive ya veremos que puede haber otras estructuras condicionales.

# Problema
Ingresar el sueldo de una persona, si supera los 3000 pesos mostrar un mensaje en pantalla indicando que debe abonar impuestos.

## Diagrama de flujo
![[Pasted image 20230204125615.png]]
Podemos observar lo siguiente: Siempre se hace la carga del sueldo, pero si el sueldo que ingresamos supera 3000 pesos se mostrará por pantalla el mensaje "Esta persona debe abonar impuestos", en caso que la persona cobre 3000 o menos no aparece nada por pantalla.

# Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
    float sueldo;
    printf("Ingrese el sueldo:");
    scanf("%f",&sueldo);
    if (sueldo>3000)
    {
        printf("Esta persona debe abonar impuestos");
    }
    getch();
    return 0;
}
```
La palabra clave "==if==" indica que estamos en presencia de una estructura condicional; seguidamente disponemos la condición entre paréntesis. Por último encerrada entre llaves las instrucciones de la rama del verdadero.

Es necesario que las instrucciones a ejecutar en caso que la condición sea verdadera estén encerradas entre llaves { }, con ellas marcamos el comienzo y el fin del bloque del verdadero.

Ejecutando el programa e ingresando un sueldo superior a 3000. Podemos observar como aparece en pantalla el mensaje "Esta persona debe abonar impuestos", ya que la condición del if es verdadera:

![[Pasted image 20230204125758.png]]
