# Dataset provenance / Procedencia / Herkomst

The **augmented UC Merced Land Use image dataset** used for this experiment is provided by Kaggle user `apollo2506`:

https://www.kaggle.com/datasets/apollo2506/landuse-scene-classification

Original UC Merced dataset: https://vision.ucmerced.edu/datasets/

The augmented edition described in the original notebook contains **10,500 images across 21 classes**, split into **7,350 train / 2,100 validation / 1,050 test**. It was distributed as a roughly 2 GB download at the time of the academic exercise. **These data are not bundled or committed to this repo.**

Place the extracted dataset under `data/external/landuse-scene-classification/`, or set the environment variable `LANDUSE_DATA_DIR` to its location. Its root is expected to contain `train.csv`, `validation.csv`, `test.csv`, `label_map.json` and `images_train_test_val/` with class-named subfolders in `train`, `validation`, and `test`.

The original UC Merced collection is commonly described as public-domain imagery from the USGS National Map; **this does not automatically establish the redistribution license of Kaggle's augmented version**. Consult the dataset page for the applicable terms before sharing images.

### Important evaluation caveat

The exact transformation lineage and parent-original IDs were not verified. If augmentations of a source image appear across train and test splits, the reported test accuracy may be optimistic. Treat the evaluation as applying to the *distributed Kaggle split*, not an independent holdout of geographic locations or source scenes.

**Citation:** Yang, Y. & Newsam, S. (2010). *Bag-of-Visual-Words and Spatial Extensions for Land-Use Classification*. ACM SIGSPATIAL GIS.
