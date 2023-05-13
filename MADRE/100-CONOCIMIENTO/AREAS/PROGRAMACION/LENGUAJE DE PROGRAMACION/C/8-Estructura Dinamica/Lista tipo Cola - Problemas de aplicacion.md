	# Planteo de problema:
Este Practico tiene por objetivo mostrar la importancia de las [[Listas tipo cola|colas]] en las 
Ciencias de la Computacion y mas precisamente en las simulaciones.

Las simulasciones permiten analizar situaciones de la realidad sin la necesidad de ejecutarlas realmente. Tiene el beneficio que su costo es muy inferior a hacer pruebas en la realidad.

Desarollar un programa para la simulacion de un cajero autormatico:
Se cuenta con la siguiente informacion:
- Llegan clientes a la puerta del cajero cada 2 o 3 minutos.(promedio 2.5min)
- Cada cliente tarda entre 2 y 5 minitos para ser atendido.(promedio 3.5min)

Obtener la siguiente informacion:
1. Cantidad de clientes que se atienden en 10 horas(600min)
2. Cantidad de clientes que hay en cola despues de las 10 horas.
3. Hora de llegada del primer  cliente que no es atendido luego de 10 horas ( es decir, la persona que esta primera en la cola cuando se cumplen 10 horas)

struct cliente 
{
int *min  
}

## Codigo
```c
//Ejercicio166.c
 s 
```
## Explicacion

Par administrar la estructura cola definimos las funciones de insertar, extraer, vacía, cantidad y liberar.

La función donde implementamos la lógica es en la función de simulacion, veamos las distintas partes:

    srand(time(NULL));
    int estado = 0;
    int llegada = rand() % 2 + 2;
    int salida = -1;
    int cantAtendidas = 0;
    int minuto;

Iniciamos la semilla de valores aleatorios llamando a srand y pasando el valor devuelto por la función time que se encuentra en el archivo de inclusión time.h.

La variable estado almacena un cero si el cajero está libre y un uno cuando está ocupado.  
La variable llegada almacena en que minuto llegará el próximo cliente (debemos generar un valor entre 2 y 3)

La variable salida almacenará en que minuto terminará el cliente de ser atendido (como al principio el cajero está vacío inicializamos esta variable con -1)

Llevamos un contador para saber la cantidad de personas atendidas (cantAtendidas)

Disponemos un for que se repita 600 veces (600 minutos o lo que es lo mismo 10 horas)  

    for (minuto = 0; minuto < 600; minuto++)

Dentro del for hay dos if fundamentales que verifican que sucede cuando llega una persona o cuando una persona se retira:

        if (llegada == minuto)
        {
            ............
        }
        if (salida == minuto)
        {
            ............
        }

Cuando llega una persona al cajero primero verificamos si el cajero está desocupado:

        if (llegada == minuto)
        {
            if (estado==0) {

Si está desocupado lo ocupamos cambiando el valor de la variable estado y generando en que minuto esta persona dejará el cajero (un valor aleatorio entre 2 y 4 minutos):

                estado = 1;
                salida = minuto + 2 + rand() % 3;

Si el cajero está ocupado procedemos a cargar dicha persona en la cola (insertamos el minuto que llega):

            else
            {
                insertar(minuto);
            }

Luego generamos el próximo minuto que llegará otra persona:

            llegada = minuto + 2 + rand() % 2;

El otro if importante es ver que sucede cuando sale la persona del cajero:

if (salida == minuto) {

Si sale una persona del cajero cambiamos el valor de la variable estado, incrementamos en uno el contador cantAtendidos y si la cola no está vacía extraemos una persona, cambiamos a uno la variable estado y generamos en que minuto dejará esta persona el cajero:

            estado = 0;
            cantAtendidas++;
            if (!vacia())
            {
                extraer();
                estado = 1;
                salida = minuto + 2 + rand() % 3;
            }

Fuera del for mostramos los tres resultados:

    printf("Atendidos:%i\n" ,cantAtendidas);
    printf("En cola:%i\n",cantidad());
    printf("Minuto llegada:%i",extraer());