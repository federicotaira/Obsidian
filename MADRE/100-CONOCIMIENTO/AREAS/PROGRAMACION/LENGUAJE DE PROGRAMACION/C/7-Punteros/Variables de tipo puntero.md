Un tema fundamental del lenguaje C es el manejo del concepto de punteros.

Los punteros en el lenguaje C son esenciales:

-   Para que un programa sea muy eficiente.
-   Modificar variables de tipo int, float, struct etc. en otras funciones.
-   Poder requerir y liberar memoria durante la ejecución del programa (hay muchas situaciones donde no sabemos cuanto espacio reservar)

# Definición de puntero.

Una variable de tipo puntero almacena la dirección de memoria de otra variable que puede ser [[Punteros a tipos de datos int y float| int, float]] , char, struct etc.

Es difícil en un principio entender y ver las ventajas que trae trabajar con punteros pero su uso es fundamental si vamos a trabajar con el lenguaje C. Recordemos que el lenguaje C se utiliza fundamentalmente para el desarrollo de software de base (sistemas operativos, drivers etc.)  
Por ejemplo si seleccionamos una función al azar del código fuente del sistema operativo Linux veremos que siempre utilizan punteros:
![[Pasted image 20230218194148.png]]
Los punteros son aquellas variables antecedidas por el caracter *.

