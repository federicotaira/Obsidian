1. Abre Anaconda Prompt en tu sistema.
2. Crea un nuevo entorno con la versión de Python que necesitas y activa el entorno:
```
conda create -n mi_nuevo_entorno python=X.X
conda activate mi_nuevo_entorno

```
3. Exporta la lista de paquetes instalados en el entorno base de Anaconda a un archivo YAML:
```
conda env export --name base > base.yml
```
4.  Abre el archivo base.yml con tu editor de texto preferido y busca la sección que comienza con `dependencies:`.
   
5.  Copia la lista de paquetes de la sección `dependencies` y pégala en el archivo environment.yml de tu nuevo entorno. Asegúrate de que los nombres de los paquetes sean los mismos que aparecen en la lista.
  
6.  Instala los paquetes en tu nuevo entorno con el siguiente comando:
```
conda env update --name mi_nuevo_entorno --file environment.yml
```
7. Verifica que los paquetes se hayan instalado correctamente en el nuevo entorno:

```
conda list
```