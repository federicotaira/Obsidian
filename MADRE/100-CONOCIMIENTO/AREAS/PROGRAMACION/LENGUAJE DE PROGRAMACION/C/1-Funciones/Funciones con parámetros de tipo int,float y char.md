Una funcion puede recibir uno o mas parametros para que sea mas flexible y se adapte a varias situaciones.

Veremos primero funciones que reciban uno o mas paramentros tipo int, flot o char.

La sinstaxis cuando una funcion recibe parametros es:
(Valor que retornar) (nombre de la funcion)((Parametros de la funcion))
{
	(Altorimo)
}
Los parámetros se disponen luego del nombre de la funcion encerrado entre parentesis y separados por coma, algunos ejemplos:

```
void funcion1(int v1)
{
	....
}
void funcion2(int v1, int v2)
{
	....
}
void funcion3(float f1, int v1)
{
	...
}
void funcion4(char letra)
{
	...
}
```

## Problema
Confeccionar una funcion que reciba dos enteros e imprima el mayor de ellos. Llamar a la funcion desde la main cargando previamente 2 valores por teclado
## Codigo
```Ejercicio89.c
#include<stdio.h>
#include<conio.h>

void imprimirMayor(int v1,int v2)
{
    if (v1>v2)
    {
        printf("El mayor es %i",v1);
    }
    else
    {
        printf("El mayor es %i",v2);
    }
}


int main()
{
    int valor1,valor2;
    printf("Ingrese primer valor:");
    scanf("%i",&valor1);
    printf("Ingrese segundo valor:");
    scanf("%i",&valor2);
    imprimirMayor(valor1,valor2);
    getch();
    return 0;
}
```
En este problema se requiere implementar una función que reciba dos enteros y muestre el mayor de ellos. Los parámetros van después del nombre de la función entre paréntesis y separados por coma:

	void imprimirMayor(int v1,int v2)

Los parámetros de la función solo se pueden utilizar dentro de dicha función y luego que finaliza de ejecutarse dicha función los mismos se borran de la memoria.

Cuando llamamos a la función desde la main le debemos pasar dos variables enteras, las variables que le pasamos deben estar cargadas con información:

    imprimirMayor(valor1,valor2);

En el momento que se llama la función el contenido de valor1 se copia en el parámetro v1 y el contenido de valor2 se copia en el parámetro v2:
![[Pasted image 20230214095428.png]]
Si por algún motivo modificamos v1 o v2 dentro de la función las variables definidas en la main no cambian.

Los parámetros son la forma de comunicarse entre una función y otra.

Podemos inclusive desde la main llamar a la función inprimirMayor pasando directamente valores enteros:

imprimirMayor(70, 5);

Tengamos en cuenta que en este caso v1 recibe el número 70 y v2 recibe el número 5.
