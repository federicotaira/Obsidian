Estamos viendo en los últimos conceptos la metodología de programación estructurada. Buscamos dividir un problema en subproblemas y plantear funciones que los resuelvan.

Hemos visto que una función la definimos mediante un nombre y que puede recibir datos por medio de sus parámetros.

Los parámetros son la forma para que una función reciba datos para ser procesados. Ahora veremos otra característica de las funciones que es la de devolver un dato a quien invocó la función (recordemos que una función la podemos llamar desde la función main o inclusive desde otras funciones de nuestro programa)

# Problema
Confeccionar una función que le enviemos como parámetro el valor del lado de un cuadrado y nos retorne su superficie.
## Codigo
```problema96
#include<stdio.h>

int retornarSuperficie(int lado)
{

    int superficie = lado*lado;

    return superficie;

}
int main()
{

    int valor,sup;

    printf("Ingrese el valor del lado del cuadrado: ");

    scanf("%i", &valor);

    sup = retornarSuperficie(valor);

    printf("La superficie del cuadrado es %i", sup);

    return 0;

}
```
Cuando una función retorna un valor indicamos previo al nombre de la función el tipo de dato que retorna (puede ser alguno de los tipos de datos que conocemos: char, int, float):

	int retornarSuperficie(int lado)

En nuestro ejemplo la función retornarSuperficie devuelve un int.

La función retornarSuperficie recibe un parámetro llamado lado, definimos una variable local llamada superficie donde almacenamos el producto del parámetro lado por sí mismo.

La variable local superficie es la que retorna la función mediante la palabra clave return:

	int retornarSuperficie(int lado)
	{
	    int superficie=lado*lado;
	    return superficie;
	}

Hay que tener en cuenta que las variables locales (en este caso superficie) solo se pueden consultar y modificar dentro de la función donde se las define, no se tienen acceso a las mismas en la función main del programa o dentro de otra función.

Hay un cambio importante cuando llamamos o invocamos a una función que devuelve un dato:

    sup=retornarSuperficie(valor);

Es decir el valor devuelto por la función retornarSuperficie se almacena en la variable sup.

Es un error lógico llamar a la función retornarSuperficie y no asignar el valor a una variable:

	retornarSuperficie(valor);

El dato devuelto por la función(en nuestro caso la superficie del cuadrado) no se almacena y se pierde sin que podamos imprimirlo.

Si podemos utilizar el valor devuelto para pasarlo a otra función como por ejemplo a printf:

    printf("La superficie del cuadrado es %i",retornarSuperficie(valor));

La función retornarSuperficie devuelve un entero y se lo pasamos a la función printf para que lo muestre.

Ahora podemos entender porque la función main tiene obligatoriamente la palabra clave return al final devolviendo un 0. Es obligatorio que la función main retorne un int. El dato devuelto podrá ser capturado si nuestro programa es llamado desde otro programa.
# Problema2
Confeccionar una función que defina dos parámetros enteros y nos retorne el mayor.
## Codigo
```Ejercicio97.c
#include<stdio.h>
#include<conio.h>

int retornarMayor(int v1,int v2)
{
    int mayor;
    if (v1>v2)
    {
        mayor=v1;
    }
    else
    {
        mayor=v2;
    }
    return mayor;
}


int main()
{
    int valor1,valor2;
    printf("Ingrese el primer valor:");
    scanf("%i",&valor1);
    printf("Ingrese el segundo valor:");
    scanf("%i",&valor2);
    printf("El valor mayor es %i",retornarMayor(valor1,valor2));
    getch();
    return 0;
}
```
Estamos definiendo una función que recibe dos parámetros y retorna el mayor de ellos:

```
int retornarMayor(int v1,int v2)
{
    int mayor;
    if (v1>v2)
    {
        mayor=v1;
    }
    else
    {
        mayor=v2;
    }
    return mayor;
}
```

Si queremos podemos hacerla más sintética a esta función sin tener que guardar en una variable local el valor a retornar:

```
int retornarMayor(int v1,int v2)
{
    if (v1>v2)
    {
        return v1;
    }
    else
    {
        return v2;
    }
}
```

Cuando una función encuentra la palabra return no sigue ejecutando el resto de la función sino que sale a la línea del programa desde donde llamamos a dicha función.
# Problema 3
Elaborar una función que reciba tres enteros y nos retorne el valor promedio de los mismos
## Codigo
```Ejercicio 98.c
#include<stdio.h>
#include<conio.h>

int promedio(int v1, int v2, int v3)
{
    int promedio;
    promedio=(v1+v2+v3)/3;
    return promedio;
}
int main()
{
    int valor1,valor2,valor3;
    printf("Ingrese el primer valor: ");
    scanf("%i", &valor1);
    printf("Ingrese el segundo valor: ");
    scanf("%i", &valor2);
    printf("Ingrese el tercer valor: ");
    scanf("%i", &valor3);
    printf("el promedio de los 3 valores es de %i", promedio(valor1,valor2,valor3));
	    
	    return 0;

}
```