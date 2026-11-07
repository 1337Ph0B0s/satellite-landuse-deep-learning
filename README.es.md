# Clasificación de usos del suelo e imágenes aéreas mediante Deep Learning

**Deep Learning · Visión por Computador · TensorFlow/Keras · CNN · EfficientNetB0 · PyTorch**

**Idiomas:** [English](README.md) | Español | [Nederlands](README.nl.md)

Estudio comparativo de redes neuronales aplicado a la **clasificación de escenas aéreas en 21 categorías**, utilizando una versión ampliada mediante aumentación de datos del conjunto **UC Merced Land Use**, con **10.500 imágenes**. Se implementaron una red densa de referencia, una CNN propia, un autoencoder convolucional, **transfer learning y fine-tuning con EfficientNetB0**, y un experimento de entrenamiento manual en **PyTorch**.

> Esta es una adaptación para portafolio del notebook académico. Conserva el código experimental y las métricas registradas, elimina consignas de la actividad y rutas personales y **no incluye el dataset de imágenes ni los pesos entrenados**. Los modelos no han vuelto a entrenarse para esta publicación.

## Resultados principales

| Experimento | Resultado documentado | Métrica |
| --- | ---: | --- |
| CNN propia desde cero | **86,57 %** | Accuracy en test |
| EfficientNetB0 con backbone preentrenado congelado | **97,90 %** | Accuracy en test |
| EfficientNetB0 tras fine-tuning | **98,10 %** | Accuracy en test |
| Autoencoder convolucional | **0,001367** | MSE en test, imágenes normalizadas [0,1] |
| CNN en PyTorch | Pérdida 3,0010 → 2,6727 | Entrenamiento de 5 épocas; **sin accuracy de test** |

El mayor resultado entre los clasificadores evaluados fue **98,10 %** con EfficientNetB0 y fine-tuning. La diferencia respecto al modelo con backbone congelado fue de **0,20 puntos porcentuales**; no representa por sí sola una mejora estadísticamente validada.

![Precisión de EfficientNetB0 durante el entrenamiento](assets/efficientnet_accuracy.png)

## Dataset y procedencia

- **Dataset utilizado:** UC Merced Land Use, **versión aumentada** publicada por `apollo2506` en Kaggle: https://www.kaggle.com/datasets/apollo2506/landuse-scene-classification
- **Dataset original:** 2.100 imágenes (21 clases × 100 imágenes), fuente: https://vision.ucmerced.edu/datasets/
- **Versión aumentada:** 10.500 imágenes (21 clases × 500 imágenes), distribuidas en 7.350 entrenamiento, 2.100 validación y 1.050 prueba.
- **Dimensiones de entrada:** las imágenes se redimensionan a 224 × 224 píxeles para la mayor parte de los modelos.
- **Descarga:** aproximadamente 2 GB, según el formato del archivo.

**Cita original:** Yang, Y. y Newsam, S. (2010). *Bag-of-Visual-Words and Spatial Extensions for Land-Use Classification*. ACM SIGSPATIAL GIS.

El conjunto aumentado **no está incluido en GitHub** por su tamaño y porque debe verificarse su licencia de distribución. Descárgalo desde Kaggle y revisa sus condiciones de uso.

## Instalación y ejecución

Se recomienda Python 3, un entorno virtual y una GPU. El notebook original registró **TensorFlow 2.19.0** y **Keras 3.13.2**.

```bash
python -m venv .venv
# Linux / macOS
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab notebooks/landuse_deep_learning.ipynb
```

Descomprime el dataset, fuera de Git, en una carpeta con la siguiente estructura:

```text
data/external/landuse-scene-classification/
  train.csv
  validation.csv
  test.csv
  label_map.json
  images_train_test_val/
    train/<nombre_clase>/*.jpg
    validation/<nombre_clase>/*.jpg
    test/<nombre_clase>/*.jpg
```

La ruta puede variar según el archivo descargado. Si lo ubicas en otro directorio, establece la variable de entorno `LANDUSE_DATA_DIR` apuntando a la carpeta que contiene los archivos CSV y `images_train_test_val/`. En Google Colab puedes montar Drive previamente y definir dicha ruta; el notebook público ya no contiene rutas personales.

Ejecuta Jupyter desde la raíz del proyecto. **Advertencia:** `tf.data.cache()` puede consumir varios gigabytes de RAM, debido al tamaño de las imágenes decodificadas. Puedes desactivar la caché en memoria si tu entorno dispone de pocos recursos.

## Modelos y técnicas

1. Exploración del dataset y comprobación de particiones balanceadas.
2. Red densa de referencia con reducción de imágenes a 32 × 32.
3. CNN propia con convoluciones, pooling, GlobalAveragePooling y Dropout.
4. Autoencoder convolucional para reconstrucción de imágenes y evaluación MSE.
5. EfficientNetB0 preentrenado en ImageNet con clasificación personalizada.
6. Fine-tuning del modelo EfficientNetB0 utilizando una tasa de aprendizaje menor.
7. Red sencilla en PyTorch con `Dataset`, `DataLoader` y bucle de entrenamiento explícito.

![Curva de precisión CNN](assets/cnn_accuracy.png)

## Documentación adicional

- [Notebook público](notebooks/landuse_deep_learning.ipynb)
- [Resultados y limitaciones](docs/MODEL_RESULTS.md)
- [Descripciones del proyecto para LinkedIn](docs/PROJECT_DESCRIPTIONS.md)
- [Estructura y fuente del dataset](data/README.md)

## Limitaciones

**Posible fuga entre particiones:** si las imágenes aumentadas derivadas de una misma imagen original quedaron repartidas entre entrenamiento y prueba, las cifras de accuracy pueden ser optimistas. No se verificó la agrupación de imágenes por escena original. Para evaluar la generalización con mayor rigor debe aplicarse una partición basada en el identificador de la imagen original.

Los resultados corresponden al notebook entregado, no a una nueva ejecución. No se proporcionan pesos, servicio de inferencia ni medidas de precisión independientes para la red densa y el experimento PyTorch.

## Contexto académico

Proyecto desarrollado individualmente para la asignatura **M2.875 — Deep Learning (PEC1)** del Máster en Ciencia de Datos de la **Universitat Oberta de Catalunya**, curso 2025–2026. Esta versión no reproduce las consignas originales de la asignatura.
