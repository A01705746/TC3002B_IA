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

Posteriormente, una vez que las obras fueron organizadas por géneros, se realizó una división en test y train, así como un preprocesado con técnicas de escalamiento.

La división del dataset se realizó de manera aleatoria para un mejor rendimiento al entrenar el modelo.

Para visualizar el dataset original, su reestructura y el split realizado consultar el siguiente [link](https://drive.google.com/drive/folders/1Y2HcCGRgrWxP-BHMm5VF4IfI5WujnWMO?usp=sharing):
* ***original***: Dataset original de artistas
* ***images***: Dataset reestructurado por géneros
* ***model***: Dataset dividido en train y test

**Consideraciones**
* La imágenes del dataset se deben encontrar en una carpeta llamada *original*.
* Se debe crear una carpeta vacía llamada *images*, que es donde se generará la reestructura, y otra carpeta llamada *model*, donde se generará el split de train y test.
* Estas tres carpetas deberán estar al mismo nivel que el archivo .ipynb.
