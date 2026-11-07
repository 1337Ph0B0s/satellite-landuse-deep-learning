# Satellite Land-Use Classification & Image Reconstruction

**Deep Learning · Computer Vision · TensorFlow/Keras · CNN · EfficientNetB0 · PyTorch**

**Languages:** English | [Español](README.es.md) | [Nederlands](README.nl.md)

A comparative deep-learning study of aerial land-use scene classification across **21 classes**, using a **10,500-image augmented UC Merced dataset**. Models include a fully connected baseline, a custom convolutional neural network, a convolutional autoencoder, **EfficientNetB0 transfer learning and fine-tuning**, and a training exercise using **PyTorch**.

> **Portfolio adaptation:** The original experiment has been reorganized for public viewing. Recorded metrics are from the academic notebook; models were **not retrained for publication**. No image dataset or model weights are included.

## Results at a glance

| Experiment | Reported result | Evaluation |
| --- | ---: | --- |
| Custom CNN trained from scratch | **86.57%** | Test accuracy |
| EfficientNetB0, frozen pretrained backbone | **97.90%** | Test accuracy |
| EfficientNetB0, additional fine-tuning | **98.10%** | Test accuracy |
| Convolutional autoencoder | **0.001367** | Test mean squared error on [0,1]-scaled images |
| PyTorch simple CNN | Loss 3.0010 → 2.6727 | Training loss, 5 epochs; **no test accuracy reported** |

Fine-tuned EfficientNetB0 recorded the highest test accuracy in these experiments, improving by 0.20 percentage points over frozen-backbone transfer learning. This is an exploratory academic benchmark, not evidence of deployment-level performance.

## Dataset

- **Name:** UC Merced Land Use, augmented edition ([Kaggle](https://www.kaggle.com/datasets/apollo2506/landuse-scene-classification)).
- **Original dataset:** 21 classes × 100 images = 2,100.
- **Augmented edition:** 21 classes × 500 images = 10,500; images resized to 224 × 224.
- **Split:** 7,350 train / 2,100 validation / 1,050 test (350 / 100 / 50 per class).
- **Archive size:** approximately 2 GB.

Original dataset: https://vision.ucmerced.edu/datasets/

Original reference: Yang, Y. & Newsam, S. (2010), *Bag-of-Visual-Words and Spatial Extensions for Land-Use Classification*, ACM SIGSPATIAL GIS.

Images are not redistributed here. Obtain them from the publisher and check applicable license and terms.

## Installation

Python 3 and a GPU-capable environment recommended. The source execution used TensorFlow 2.19.0 and Keras 3.13.2. Platform/Python/CUDA compatibility varies.

```bash
python -m venv .venv
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab notebooks/landuse_deep_learning.ipynb
```

Place the dataset under `data/external/landuse-scene-classification/` or set `LANDUSE_DATA_DIR` to the extracted directory, containing `train.csv`, `validation.csv`, `test.csv`, `label_map.json` and `images_train_test_val/`. Run the notebook from the repository root. No cloud authorization is included.

`tf.data.cache()` can require several GB of RAM; turn it off or use disk caching on memory-limited machines.

## Experiments

1. Data exploration and balanced split checks.
2. Dense baseline on 32×32 inputs.
3. Custom CNN with three convolutional stages (16/32/64 filters), max pooling and global average pooling.
4. Convolutional autoencoder evaluated using reconstruction MSE.
5. EfficientNetB0 ImageNet transfer learning with frozen backbone.
6. EfficientNetB0 fine-tuning using a lower learning rate.
7. PyTorch custom CNN with `ImageFolder`, `DataLoader` and manual training loop.

See [model results](docs/MODEL_RESULTS.md) and [dataset instructions](data/README.md).

## Limitations and responsible use

- **Potential data leakage:** The augmented dataset may have transformed copies of the same original image across the splits; source-image grouping was not independently verified. This may inflate accuracy.
- Reported metrics come from the previously executed academic notebook; no independent retraining was conducted for publication.
- Dense baseline and PyTorch results lack directly comparable final test accuracies.
- No inference service, validated production model or included weights.
- Credit UC Merced and review the Kaggle augmented edition terms before redistributing images.

## Academic context

Adapted from **M2.875 — Deep Learning (PEC1)**, Universitat Oberta de Catalunya (UOC), MSc in Data Science, academic year 2025–2026. Course exercise prompts are not republished.

## Original execution output

[View the lightweight HTML export of executed notebook output](outputs/original_execution_text.html). This text-focused export excludes embedded images; it complements the editable [source notebook](notebooks/landuse_deep_learning.ipynb).
