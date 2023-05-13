La sintaxis para definir una [[Variables de tipo puntero]] se logra antecediendo el caracter * al nombre de la variable:

```
int *pe;
float *pf;
```
# Problema 1
Definir dos variables enteras y almacenar valores por asignación. 
Definir una variable puntero a entero y guardar sucesivamente las direcciones de dichas dos variables y acceder a sus valores.
## Codigo 
```Ejercicio141.c
#include<stdio.h>
#include<conio.h>

int main()
{
    int valor1=10;
    int valor2=20;
    int *pe;
    pe=&valor1;
    printf("Lo apuntado por pe es:%i\n",*pe);
    printf("La direccion que almacena pe es:%p\n",pe);
    pe=&valor2;
    printf("Lo apuntado por pe es:%i\n",*pe);
    printf("La direccion que almacena pe es:%p\n",pe);
    getch();
    return 0;
}
```
## Explicacion
La ejecución de este programa tiene una salida similar a:
![[Pasted image 20230218194909.png]]

Haremos una serie de problemas para entender el concepto de una variable de tipo puntero que en principio no le encontraremos mayores ventajas a como veníamos resolviendo problemas. Lo más conveniente es empezar con problemas muy sencillos para luego ver la verdadera potencia que nos proporcionan las variables de tipo puntero.

En este problema definimos dos variables de tipo entera y las inicializamos con los valores 10 y 20:

```
    int valor1=10;
    int valor2=20;
```
Nosotros ya hemos visto que si queremos acceder e imprimir el contenido de la variable "valor1" lo hacemos por medio de su nombre:

```
printf("%i",valor1);
```

Hay otra forma de acceder al contenido de la variable "valor1" mediante el concepto de una [[Variables de tipo puntero]] . Definimos una tercer variable llamada pe (una variable puntero a entero):

```
    int *pe;
```

La variable "pe" puede almacenar una dirección de memoria, es incorrecto (no podemos asignar valor1 a pe):

```
~~pe=valor1;~~
```

**pe y valor1 son variables de distinto tipo, valor1 es una variable entera y pe es una variable que apunta a una dirección de memoria donde se almacena un entero.**

Para almacenar en la variable pe la dirección de memoria donde se almacena valor1 utilizamos el caracter &:

```
    pe=&valor1;
```

La línea anterior la podemos leer como: "en la variable pe almacenamos la dirección de memoria donde se almacena valor1".

Si vemos el contenido de la memoria podemos identificar dos variables "valor1" y "pe":
![[Pasted image 20230218194951.png]]
La variable "valor1" almacena el número entero 10. La variable "valor1" se almacena por ejemplo en la dirección de memoria 1000 (este valor es a modo de ejemplo y la dirección real sucede cuando se ejecuta el programa)

La variable "pe" puede almacenar la dirección de una variable entera, cuando hacemos la asignación:

```
    pe=&valor1;
```

Estamos guardando la dirección 1000 de la variable "pe".

Para imprimir lo apuntado por el puntero pe utilizamos la sintaxis (*pe), en nuestro caso accedemos al valor 10 amacenado en valor1:

   ```
 printf("Lo apuntado por pe es:%i\n",*pe);
```

Imprimir el contenido de pe puede tener poco sentido ya que veremos la dirección que almacena pe, en el caso que lo hagamos debemos indicar **%p** :

    printf("La direccion que almacena pe es:%p\n",pe);

Una variable de tipo puntero puede cambiar la dirección que almacena a lo largo de la ejecución del programa, luego si hacemos:

    pe=&valor2;

Estamos almacenando la dirección de la variable valor2. Si imprimimos lo apuntado por pe tendremos el número 20:

    printf("Lo apuntado por pe es:%i\n",*pe);

# Problema 2
Definir dos variables enteras y no inicializarlas.  
Definir una variable puntero a entero, hacer que apunte sucesivamente a las dos variables enteras definidas previamente y cargue sus contenidos.  
Imprimir las dos variables enteras.
## Codigo
```
#include<stdio.h>
int main()
{
    int valor1, valor2;
    int *pe;
    pe = &valor1;
    *pe = 100;
    pe = &valor2;
    *pe = 200;
    printf("Primer variable entera:%i\n",valor1);
    printf("segunda variable entera:%i\n",valor2);
    return 0;
}
```
## Explicacion
Definimos dos variable enteras y no les cargamos valor:

    int x1,x2;

Definimos una variable puntero a entero:

    int *pe;

Almacenamos en la variable puntero la dirección de la variable x1:

    pe=&x1;

Modificamos el contenido de x1 accediendo a su contenido mediante el puntero pe:

    *pe=100;

Modificamos el contenido de la variable puntero pe con la dirección de la variable x2:

    pe=&x2;

Modificamos ahora el contenido de x2 accediendo por medio de puntero pe:

    *pe=200;

Imprimimos los contenidos de x1 y x2:

    printf("Primer variable entera:%i\n",x1);
    printf("Segunda variable entera:%i\n",x2);

Entiendo que este programa es más eficiente si directamente cargamos x1 y x2 asignándole las valores 100 y 200, pero tengamos presente que lo más importante es entender el concepto de punteros  la sintaxis para obtener la dirección de una variable (&) y acceder a lo apuntado por medio de (*)
# Problema 3
-   Se tienen el siguiente programa:
   ```
	#include<stdio.h>
    #include<conio.h>
    
    int main()
    {
        char c1='A';
        char c2='B';
        char *pc;
        pc=&c1;
        printf("%c\n",c1); //se imprime: ?
        *pc='a';
        printf("%c\n",c1); //se imprime: ?
        c1='Z';
        printf("%c\n",*pc); //se imprime: ?
        pc=&c2;
        printf("%c\n",*pc); //se imprime: ?
        getch();
        return 0;
    }
```
    
Indicar que valor se imprime en cada llamada a printf.
![[Pasted image 20230218232348.png]]

# Problema 4
Se tienen el siguiente programa:
```
    #include<stdio.h>
    #include<conio.h>
    int main()
    {
        int f;
        int *pe;
        pe=&f;
        for(*pe=1;*pe<=10;*pe=*pe+1)
        {
            printf("%i\n",f); //se imprime ?
        }
        getch();
        return 0;
    }
```    
 Indicar que valor se imprime en cada llamada a printf.
# Problema 5
-   Se tienen el siguiente programa: 
  ```
    #include <stdio.h>
    #include<conio.h>
    
    int main()
    {
      float z1,z2;
      float *pf;
      pf=&z1;
      *pf=10.2;
      pf=&z2;
      *pf=20.90;
      printf("%0.2f %0.2f",z1,z2); // Se imprime ?
      getch();
      return 0;
    }
```
    
Indicar que valor se imprime en la llamada a printf.
