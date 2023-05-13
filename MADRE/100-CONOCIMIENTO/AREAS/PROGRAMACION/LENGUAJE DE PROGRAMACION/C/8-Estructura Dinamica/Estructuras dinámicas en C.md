Conocemos algunas estructuras de datos como son los [[Estructura de datos tipo vector (elementos int y float)|Vectores]] , [[Estructura de datos tipo matriz (elementos int y float)|matrices]] y [[Estructura de datos tipo registro (struct)|registros]] . No son las únicas. Hay muchas situaciones donde utilizar alguna de estas estructuras nos proporcionará una solución muy ineficiente (cantidad de espacio que ocupa en memoria, velocidad de acceso a la información, etc.)

Ejemplo 1. Imaginemos que debemos realizar un editor de texto, debemos elegir la estructura de datos para almacenar en memoria las distintas líneas que el operador irá escribiendo. Una solución factible es utilizar una matriz de caracteres. Pero como sabemos debemos especificar la cantidad de filas y columnas que ocupará de antemano. Podría ser por ejemplo 2000 filas y 200 columnas. Con esta definición estamos reservando de antemano 800000 bytes de la memoria, no importa si el operador después carga una línea con 20 caracteres, igualmente ya se ha reservado una cantidad de espacio que permanecerá ociosa.

Tiene que existir alguna estructura de datos que pueda hacer más eficiente la solución del problema anterior.

Ejemplo 2. ¿Cómo estarán codificadas las planillas de cálculo? ¿Reservarán espacio para cada casilla de la planilla al principio? Si no la lleno, ¿lo mismo se habrá reservado espacio?  
Utilizar una matriz para almacenar todas las casillas de una planilla de cálculo seguro será ineficiente.

Bien, todos estos problemas y muchos más podrán ser resueltos en forma eficiente cuando conozcamos estas nuevas estructuras de datos ( [[Concepto de listas|Listas]] , árboles)

Para trabajar con estructuras dinámicas en C debemos conocer y manejar muy bien el concepto de punteros que hemos ido presentando.