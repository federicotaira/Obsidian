Este cuaderno clasifica las reseñas de películas como _positivas_ o _negativas_ usando el texto de la reseña. Este es un ejemplo de [[Clasificacion de textos]] —o de dos clases—, un tipo de problema de aprendizaje automático importante y ampliamente aplicable.
Utiliza el conjunto de [datos de IMDB](https://www.tensorflow.org/api_docs/python/tf/keras/datasets/imdb?hl=es-419) que contiene el texto de 50.000 reseñas de películas de [Internet Movie Database](https://www.imdb.com/) . Estos se dividen en 25 000 revisiones para capacitación y 25 000 revisiones para pruebas. Los conjuntos de entrenamiento y prueba están _equilibrados_ , lo que significa que contienen la misma cantidad de reseñas positivas y negativas.
Este cuaderno usa [`tf.keras`](https://www.tensorflow.org/guide/keras?hl=es-419) , una API de alto nivel para crear y entrenar modelos en TensorFlow, y [`tensorflow_hub`](https://www.tensorflow.org/hub?hl=es-419) , una biblioteca para cargar modelos entrenados desde [TFHub](https://tfhub.dev/) en una sola línea de código. Para obtener un tutorial de clasificación de texto más avanzado con [`tf.keras`](https://www.tensorflow.org/api_docs/python/tf/keras?hl=es-419) , consulte la [Guía de clasificación de texto de MLCC](https://developers.google.com/machine-learning/guides/text-classification/?hl=es-419) .

```
$ pip install tensorflow-hub
$ pip install tensorflow-datasets
```
```
import os

import numpy as np 

import tensorflow as tf

import tensorflow_hub as hub

import tensorflow_datasets as tfds  

print("Version: ", tf.__version__)

print("Eager mode: ", tf.executing_eagerly())

print("Hub version: ", hub.__version__)

print("GPU is", "available" if tf.config.list_physical_devices("GPU") else "NOT AVAILABLE")
```
```
Version: 2.9.2 
Eager mode: True 
Hub version: 0.12.0 
GPU is available
```
# Descargar el conjunto de datos de IMDB

El conjunto de datos de IMDB está disponible en las [revisiones de imdb](https://www.tensorflow.org/datasets/catalog/imdb_reviews?hl=es-419) o en los conjuntos de [datos de TensorFlow](https://www.tensorflow.org/datasets?hl=es-419) . El siguiente código descarga el conjunto de datos IMDB a su máquina (o el tiempo de ejecución de colab):
```
# Split the training set into 60% and 40% to end up with 15,000 examples

# for training, 10,000 examples for validation and 25,000 examples for testing.

train_data, validation_data, test_data = tfds.load(

    name="imdb_reviews",

    split=('train[:60%]', 'train[60%:]', 'test'),

    as_supervised=True)
```
# Exporar los datos
Tomemos un momento para comprender el formato de los datos. Cada ejemplo es una oración que representa la reseña de la película y una etiqueta correspondiente. La oración no está preprocesada de ninguna manera. La etiqueta es un valor entero de 0 o 1, donde 0 es una reseña negativa y 1 es una reseña positiva.

Imprimamos los primeros 10 ejemplos.
```
train_examples_batch, train_labels_batch = next(iter(train_data.batch(10)))

train_examples_batch
```
```
Imprimamos también las primeras 10 etiquetas.
```
# Contruir el modelo
La red neuronal se crea apilando capas; esto requiere tres decisiones arquitectónicas principales:

-   ¿Cómo representar el texto?
-   ¿Cuántas capas usar en el modelo?
-   ¿Cuántas _unidades ocultas_ usar para cada capa?

En este ejemplo, los datos de entrada consisten en oraciones. Las etiquetas para predecir son 0 o 1.

Una forma de representar el texto es convertir oraciones en vectores de incrustaciones. Use una [[incrustación de texto]] previamente entrenada como la primera capa, que tendrá tres ventajas:

-   No tiene que preocuparse por el preprocesamiento de texto,
-   Benefíciese del aprendizaje por transferencia,
-   la incrustación tiene un tamaño fijo, por lo que es más sencilla de procesar.

Para este ejemplo, usa un **modelo de incrustación de texto previamente entrenado** de [TensorFlow Hub](https://tfhub.dev/) llamado [google/nnlm-en-dim50/2](https://tfhub.dev/google/nnlm-en-dim50/2) .

Hay muchas otras incrustaciones de texto previamente entrenadas de TFHub que se pueden usar en este tutorial:

-   [google/nnlm-en-dim128/2](https://tfhub.dev/google/nnlm-en-dim128/2) : capacitado con la misma arquitectura NNLM con los mismos datos que [google/nnlm-en-dim50/2](https://tfhub.dev/google/nnlm-en-dim50/2) , pero con una dimensión de incrustación más grande. Las incorporaciones dimensionales más grandes pueden mejorar su tarea, pero puede llevar más tiempo entrenar su modelo.
-   [google/nnlm-en-dim128-with-normalization/2](https://tfhub.dev/google/nnlm-en-dim128-with-normalization/2) : lo mismo que [google/nnlm-en-dim128/2](https://tfhub.dev/google/nnlm-en-dim128/2) , pero con normalización de texto adicional, como la eliminación de puntuación. Esto puede ayudar si el texto de su tarea contiene caracteres o signos de puntuación adicionales.
-   [google/universal-sentence-encoder/4](https://tfhub.dev/google/universal-sentence-encoder/4) : un modelo mucho más grande que produce incrustaciones de 512 dimensiones entrenadas con un codificador de red de promedio profundo (DAN).

¡Y muchos más! Encuentre más [modelos de incrustación de texto](https://tfhub.dev/s?module-type=text-embedding) en TFHub.

Primero, creemos una capa de Keras que use un modelo de TensorFlow Hub para incrustar las oraciones y pruébalo en un par de ejemplos de entrada. Tenga en cuenta que, independientemente de la longitud del texto de entrada, la forma de salida de las incorporaciones es: `(num_examples, embedding_dimension)` .
```
embedding = "https://tfhub.dev/google/nnlm-en-dim50/2"

hub_layer = hub.KerasLayer(embedding, input_shape=[],

                           dtype=tf.string, trainable=True)

hub_layer(train_examples_batch[:3])
```
Ahora construyamos el modelo completo:
```
model = tf.keras.Sequential()

model.add(hub_layer)

model.add(tf.keras.layers.Dense(16, activation='relu'))

model.add(tf.keras.layers.Dense(1))

  

model.summary()
```
Las capas se apilan secuencialmente para construir el clasificador:

1.  La primera capa es una capa de TensorFlow Hub. Esta capa utiliza un modelo guardado preentrenado para mapear una oración en su vector de incrustación. El modelo de incrustación de texto preentrenado que está utilizando ( [google/nnlm-en-dim50/2](https://tfhub.dev/google/nnlm-en-dim50/2) ) divide la oración en tokens, incrusta cada token y luego combina la incrustación. Las dimensiones resultantes son: `(num_examples, embedding_dimension)` . Para este modelo NNLM, `embedding_dimension` es 50.
2.  Este vector de salida de longitud fija se canaliza a través de una capa ( `Dense` ) completamente conectada con 16 unidades ocultas.
3.  La última capa está densamente conectada con un solo nodo de salida.

Vamos a compilar el modelo.

# Funcion de perdida y optimizador
Un modelo necesita una función de pérdida y un optimizador para el entrenamiento. Como se trata de un problema de clasificación binaria y el modelo genera logits (una capa de una sola unidad con una activación lineal), utilizará la función de pérdida `binary_crossentropy` .

Esta no es la única opción para una función de pérdida, podría, por ejemplo, elegir `mean_squared_error` . Pero, en general, `binary_crossentropy` es mejor para tratar con probabilidades: mide la "distancia" entre las distribuciones de probabilidad o, en nuestro caso, entre la distribución de la verdad fundamental y las predicciones.

Más adelante, cuando esté explorando problemas de regresión (por ejemplo, para predecir el precio de una casa), verá cómo usar otra función de pérdida llamada error cuadrático medio.

Ahora, configure el modelo para usar un optimizador y una función de pérdida:
```
model.compile(optimizer='adam',

              loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),

              metrics=['accuracy'])
```

# Entrenar el modelo
Entrene el modelo durante 10 épocas en mini lotes de 512 muestras. Son 10 iteraciones sobre todas las muestras en los tensores `x_train` y `y_train` . Mientras entrena, controle la pérdida y la precisión del modelo en las 10 000 muestras del conjunto de validación:
```
history = model.fit(train_data.shuffle(10000).batch(512),

                    epochs=10,

                    validation_data=validation_data.batch(512),

                    verbose=1)
```
# Evaluar el modelo
Y veamos cómo se comporta el modelo. Se devolverán dos valores. Pérdida (un número que representa nuestro error, los valores más bajos son mejores) y precisión.
```
results = model.evaluate(test_data.batch(512), verbose=2)

  

for name, value in zip(model.metrics_names, results):

  print("%s: %.3f" % (name, value))
```
```
49/49 - 3s - loss: 0.3656 - accuracy: 0.8521 - 3s/epoch - 62ms/step loss: 0.366 accuracy: 0.852
```

Este enfoque bastante ingenuo logra una precisión de alrededor del 87%. Con enfoques más avanzados, el modelo debería acercarse al 95%.