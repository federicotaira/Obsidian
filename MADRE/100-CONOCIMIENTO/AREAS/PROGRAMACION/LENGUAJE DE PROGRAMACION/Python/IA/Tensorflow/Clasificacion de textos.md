# Clasificacion de textos
```
import matplotlib.pyplot as plt

import os

import re

import shutil

import string

import tensorflow as tf

  

from tensorflow.keras import layers

from tensorflow.keras import losses
```

## Análisis de los sentimientos
Este cuaderno entrena un modelo de análisis de opiniones para c
lasificar las reseñas de películas como _positivas_ o _negativas_ , según el texto de la reseña. Este es un ejemplo de [[clasificación binaria]] —o de dos clases—, un tipo de problema de aprendizaje automático importante y ampliamente aplicable.

Utilizará el conjunto de [datos de reseñas de películas grandes](https://ai.stanford.edu/~amaas/data/sentiment/) que contiene el texto de 50 000 reseñas de películas de la base de datos de [películas de Internet](https://www.imdb.com/) . Estos se dividen en 25 000 revisiones para capacitación y 25 000 revisiones para pruebas. Los conjuntos de entrenamiento y prueba están _equilibrados_ , lo que significa que contienen la misma cantidad de reseñas positivas y negativas.
## Descargue y explore el conjunto de datos de IMDB

Descarguemos y extraigamos el conjunto de datos, luego exploremos la estructura del directorio.
```
url = "https://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz"

  

dataset = tf.keras.utils.get_file("aclImdb_v1", url,

                                    untar=True, cache_dir='.',

                                    cache_subdir='')

  

dataset_dir = os.path.join(os.path.dirname(dataset), 'aclImdb')
```
```
Downloading data from https://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz
84131840/84125825 [==============================] - 6s 0us/step
84140032/84125825 [==============================] - 6s 0us/step
```

de posición5
```
os.listdir(dataset_dir)
```
```
['imdb.vocab', 'imdbEr.txt', 'README', 'test', 'train']
```

```
train_dir = os.path.join(dataset_dir, 'train')

os.listdir(train_dir)
```
```
['labeledBow.feat', 
'neg',
'pos',
'unsup',
'unsupBow.feat',
'urls_neg.txt',
'urls_pos.txt', 
'urls_unsup.txt']
```
Los `aclImdb/train/pos` y `aclImdb/train/neg` contienen muchos archivos de texto, cada uno de los cuales es una reseña de una sola película. Echemos un vistazo a uno de ellos.

```
sample_file = os.path.join(train_dir, 'pos/1181_9.txt')

with open(sample_file) as f:

  print(f.read())
```
```
Rachel Griffiths writes and directs this award winning short film. A heartwarming story about coping with grief and cherishing the memory of those we've loved and lost. Although, only 15 minutes long, Griffiths manages to capture so much emotion and truth onto film in the short space of time. Bud Tingwell gives a touching performance as Will, a widower struggling to cope with his wife's death. Will is confronted by the harsh reality of loneliness and helplessness as he proceeds to take care of Ruth's pet cow, Tulip. The film displays the grief and responsibility one feels for those they have loved and lost. Good cinematography, great direction, and superbly acted. It will bring tears to all those who have lost a loved one, and survived.
```

## Cargar conjunto de datos

A continuación, cargará los datos del disco y los preparará en un formato adecuado para el entrenamiento. Para hacerlo, utilizará la útil utilidad [text_dataset_from_directory](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/text_dataset_from_directory?hl=es-419) , que espera una estructura de directorios de la siguiente manera.

```
main_directory/
...classs_a/
.....a_test_1.txt
.....a_test_2.txt
...classs_b/
.....a_test_1.txt
.....a_test_2.txt
```
Para preparar un conjunto de datos para la clasificación binaria, necesitará dos carpetas en el disco, correspondientes a `class_a` y `class_b` . Estas serán las reseñas positivas y negativas de películas, que se pueden encontrar en `aclImdb/train/pos` y `aclImdb/train/neg` . Como el conjunto de datos de IMDB contiene carpetas adicionales, las eliminará antes de usar esta utilidad.
```
remove_dir = os.path.join(train_dir, 'unsup')

shutil.rmtree(remove_dir)
```

A continuación, utilizará la utilidad `text_dataset_from_directory` para crear un [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset?hl=es-419) etiquetado. [tf.data](https://www.tensorflow.org/guide/data?hl=es-419) es una poderosa colección de herramientas para trabajar con datos.

Al ejecutar un experimento de aprendizaje automático, se recomienda dividir el conjunto de datos en tres divisiones: [entrenamiento](https://developers.google.com/machine-learning/glossary?hl=es-419#training_set) , [validación](https://developers.google.com/machine-learning/glossary?hl=es-419#validation_set) y [prueba](https://developers.google.com/machine-learning/glossary?hl=es-419#test-set) .

El conjunto de datos de IMDB ya se ha dividido en entrenamiento y prueba, pero carece de un conjunto de validación. Vamos a crear un conjunto de validación usando una división 80:20 de los datos de entrenamiento usando el argumento `validation_split` a continuación.

```
batch_size = 32

seed = 42

  

raw_train_ds = tf.keras.utils.text_dataset_from_directory(

    'aclImdb/train',

    batch_size=batch_size,

    validation_split=0.2,

    subset='training',

    seed=seed)
```
```
Found 25000 files belonging to 2 classes. 
Using 20000 files for training.
```
Como puede ver arriba, hay 25 000 ejemplos en la carpeta de capacitación, de los cuales usará el 80 % (o 20 000) para la capacitación. Como verá en un momento, puede entrenar un modelo pasando un conjunto de datos directamente a `model.fit` . Si es nuevo en [`tf.data`](https://www.tensorflow.org/api_docs/python/tf/data?hl=es-419) , también puede iterar sobre el conjunto de datos e imprimir algunos ejemplos de la siguiente manera.
```
for text_batch, label_batch in raw_train_ds.take(1):

  for i in range(3):

    print("Review", text_batch.numpy()[i])

    print("Label", label_batch.numpy()[i])
```
Observe que las reseñas contienen texto sin formato (con signos de puntuación y etiquetas HTML ocasionales como `<br/>` ). Mostrará cómo manejarlos en la siguiente sección.

Las etiquetas son 0 o 1. Para ver cuáles corresponden a reseñas de películas positivas y negativas, puede verificar la propiedad `class_names` en el conjunto de datos.
```
print("Label 0 corresponds to", raw_train_ds.class_names[0])

print("Label 1 corresponds to", raw_train_ds.class_names[1])
```
```
Label 0 corresponds to neg
Label 1 corresponds to pos
```
A continuación, creará un conjunto de datos de validación y prueba. Utilizará las 5000 revisiones restantes del conjunto de capacitación para la validación.
```
raw_val_ds = tf.keras.utils.text_dataset_from_directory(

    'aclImdb/train',

    batch_size=batch_size,

    validation_split=0.2,

    subset='validation',

    seed=seed)
```
```
Found 25000 files belonging to 2 classes.
Using 5000 files for validation.
```
```
raw_test_ds = tf.keras.utils.text_dataset_from_directory(

    'aclImdb/test',

    batch_size=batch_size)
```
```
Found 25000 files belonging to 2 classes.
```

### Preparar el conjunto de datos para el entrenamiento
A continuación, estandarizará, tokenizará y vectorizará los datos utilizando la útil capa [`tf.keras.layers.TextVectorization`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/TextVectorization?hl=es-419) .

La estandarización se refiere al preprocesamiento del texto, generalmente para eliminar la puntuación o elementos HTML para simplificar el conjunto de datos. La tokenización se refiere a dividir cadenas en tokens (por ejemplo, dividir una oración en palabras individuales, dividiéndola en espacios en blanco). La vectorización se refiere a convertir tokens en números para que puedan alimentar una red neuronal. Todas estas tareas se pueden lograr con esta capa.

Como vio anteriormente, las reseñas contienen varias etiquetas HTML como `<br />` . Estas etiquetas no serán eliminadas por el estandarizador predeterminado en la capa `TextVectorization` (que convierte el texto a minúsculas y elimina la puntuación de forma predeterminada, pero no elimina HTML). Escribirá una función de estandarización personalizada para eliminar el HTML.

```
def custom_standardization(input_data):

  lowercase = tf.strings.lower(input_data)

  stripped_html = tf.strings.regex_replace(lowercase, '<br />', ' ')

  return tf.strings.regex_replace(stripped_html,

                                  '[%s]' % re.escape(string.punctuation),

                                  '')
```
A continuación, creará una capa de `TextVectorization` . Utilizará esta capa para estandarizar, tokenizar y vectorizar nuestros datos. Establece el modo de `output_mode` en `int` para crear índices enteros únicos para cada token.

Tenga en cuenta que está utilizando la función de división predeterminada y la función de estandarización personalizada que definió anteriormente. También definirá algunas constantes para el modelo, como una `sequence_length` máxima explícita, que hará que la capa rellene o trunque las `sequence_length` a valores exactos de longitud_secuencia.
```
max_features = 10000

sequence_length = 250

  

vectorize_layer = layers.TextVectorization(

    standardize=custom_standardization,

    max_tokens=max_features,

    output_mode='int',

    output_sequence_length=sequence_length)
```
A continuación, llamará a `adapt` para ajustar el estado de la capa de preprocesamiento al conjunto de datos. Esto hará que el modelo genere un índice de cadenas a números enteros.
```
# Make a text-only dataset (without labels), then call adapt

train_text = raw_train_ds.map(lambda x, y: x)

vectorize_layer.adapt(train_text)
```
Vamos a crear una función para ver el resultado de usar esta capa para preprocesar algunos datos.
```
def vectorize_text(text, label):

  text = tf.expand_dims(text, -1)

  return vectorize_layer(text), label
```

```
# retrieve a batch (of 32 reviews and labels) from the dataset

text_batch, label_batch = next(iter(raw_train_ds))

first_review, first_label = text_batch[0], label_batch[0]

print("Review", first_review)

print("Label", raw_train_ds.class_names[first_label])

print("Vectorized review", vectorize_text(first_review, first_label))
```
```
Review tf.Tensor(b'Great movie - especially the music - Etta James - "At Last". This speaks volumes when you have finally found that special someone.', shape=(), dtype=string)
Label neg Vectorized review (<tf.Tensor: shape=(1, 250), dtype=int64, numpy= 
array([[ 86, 17, 260, 2, 222, 1, 571, 31, 229, 11, 2418, 1, 51, 22, 25, 404, 251, 12, 306, 282, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], dtype=int64)>, <tf.Tensor: shape=(), dtype=int32, numpy=0>)
```
Como puede ver arriba, cada token ha sido reemplazado por un número entero. Puede buscar el token (cadena) al que corresponde cada entero llamando a `.get_vocabulary()` en la capa.
```
print("1287 ---> ",vectorize_layer.get_vocabulary()[1287])

print(" 313 ---> ",vectorize_layer.get_vocabulary()[313])

print('Vocabulary size: {}'.format(len(vectorize_layer.get_vocabulary())))
```
```
1287 --->  silent
 313 --->  night
Vocabulary size: 10000
```
Está casi listo para entrenar a su modelo. Como paso final de preprocesamiento, aplicará la capa TextVectorization que creó anteriormente al conjunto de datos de entrenamiento, validación y prueba.
```
train_ds = raw_train_ds.map(vectorize_text)

val_ds = raw_val_ds.map(vectorize_text)

test_ds = raw_test_ds.map(vectorize_text)
```
### Configurar el conjunto de datos para el rendimiento

Estos son dos métodos importantes que debe usar al cargar datos para asegurarse de que la E/S no se bloquee.

`.cache()` mantiene los datos en la memoria después de que se cargan fuera del disco. Esto asegurará que el conjunto de datos no se convierta en un cuello de botella mientras entrena su modelo. Si su conjunto de datos es demasiado grande para caber en la memoria, también puede usar este método para crear un caché en disco de alto rendimiento, que es más eficiente para leer que muchos archivos pequeños.

`.prefetch()` superpone el preprocesamiento de datos y la ejecución del modelo durante el entrenamiento.

Puede obtener más información sobre ambos métodos, así como sobre cómo almacenar datos en caché en el disco en la [guía de rendimiento de datos](https://www.tensorflow.org/guide/data_performance?hl=es-419) .

```
AUTOTUNE = tf.data.AUTOTUNE

  

train_ds = train_ds.cache().prefetch(buffer_size=AUTOTUNE)

val_ds = val_ds.cache().prefetch(buffer_size=AUTOTUNE)

test_ds = test_ds.cache().prefetch(buffer_size=AUTOTUNE)
```
## Crear modelo
Es hora de crear tu red neuronal:
```
embedding_dim = 16
```
```
model = tf.keras.Sequential([

  layers.Embedding(max_features + 1, embedding_dim),

  layers.Dropout(0.2),

  layers.GlobalAveragePooling1D(),

  layers.Dropout(0.2),

  layers.Dense(1)])

  

model.summary()
```
| layer(type)                                       | Output           | Param# |
| ------------------------------------------------- | ---------------- | ------ |
| embedding (Embedding)                             | (None, None, 16) | 160016 |
| dropout (Dropout)                                 | (None, None, 16) | 0      |
| global_average_pooling1d (GlobalAveragePooling1D) | (None, 16)       | 0      |
| dropout_1 (Dropout)                               | (None, 16)       | 0      |
| dense (Dense)                                     | (None, 1)        | 17     |
|                 |        |

---
Total params: 160,033 
Trainable params: 160,033 
Non-trainable params: 0

---

Las capas se apilan secuencialmente para construir el clasificador:

1.  La primera capa es una capa de `Embedding` . Esta capa toma las revisiones codificadas con enteros y busca un vector de incrustación para cada índice de palabras. Estos vectores se aprenden a medida que el modelo se entrena. Los vectores agregan una dimensión a la matriz de salida. Las dimensiones resultantes son: `(batch, sequence, embedding)` . Para obtener más información sobre incrustaciones, consulte el [tutorial de incrustaciones](https://www.tensorflow.org/tutorials/text/word_embeddings?hl=es-419) de Word.
2.  A continuación, una capa `GlobalAveragePooling1D` devuelve un vector de salida de longitud fija para cada ejemplo promediando la dimensión de la secuencia. Esto permite que el modelo maneje entradas de longitud variable, de la manera más simple posible.
3.  Este vector de salida de longitud fija se canaliza a través de una capa ( `Dense` ) completamente conectada con 16 unidades ocultas.
4.  La última capa está densamente conectada con un solo nodo de salida.

## Función de pérdida y optimizador
Un modelo necesita una función de pérdida y un optimizador para el entrenamiento. Dado que se trata de un problema de clasificación binaria y el modelo genera una probabilidad (una capa de una sola unidad con una activación sigmoidea), utilizará la función de pérdida [`losses.BinaryCrossentropy`](https://www.tensorflow.org/api_docs/python/tf/keras/losses/BinaryCrossentropy?hl=es-419) .

Ahora, configure el modelo para usar un optimizador y una función de pérdida:
```
model.compile(loss=losses.BinaryCrossentropy(from_logits=True),

              optimizer='adam',

              metrics=tf.metrics.BinaryAccuracy(threshold=0.0))
```
## Entrenar al modelo

Entrenará el modelo pasando el objeto del `dataset` de datos al método de ajuste.
```
epochs = 10history = model.fit(    train_ds,    validation_data=val_ds,    epochs=epochs)
```
```
Epoch 1/10
625/625 [==============================] - 4s 4ms/step - loss: 0.6644 - binary_accuracy: 0.6894 - val_loss: 0.6159 - val_binary_accuracy: 0.7696
Epoch 2/10
625/625 [==============================] - 2s 4ms/step - loss: 0.5494 - binary_accuracy: 0.8020 - val_loss: 0.4993 - val_binary_accuracy: 0.8226
Epoch 3/10
625/625 [==============================] - 2s 3ms/step - loss: 0.4450 - binary_accuracy: 0.8447 - val_loss: 0.4205 - val_binary_accuracy: 0.8466
Epoch 4/10
625/625 [==============================] - 2s 3ms/step - loss: 0.3778 - binary_accuracy: 0.8659 - val_loss: 0.3740 - val_binary_accuracy: 0.8618
Epoch 5/10
625/625 [==============================] - 2s 3ms/step - loss: 0.3357 - binary_accuracy: 0.8785 - val_loss: 0.3451 - val_binary_accuracy: 0.8678
Epoch 6/10
625/625 [==============================] - 2s 3ms/step - loss: 0.3055 - binary_accuracy: 0.8885 - val_loss: 0.3260 - val_binary_accuracy: 0.8700
Epoch 7/10
625/625 [==============================] - 2s 3ms/step - loss: 0.2817 - binary_accuracy: 0.8971 - val_loss: 0.3126 - val_binary_accuracy: 0.8730
Epoch 8/10
625/625 [==============================] - 2s 4ms/step - loss: 0.2616 - binary_accuracy: 0.9034 - val_loss: 0.3037 - val_binary_accuracy: 0.8754
Epoch 9/10
625/625 [==============================] - 2s 4ms/step - loss: 0.2458 - binary_accuracy: 0.9110 - val_loss: 0.2965 - val_binary_accuracy: 0.8788
Epoch 10/10
625/625 [==============================] - 2s 4ms/step - loss: 0.2319 - binary_accuracy: 0.9158 - val_loss: 0.2920 - val_binary_accuracy: 0.8792
```
## Evaluar el modelo
Veamos cómo se comporta el modelo. Se devolverán dos valores. Pérdida (un número que representa nuestro error, los valores más bajos son mejores) y precisión.
```
loss, accuracy = model.evaluate(test_ds)

print("Loss: ", loss)

print("Accuracy: ", accuracy)
```
```
782/782 [==============================] - 15s 19ms/step - loss: 0.3101 - binary_accuracy: 0.8736 
Loss: 0.31007683277130127
Accuracy: 0.8736000061035156
```
Este enfoque bastante ingenuo logra una precisión de alrededor del 86%.
### Crear un grafico de precision y predida a lo largo del tiempo
`model.fit()` devuelve un objeto de `History` que contiene un diccionario con todo lo que sucedió durante el entrenamiento:
```
history_dict = history.history

history_dict.keys()
```
```
dict_keys(['loss', 'binary_accuracy', 'val_loss', 'val_binary_accuracy'])
```
Hay cuatro entradas: una para cada métrica monitoreada durante el entrenamiento y la validación. Puede usarlos para trazar la pérdida de entrenamiento y validación para comparar, así como la precisión del entrenamiento y la validación:
```
acc = history_dict['binary_accuracy']

val_acc = history_dict['val_binary_accuracy']

loss = history_dict['loss']

val_loss = history_dict['val_loss']

  

epochs = range(1, len(acc) + 1)

  

# "bo" is for "blue dot"

plt.plot(epochs, loss, 'bo', label='Training loss')

# b is for "solid blue line"

plt.plot(epochs, val_loss, 'b', label='Validation loss')

plt.title('Training and validation loss')

plt.xlabel('Epochs')

plt.ylabel('Loss')

plt.legend()

  

plt.show()
```
![[Pasted image 20230208152203.png]]
```
plt.plot(epochs, acc, 'bo', label='Training acc')

plt.plot(epochs, val_acc, 'b', label='Validation acc')

plt.title('Training and validation accuracy')

plt.xlabel('Epochs')

plt.ylabel('Accuracy')

plt.legend(loc='lower right')

  

plt.show()
```
![[Pasted image 20230208152242.png]]
En este gráfico, los puntos representan la pérdida y la precisión del entrenamiento, y las líneas sólidas son la pérdida y la precisión de la validación.

Observe que la pérdida de entrenamiento _disminuye_ con cada época y la precisión del entrenamiento _aumenta_ con cada época. Esto se espera cuando se usa una optimización de descenso de gradiente: debería minimizar la cantidad deseada en cada iteración.

Este no es el caso de la pérdida de validación y la precisión: parecen alcanzar su punto máximo antes que la precisión del entrenamiento. Este es un ejemplo de sobreajuste: el modelo funciona mejor con los datos de entrenamiento que con los datos que nunca antes había visto. Después de este punto, el modelo sobreoptimiza y aprende representaciones _específicas_ de los datos de entrenamiento que no se _generalizan_ a los datos de prueba.

Para este caso particular, podría evitar el sobreajuste simplemente deteniendo el entrenamiento cuando la precisión de la validación ya no aumente. Una forma de hacerlo es utilizar la devolución de llamada [`tf.keras.callbacks.EarlyStopping`](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping?hl=es-419) .
## Exportar el modelo
En el código anterior, aplicó la capa `TextVectorization` al conjunto de datos antes de enviar texto al modelo. Si desea que su modelo sea capaz de procesar cadenas sin formato (por ejemplo, para simplificar su implementación), puede incluir la capa `TextVectorization` dentro de su modelo. Para hacerlo, puedes crear un nuevo modelo usando los pesos que acabas de entrenar.
```
export_model = tf.keras.Sequential([

  vectorize_layer,

  model,

  layers.Activation('sigmoid')

])

  

export_model.compile(

    loss=losses.BinaryCrossentropy(from_logits=False), optimizer="adam", metrics=['accuracy']

)

  

# Test it with `raw_test_ds`, which yields raw strings

loss, accuracy = export_model.evaluate(raw_test_ds)

print(accuracy)
```
```782/782 [==============================] 
- 5s 6ms/step
- loss: 0.3101
- accuracy: 0.8736 0.8736000061035156
```
### Inferencia sobre los nuevos datos
Para obtener predicciones para nuevos ejemplos, simplemente puede llamar a `model.predict()` .
```
examples = [

  "The movie was great!",

  "The movie was okay.",

  "The movie was terrible..."

]

  

export_model.predict(examples)
```
```
1/1 [==============================] - 0s 101ms/step

array([[0.61112636], [0.43377408], [0.35037863]], dtype=float32)
```
Incluir la lógica de preprocesamiento de texto dentro de su modelo le permite exportar un modelo para producción que simplifica la implementación y reduce el potencial de [sesgo de entrenamiento/prueba](https://developers.google.com/machine-learning/guides/rules-of-ml?hl=es-419#training-serving_skew) .

Hay una diferencia de rendimiento a tener en cuenta al elegir dónde aplicar la capa TextVectorization. Usarlo fuera de su modelo le permite realizar procesamiento de CPU asíncrono y almacenamiento en búfer de sus datos cuando entrena en GPU. Por lo tanto, si está entrenando su modelo en la GPU, probablemente desee optar por esta opción para obtener el mejor rendimiento mientras desarrolla su modelo, luego cambie para incluir la capa TextVectorization dentro de su modelo cuando esté listo para prepararse para la implementación. .

Visite este [tutorial](https://www.tensorflow.org/tutorials/keras/save_and_load?hl=es-419) para obtener más información sobre cómo guardar modelos.