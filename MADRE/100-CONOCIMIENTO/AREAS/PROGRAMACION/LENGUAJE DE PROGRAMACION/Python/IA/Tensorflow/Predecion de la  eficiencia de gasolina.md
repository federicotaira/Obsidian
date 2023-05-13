En un problema de _regresion_, buscamos predecir la salida de un valor continuo como la probabilidad de un precio. En contraste en un problema de _Clasificacion_, buscamos seleccionar una clase de una lista de clases (por ejemplo, en donde una imagen contenga una manzana o una naranja queremos reconocer cual es la fruta en la imagen).

Este libro usa el set de datos clasico [Auto MPG](https://archive.ics.uci.edu/ml/datasets/auto+mpg) y construye un modelo para predecir la eficiencia de vehiculos de 1970 y 1980. Para hacer esto proveeremos el modelo con una descripcion de muchos automoviles de ese periodo. Esta descripcion incluye atributos como: Cilindros, desplazamiento, potencia y peso.

Este ejemplo usa el API `tf.keras` , revise [Esta Guia](https://www.tensorflow.org/guide/keras?hl=es-419) para obtener mas detalles.

```
# Use seaborn for pairplot

pip install -q seaborn
```
```
import pathlib

  

import matplotlib.pyplot as plt

import pandas as pd

import seaborn as sns

  

import tensorflow as tf

  

from tensorflow import keras

from tensorflow.keras import layers

  

print(tf.__version__)
```

# Set de datos de MPG
el set de datos esta disponible de el siguiente repositorio [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/).

# Obtener el dato
Primero descargue el set de datos.
```
dataset_path = keras.utils.get_file("auto-mpg.data", "http://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data")

dataset_path
```
Importelo usando pandas.
```
column_names = ['MPG','Cylinders','Displacement','Horsepower','Weight',

                'Acceleration', 'Model Year', 'Origin']

raw_dataset = pd.read_csv(dataset_path, names=column_names,

                      na_values = "?", comment='\t',

                      sep=" ", skipinitialspace=True)

  

dataset = raw_dataset.copy()

dataset.tail()
```

# Limpiar Datos
El set de datos contiene algunos valores desconocidos
```
dataset.isna().sum()
```
```
MPG 0 
Cylinders 0
Displacement 0
Horsepower 6
Weight 0
Acceleration 0
Model Year 0
Origin 0
dtype: int64
```
**Para** Mantener este tutorial inicial sencillo eliminemos las siguientes filas.
```
dataset = dataset.dropna()
```
La columna de `"Origin"` realmente es categorica, no numerica. Entonces conviertala a un "one-hot":
```
origin = dataset.pop('Origin')
```
```
dataset['USA'] = (origin == 1)*1.0

dataset['Europe'] = (origin == 2)*1.0

dataset['Japan'] = (origin == 3)*1.0

dataset.tail()
```
![[Pasted image 20230210122750.png]]

# Dividir la data en entrenamiento y prueba

Ahora divida el set de datos en un set de entrenamiento y otro de pruebas.
Usaremos el set de pruebas en la evaluacion final de nuestro modelo.
```
train_dataset = dataset.sample(frac=0.8,random_state=0)

test_dataset = dataset.drop(train_dataset.index)
```
## Inspeccionar la data
Revise rapidamente la distribucion conjunta de un par de columnas de el set de entrenamiento.
```
sns.pairplot(train_dataset[["MPG", "Cylinders", "Displacement", "Weight"]], diag_kind="kde")

```
![[Pasted image 20230210123000.png]]
Tambien revise las estadisticas generales:
```
train_stats = train_dataset.describe()

train_stats.pop("MPG")

train_stats = train_stats.transpose()

train_stats
```
![[Pasted image 20230210123031.png]]
## Separar las caracteristicas de las etiquetas
Separe el valor objetivo, o la "etiqueta" de las caracteristicas. Esta etiqueta es el valor que entrenara el modelo para predecir.
```
train_labels = train_dataset.pop('MPG')

test_labels = test_dataset.pop('MPG')
```
## Normalizar la data
Revise otra vez el bloque de `train_stats` que se presento antes y note la diferencia de rangos de cada caracteristica.

Es una buena práctica normalizar funciones que utilizan diferentes escalas y rangos. Aunque el modelo * podría * converger sin normalización de características, dificulta el entrenamiento y hace que el modelo resultante dependa de la elección de las unidades utilizadas en la entrada.

Nota: Aunque generamos intencionalmente estas estadísticas solo del conjunto de datos de entrenamiento, estas estadísticas también se utilizarán para normalizar el conjunto de datos de prueba. Necesitamos hacer eso para proyectar el conjunto de datos de prueba en la misma distribución en la que el modelo ha sido entrenado.
```
def norm(x):

  return (x - train_stats['mean']) / train_stats['std']

normed_train_data = norm(train_dataset)

normed_test_data = norm(test_dataset)
```
Estos datos normalizados es lo que usaremos para entrenar el modelo.

Precaución: las estadísticas utilizadas para normalizar las entradas aquí (media y desviación estándar) deben aplicarse a cualquier otro dato que se alimente al modelo, junto con la codificación de un punto que hicimos anteriormente. Eso incluye el conjunto de pruebas, así como los datos en vivo cuando el modelo se usa en producción.
# El modelo

## Construye el modelo

Construyamos nuestro modelo. Aquí, utilizaremos un modelo `secuencial` con dos capas ocultas densamente conectadas y una capa de salida que devuelve un único valor continuo. Los pasos de construcción del modelo se envuelven en una función, `build_model`, ya que crearemos un segundo modelo, más adelante.
```
def build_model():

  model = keras.Sequential([

    layers.Dense(64, activation='relu', input_shape=[len(train_dataset.keys())]),

    layers.Dense(64, activation='relu'),

    layers.Dense(1)

  ])

  

  optimizer = tf.keras.optimizers.RMSprop(0.001)

  

  model.compile(loss='mse',

                optimizer=optimizer,

                metrics=['mae', 'mse'])

  return model
```
```
model = build_model()
```
## Inpeccionar el modelo
Use el método `.summary` para imprimir una descripción simple del modelo
```
model.summary()
```
![[Pasted image 20230210124756.png]]
Ahora pruebe el modelo. Tome un lote de ejemplos `10` de los datos de entrenamiento y llame a`model.predict` en él.
![[Pasted image 20230210124823.png]]
Parece estar funcionando, y produce un resultado de la forma y tipo esperados.
# Entrenar el modelo
Entrene el modelo durante 1000 épocas y registre la precisión de entrenamiento y validación en el objeto `history`.
```
# Display training progress by printing a single dot for each completed epoch

class PrintDot(keras.callbacks.Callback):

  def on_epoch_end(self, epoch, logs):

    if epoch % 100 == 0: print('')

    print('.', end='')

  

EPOCHS = 1000

  

history = model.fit(

  normed_train_data, train_labels,

  epochs=EPOCHS, validation_split = 0.2, verbose=0,

  callbacks=[PrintDot()])
```
Visualice el progreso de entrenamiento del modelo usando las estadísticas almacenadas en el objeto `history`.
```
hist = pd.DataFrame(history.history)

hist['epoch'] = history.epoch

hist.tail()
```
![[Pasted image 20230210125015.png]]
```
def plot_history(history):

  hist = pd.DataFrame(history.history)

  hist['epoch'] = history.epoch

  

  plt.figure()

  plt.xlabel('Epoch')

  plt.ylabel('Mean Abs Error [MPG]')

  plt.plot(hist['epoch'], hist['mae'],

           label='Train Error')

  plt.plot(hist['epoch'], hist['val_mae'],

           label = 'Val Error')

  plt.ylim([0,5])

  plt.legend()

  

  plt.figure()

  plt.xlabel('Epoch')

  plt.ylabel('Mean Square Error [$MPG^2$]')

  plt.plot(hist['epoch'], hist['mse'],

           label='Train Error')

  plt.plot(hist['epoch'], hist['val_mse'],

           label = 'Val Error')

  plt.ylim([0,20])

  plt.legend()

  plt.show()

  
  

plot_history(history)
```
![[Pasted image 20230210125046.png]]
Este gráfico muestra poca mejora, o incluso degradación en el error de validación después de aproximadamente 100 épocas. Actualicemos la llamada `model.fit` para detener automáticamente el entrenamiento cuando el puntaje de validación no mejore. Utilizaremos una * devolución de llamada de EarlyStopping * que pruebe una condición de entrenamiento para cada época. Si transcurre una cantidad determinada de épocas sin mostrar mejoría, entonces detiene automáticamente el entrenamiento.

Puedes obtener más información sobre esta devolución de llamada [Aca](https://www.tensorflow.org/versions/master/api_docs/python/tf/keras/callbacks/EarlyStopping?hl=es-419).
```
model = build_model()

  

# The patience parameter is the amount of epochs to check for improvement

early_stop = keras.callbacks.EarlyStopping(monitor='val_loss', patience=10)

  

history = model.fit(normed_train_data, train_labels, epochs=EPOCHS,

                    validation_split = 0.2, verbose=0, callbacks=[early_stop, PrintDot()])

  

plot_history(history)
```
![[Pasted image 20230210125207.png]]
El gráfico muestra que en el conjunto de validación, el error promedio generalmente es de alrededor de +/- 2 MPG. ¿Es esto bueno? Le dejaremos esa decisión a usted.

Veamos qué tan bien generaliza el modelo al usar el conjunto ** test **, que no usamos al entrenar el modelo. Esto nos dice qué tan bien podemos esperar que el modelo prediga cuándo lo usamos en el mundo real.
```
loss, mae, mse = model.evaluate(normed_test_data, test_labels, verbose=2)

  

print("Testing set Mean Abs Error: {:5.2f} MPG".format(mae))
```
![[Pasted image 20230210125242.png]]
# Hacer predicciones
Finalmente, prediga los valores de MPG utilizando datos en el conjunto de pruebas:
```
test_predictions = model.predict(normed_test_data).flatten()

  

plt.scatter(test_labels, test_predictions)

plt.xlabel('True Values [MPG]')

plt.ylabel('Predictions [MPG]')

plt.axis('equal')

plt.axis('square')

plt.xlim([0,plt.xlim()[1]])

plt.ylim([0,plt.ylim()[1]])

_ = plt.plot([-100, 100], [-100, 100])
```
![[Pasted image 20230210125319.png]]
Parece que nuestro modelo predice razonablemente bien. Echemos un vistazo a la distribución de errores.
```
error = test_predictions - test_labels

plt.hist(error, bins = 25)

plt.xlabel("Prediction Error [MPG]")

_ = plt.ylabel("Count")
```
![[Pasted image 20230210125349.png]]
# Conclusion

Este cuaderno introdujo algunas técnicas para manejar un problema de regresión.

-   El error cuadrático medio (MSE) es una función de pérdida común utilizada para problemas de regresión (se utilizan diferentes funciones de pérdida para problemas de clasificación).
-   Del mismo modo, las métricas de evaluación utilizadas para la regresión difieren de la clasificación. Una métrica de regresión común es el error absoluto medio (MAE).
-   Cuando las características de datos de entrada numéricos tienen valores con diferentes rangos, cada característica debe escalarse independientemente al mismo rango.
-   Si no hay muchos datos de entrenamiento, una técnica es preferir una red pequeña con pocas capas ocultas para evitar el sobreajuste.
-   La detención temprana es una técnica útil para evitar el sobreajuste.