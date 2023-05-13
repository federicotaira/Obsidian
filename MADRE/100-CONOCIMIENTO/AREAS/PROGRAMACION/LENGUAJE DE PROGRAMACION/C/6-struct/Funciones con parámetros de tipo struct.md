Hemos visto anteriormente que una función puede recibir tipos de datos simples como int, char y float y lo que sucede es que se hace una copia en el parámetro.  
También hemos visto y trabajado con [[Vectores y matrices paralelas| parámetros de tipo vector y matriz]] y el funcionamiento es muy distinto a los tipos de datos simples. Si en la función modificamos el parámetro lo que sucede es que se modifica la variable que le pasamos desde la función main.

Ahora veamos como trata el lenguaje C los parámetros de tipo struct. Su funcionamiento es idéntico a los tipos de datos simples, es decir que se hace una copia del registro en el parámetro. No podemos modificar el registro que le enviamos a la función sino solo accederlo para consultarlo.
# Inicializar por asignación variables.
En el lenguaje C podemos definir e inicializar inmediatamente el contenido de una variable.
Si es de tipo **entera** :
```
int cantidad=0;
```
Si es de tipo **float** :
```
float altura=1.92;
```
Si es de tipo **char** :
```
char opcion='A';
```
Si es de tipo **vector de enteros** :
```
int vec[3]={10,20,30);
```
Si se trata de una **matriz de enteros** :
```
int mat[2][3]={{10,20,30},
               {40,50,50}};
```


Finalmente si se trata de una variable de tipo **registro** la forma de inicializar cada campo con valores es:

```
int main()
{
    struct producto pro={1,"naranjas",12.50};
    imprimir(pro);
    getch();
    return 0;
}
```
# Problema 
Se tiene la siguiente declaración de registro:
```
struct producto {
    int codigo;
    char descripcion[41];
    float precio;
}; //obligatorio el punto y coma
```
Definir una variable en la función main e inicializar por asignación los tres campos.  
Plantear una función que reciba el registro y lo imprima.
## Codigo
```Ejercicio133.c
#include<stdio.h>
#include<string.h>

struct producto
{
    int codigo;
    char descripcion[41];
    float precio;
};

void imprimir(struct producto p)
{
    printf("El codigo del producto :%i\n", p.codigo);
    printf("Descripcion:%s\n",p.descripcion);
    printf("Precio:%0.2f",p.precio);
}

int main()
{
    //Forma Corta

    struct producto pro ={1,"naranjas",12.50};

    //Forma larga
    /*struct producto pro;
    pro.codigo=1;
    strcpy(pro.descripcion,"naranjas");
    pro.precio=5.50;*/
    
    imprimir(pro);
    return 0; 
}
```
## Explicacion:
Definimos una variable de tipo producto:

    struct producto pro;

En este problema no se pide cargar por teclado los datos sino por asignación:

    pro.codigo=1;
    strcpy(pro.descripcion,"naranjas");
    pro.precio=12.50;

La impresión de los datos del registro lo queremos hacer en otra función por lo que debemos pasar el registro como parámetro:

    imprimir(pro);

La función imprimir debe tener en forma obligada un parámetro de tipo struct producto:

```
void imprimir(struct producto p)
{
    printf("Codigo del producto:%i\n",p.codigo);
    printf("Descripcion:%s\n",p.descripcion);
    printf("precio:%0.2f",p.precio);
}
```

El parámetro p recibe una copia exacta de la variable pro definida en la main.
![[Pasted image 20230217114245.png]]
# Problema 2
Se tiene la siguiente declaración de registro:

```
struct pais {
    char nombre[40];
    int cantidadhab;
};
```

Definir tres variables de tipo país e iniciarlas por asignación con la sintaxis:  

```
struct pais pais1={"Argentina",40000000};
```

Elaborar una función que reciba un parámetro de tipo pais y muestre por pantalla sus dos campos. Llamar a dicha función desde la main pasando en forma sucesiva las tres variables definidas.
## Codigo
```Ejercicio134.c
#include<stdio.h>
#include<string.h>

struct pais {
    char nombre[40];
    int cantidadhab;
};

void imprimir(struct pais pais)
{
    printf("Pais:%s Problacion:%i\n", pais.nombre,pais.cantidadhab);
}

int main()
{
    struct pais pais1={"Argentina",40000000};
    struct pais pais2={"Brasil",80000000};
    struct pais pais3={"Paraguay",5000000};
    imprimir(pais1);
    imprimir(pais2);
    imprimir(pais3);
    return 0;
}
```