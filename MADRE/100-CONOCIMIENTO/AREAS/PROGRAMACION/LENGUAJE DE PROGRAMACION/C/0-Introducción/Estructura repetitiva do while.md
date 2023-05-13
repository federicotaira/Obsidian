La estructura do while es otra estructura repetitiva del lenguaje C, la cual ejecuta al menos una vez su bloque repetitivo, a diferencia del [[Estructura repetitiva while|while]] o del [[Estructura repetitiva for|for]] que podían no ejecutar el bloque.  
Esta estructura repetitiva se utiliza cuando conocemos de antemano que por lo menos una vez se ejecutará el bloque repetitivo.  
La condición de la estructura está abajo del bloque a repetir, a diferencia del while o del for que está en la parte superior.
# Representacion Grafica
![[Captura de pantalla_20230206_185926.png]]
El bloque de operaciones se repite MIENTRAS la condición sea Verdadera.  
Si la condición retorna Falso el ciclo se detiene. En C, todos los ciclos repiten por verdadero y cortan por falso.  
Es importante analizar y ver que las operaciones se ejecutan como mínimo una vez.

# Problema
Escribir un programa que solicite la carga de un número entre 0 y 999, y nos muestre un mensaje de cuántos dígitos tiene el mismo. Finalizar el programa cuando se cargue el valor 0.
## Diagrama de flujo
![[Pasted image 20230206190052.png]]

# Codigo
```C
#include<stdio.h>
#include<conio.h>

int main()
{
     int valor;
     do {
         printf("Ingrese un valor entre 0 y 999 (0 finaliza):");
         scanf("%i",&valor);
         if (valor >= 100)
         {
             printf("Tiene 3 digitos.");
         }
         else
         {
             if (valor >= 10)
             {
                 printf("Tiene 2 digitos.");
             }
             else
             {
                 printf("Tiene 1 digito.");
             }
         }
         printf("\n");
     } while (valor != 0);
     getch();
     return 0;
}
```