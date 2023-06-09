# TC3002B_IA
Enrique Santos Fraire - A01705746
## Modelo de clasificación de obras de arte
A través de herramientas de inteligencia artificial, se busca la clasificación de imágenes. En este caso, de obras de arte para identificar a que género pertenece.

Se cuenta con 12 géneros disponibles a identificar:
* Anime
* Baroque
* Cubism
* Expressionism
* Impressionism
* Northern Renaissance
* Post-Impressionism
* Primitivism
* Renaissance
* Romanticism
* Surrealism
* Symbolism

### Descripción de los Datasets
El dataset de las obras de arte se extrajo de Kaggle utilizando el dataset [Best Artworks of All Time](https://www.kaggle.com/datasets/ikarus777/best-artworks-of-all-time) por ikarus777.

Adicionalmente, se generó un segundo dataset para el género *Anime*, obtenido de diversas fuentes de terceros.

### Preprocesado de datos
Dado que el primer dataset de artistas se encontraba en una estructura de *Artista/Obras*, fue requerida una reestructuración del dataset. Por lo que, utilizando el archivo de ***[labels.csv](https://github.com/A01705746/TC3002B_IA/blob/main/labels.csv)*** con el que venía el dataset, se filtraron los géneros de los artistas para su correcta asignación en la reestructura, de igual manera se eliminaron algunas imágenes parabalancear los géneros y no haya mucha disparidad.

*Para un mejor apoyo visual de la estrucutra consultar el archivo **[labels_modified.xlsx](https://github.com/A01705746/TC3002B_IA/blob/main/labels_modified.xlsx)**, la documentación del código se encuentra en el archivo **[Data_Genre.ipynb](https://github.com/A01705746/TC3002B_IA/blob/main/Data_Genre.ipynb)***

Posteriormente, una vez que las obras fueron organizadas por géneros, se realizó una división en test y train, así como un preprocesado adicional con técnicas de escalamiento (rescale, rotation_range, width_shift_range, height_shift_range, shear_range, zoom_range, horizontal_flip), pues no afecta el entendimiento de la imagen y le proporciona al modelo más elementos para entrenar.

La división del dataset se realizó de manera aleatoria para un mejor rendimiento al entrenar el modelo.

Para visualizar el dataset original, su reestructura y el split realizado consultar el siguiente **[link](https://drive.google.com/drive/folders/1Y2HcCGRgrWxP-BHMm5VF4IfI5WujnWMO?usp=sharing)**:
* ***original***: Dataset original de artistas
* ***images***: Dataset reestructurado por géneros
* ***model***: Dataset dividido en train y test

**Consideraciones**
* La imágenes del dataset se deben encontrar en una carpeta llamada *original*.
* Se debe crear una carpeta vacía llamada *images*, que es donde se generará la reestructura, y otra carpeta llamada *model*, donde se generará el split de train y test.
* Estas tres carpetas deberán estar al mismo nivel que el archivo .ipynb.

### Primer modelo
Como modelo base se utilizó un modelo Secuencial Categórico, cuenta con una capa Conv2D, una capa Flatten para que las imágenes se procesen en una sola línea, una capa densa con activación *relu* y una capa densa de 12 nodos (el número de géneros a clasificar) de activación *softmax*.

Como métricas se tiene el *Categorical Cross Entropy* para *Loss*, *RMSprop* como *optimizer* y *accuracy* para las *metrics*.

### Segundo modelo
Para el segundo modelo se optó por un modelo preentrenado como lo es *VGG16*, cambiando la capa *Conv2D* por el nuevo modelo, el resto de capas y métricas se mantuvieron iguales.

Sin embargo, se ajustó el hiper parámetro de *learning_rate* de 2e-5 a 1e-5.

### Tercer modelo
Con el fin de explrar más alternativas, se utilizó el mismo modelo anterior pero cambiando el *optimizer* por *Adam* para comparar resultados.

### Estado del arte
**Loss**
* *[Categorical Cross Entropy](https://www.tensorflow.org/api_docs/python/tf/keras/metrics/categorical_crossentropy):*  Se utiliza para medir la discrepancia entre la distribución de probabilidades predicha por un modelo y la distribución de probabilidades real de las etiquetas de los datos. Cuanto menor sea el valor de la entropía cruzada, mejor se ajustará el modelo a los datos de entrenamiento.

**[Optimizers](https://medium.com/analytics-vidhya/a-complete-guide-to-adam-and-rmsprop-optimizer-75f4502d83be)**
* *[RMSprop](https://keras.io/api/optimizers/rmsprop/):* Es una mejora del algoritmo de descenso de gradiente estocástico (SGD) que adapta la tasa de aprendizaje para cada parámetro de forma individual. Utiliza una media móvil ponderada de los cuadrados de los gradientes anteriores para ajustar la tasa de aprendizaje. Esto permite un ajuste más rápido en las direcciones con gradientes grandes y una adaptación más lenta en las direcciones con gradientes pequeños. 
*  *[Adam](https://keras.io/api/optimizers/adam/):* Es una combinación de los métodos de descenso de gradiente estocástico (SGD) con momentum y RMSprop. Adapta la tasa de aprendizaje para cada parámetro individual y también mantiene un promedio móvil de los gradientes anteriores similar a RMSprop.

**Metrics**
* *[Accuracy](https://keras.io/api/metrics/accuracy_metrics/):* Es la proporción de ejemplos clasificados correctamente en relación con el total de ejemplos, verifica la frecuencia en que una predicción coincide con el valor real. Se expresa como un valor entre 0 y 1

**Model**
* *[VGG16](https://keras.io/api/applications/vgg/):* Es un modelo preentrenado que consta de 16 capas de convolución. Se ha utilizado ampliamente como base para la extracción de características en tareas de clasificación de imágenes, cuenta con un rendimiento sólido en varios conjuntos de datos de referencia, como ImageNet.

### Resultados
| Model          | test loss | test accuracy | train loss | train accuracy |
|----------------|-----------|---------------|------------|----------------|
| Base           | 0.9876    | 0.8123        | 2.0467     | 0.3079         |
| VGG with RMS   | 0.8745    | 0.8312        | 1.7327     | 0.4336         |
| VGG with Adam  | 0.7564    | 0.8541        | 1.4415     | 0.5176         |

De los 3 modelos realizados se observa que aquel que tuvo mejor rendimiento fue *VGG16* con el uso del *optimizer RMSprop*.

Si bien *VGG16* con *Adam* tuvo un 51.76% de precisión en el entrenamiento, éste decayó en las pruebas, siendo casi un 4% inferior a *VGG16* con *RMSprop*. Por lo que el más consistente es el **segundo modelo**.

Finalmente, aquí se observa la matriz confusión de las predicciones y etiquetas reales a través de un mapa de calor.

Se puede ver una clara inclinación hacia los géneros de *Anime* y *Privitivism*, mayormete al *Anime*. Esto puede deberse nuevamente al *accuracy* relativamente bajo del modelo y a la complejidad de los datos. Además, tanto en las imágenes de *Anime* como en las de *Privitivism* se aprecia una mayor cantidad de colores vistosos y definidos, por lo que pueden ser fácilmente identificables y relacionables en comparación a los otros géneros con paletas de colores similares que pueden confundirse fácilmente.

Podríamos explorar modelos más complejos o incrementar el set de datos con el fin de hacer a cada clase más fácil de identificar y mejorar el rendimiento de modelo.

<p align="center"><img width=50% src="https://github.com/A01705746/TC3002B_IA/blob/main/Media/matrix.png"></p>
