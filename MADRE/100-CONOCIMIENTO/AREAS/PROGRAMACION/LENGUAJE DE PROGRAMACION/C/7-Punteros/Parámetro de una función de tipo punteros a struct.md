Vimos que cuando pasamos una [[Funciones con parámetros de tipo struct| variable de tipo registro]] a una función lo que sucede es que se hace una copia en el parámetro.

Ahora veremos que si queremos modificar el registro en la función lo que debemos hacer es pasar la dirección del registro y que lo reciba un puntero.
# Problema 1:
Se tiene la siguiente declaración de registro:

```
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; 
```

Plantear una función que reciba la dirección de un registro y mediante esta modificar los campos de la variable que le pasamos desde la main.

Imprimir el registro definido en la main.
## Codigo
```ejercicio150.c
#include<stdio.h>

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
};
void cargar(struct producto *ppro)
{
    printf("Ingrese el codigo del producto: ");
    scanf("%i",&(*ppro).codigo);
    fflush(stdin);
    printf("Ingrese la Descripcion:");
    fgets((*ppro).descripcion,41,stdin);
    printf("Ingrese el precio del producto: ");
    scanf("%f", &(*ppro).precio);
}

void imprimir(struct producto prod)
{
    printf("Codigo del producto: %i\n", prod.codigo);
    printf("Descripcion: %s\n", prod.descripcion);
    printf("Precio del producto: %0.2f", prod.precio);
}

int main()
{
    struct producto prod;
    cargar(&prod);
    imprimir(prod);
    return 0;
}
```
## Explicacion
Como vemos cuando queremos modificar un registro en una función debemos pasar la dirección del registro desde la main y la función lo recibe mediante un puntero:

```
void cargar(struct producto *pprod)
```

Dentro de la función debemos acceder a los campos del registro mediante el puntero:

    scanf("%i",&(*pprod).codigo);

Por prioridad de operadores en el lenguaje C debemos encerrar entre paréntesis el nombre del puntero con el caracter asterisco.

#### Importante

Si bien en el lenguaje C podemos utilizar para acceder a los campos del registro la sintaxis anterior es mucho más común utilizar el operador -> para acceder a los campos (se escribe con el caracter menos seguido del caracter mayor)

Entonces el problema anterior quedaría codificado con la siguiente sintaxis:
```#include<stdio.h>
#include<conio.h>

struct producto {
    int codigo;
    char descripcion[41];
    float precio;
};

void cargar(struct producto *pprod)
{
    printf("Ingrese codigo:");
    scanf("%i",&pprod->codigo);
    fflush(stdin);
    printf("Ingrese descripcion:");
    gets(pprod->descripcion);
    printf("Ingrese precio:");
    scanf("%f",&pprod->precio);
}

void imprimir(struct producto prod)
{
    printf("Codigo:%i\n",prod.codigo);
    printf("Descripcion:%s\n",prod.descripcion);
    printf("Precio:%0.2f",prod.precio);
}


int main()
{
    struct producto prod;
    cargar(&prod);
    imprimir(prod);
    getch();
    return 0;
}
```
Esta es la sintaxis más usada para acceder a los campos de un registro mediante un puntero:

```
void cargar(struct producto *pprod)
{
    printf("Ingrese codigo:");
    scanf("%i",&pprod->codigo);
    fflush(stdin);
    printf("Ingrese descripcion:");
    gets(pprod->descripcion);
    printf("Ingrese precio:");
    scanf("%f",&pprod->precio);
}
```

Esta es la sintaxis más usada para acceder a los campos de un registro mediante un puntero:

```
void cargar(struct producto *pprod)
{
    printf("Ingrese codigo:");
    scanf("%i",&pprod->codigo);
    fflush(stdin);
    printf("Ingrese descripcion:");
    gets(pprod->descripcion);
    printf("Ingrese precio:");
    scanf("%f",&pprod->precio);
}
```

Nuevamente si analizamos alguna de las miles de funciones que contiene el sistema operativo Linux veremos que aparece el empleo de punteros a struct utilizando el operador **->** para acceder a los campos de un registro:
![[Pasted image 20230220123811.png]]
Es decir cuando analizamos si un programa utiliza punteros debemos considerar todas las líneas donde aparecen previo al nombre de la variable el asterico (*) o el operador ->

# Problema propuesto
Se tiene la siguiente declaración de registro:

```
struct pais {
    char nombre[40];
    int cantidadhab;
};
```

Definir tres variables de tipo país en la función main.  
Crear una función que reciba un puntero de tipo pais y cargue por teclado el nombre del país y la cantidad de habitantes.  
Mostrar en otra función los datos cargados de cada país.

## Codigo
```Ejercicio151.c
#include<stdio.h>

#include<conio.h>

  

struct pais {

    char nombre[40];

    int cantidadhab;

};

  

void cargar(struct pais *ppais)

{

    printf("Ingrese el nombre del pais:");

    gets(ppais->nombre);

    printf("Ingrese la cantidad de habitantes de dicho pais:");

    scanf("%i",&ppais->cantidadhab);

    fflush(stdin);

}

  

void imprimir(struct pais p)

{

    printf("Nombre del pais:%s y la cantidad de habitantes es: %i\n", p.nombre,p.cantidadhab);

}

  
  

int main()

{

    struct pais pais1, pais2, pais3;

    cargar(&pais1);

    cargar(&pais2);

    cargar(&pais3);

    imprimir(pais1);

    imprimir(pais2);

    imprimir(pais3);

    getch();

    return 0;

}
```