# Classificatie van landgebruik op luchtbeelden met Deep Learning

**Deep Learning · Computer Vision · TensorFlow/Keras · CNN · EfficientNetB0 · PyTorch**

**Talen:** [English](README.md) | [Español](README.es.md) | Nederlands

Vergelijkend deep-learningonderzoek voor de **classificatie van luchtbeelden in 21 categorieën**, uitgevoerd op een door data-augmentatie uitgebreide versie van **UC Merced Land Use**, met **10.500 afbeeldingen**. Het project omvat een volledig verbonden referentiemodel, een zelfontwikkeld convolutioneel neuraal netwerk (CNN), een convolutionele autoencoder, **transfer learning en fine-tuning met EfficientNetB0** en een afzonderlijk experiment in **PyTorch**.

> Deze publieke portfolio-versie is afgeleid van het oorspronkelijke academische notebook. De experimentele code en de geregistreerde resultaten zijn behouden; opdrachtteksten en persoonlijke Google Drive-paden zijn verwijderd. De afbeeldingsdataset en de getrainde modelgewichten worden **niet meegeleverd**. De modellen zijn voor deze publicatie niet opnieuw getraind.

## Belangrijkste resultaten

| Experiment | Gerapporteerd resultaat | Evaluatie |
| --- | ---: | --- |
| Zelfontwikkelde CNN vanaf nul | **86,57%** | Testaccuratesse |
| EfficientNetB0, voorgetrainde backbone bevroren | **97,90%** | Testaccuratesse |
| EfficientNetB0 na fine-tuning | **98,10%** | Testaccuratesse |
| Convolutionele autoencoder | **0,001367** | MSE op testbeelden, genormaliseerd naar [0,1] |
| Eenvoudige CNN in PyTorch | Trainingsverlies 3,0010 → 2,6727 | 5 trainingsepochs; **geen testaccuratesse** |

De hoogste testaccuratesse in het oorspronkelijke notebook werd behaald met **EfficientNetB0 na fine-tuning: 98,10%**. Dit is **0,20 procentpunt** hoger dan de variant met bevroren backbone. Dat verschil vormt op zichzelf geen statistisch onderbouwd bewijs van een algemene verbetering.

![Trainingsaccuratesse EfficientNetB0](assets/efficientnet_accuracy.png)

## Dataset en herkomst

- **Gebruikte dataset:** aangevulde versie van **UC Merced Land Use**, gepubliceerd door `apollo2506` op Kaggle: https://www.kaggle.com/datasets/apollo2506/landuse-scene-classification
- **Oorspronkelijke dataset:** 2.100 afbeeldingen (21 klassen × 100 afbeeldingen); bron: https://vision.ucmerced.edu/datasets/
- **Augmentatie:** 10.500 afbeeldingen (21 klassen × 500 afbeeldingen), verdeeld over 7.350 training, 2.100 validatie en 1.050 test.
- **Invoerformaat:** voor de meeste modellen worden de afbeeldingen naar 224 × 224 pixels geschaald.
- **Omvang van de download:** ongeveer 2 GB, afhankelijk van het archiefformaat.

**Oorspronkelijke wetenschappelijke referentie:** Yang, Y. & Newsam, S. (2010). *Bag-of-Visual-Words and Spatial Extensions for Land-Use Classification*. ACM SIGSPATIAL GIS.

De uitgebreide dataset wordt **niet in GitHub opgeslagen**, wegens de bestandsgrootte en omdat de herdistributievoorwaarden van de Kaggle-versie afzonderlijk gecontroleerd moeten worden. Download de bestanden via de bronwebsite en raadpleeg de actuele gebruiksvoorwaarden.

## Installatie en uitvoering

Python 3, een virtuele omgeving en een GPU worden aanbevolen. In het oorspronkelijke notebook werden **TensorFlow 2.19.0** en **Keras 3.13.2** geregistreerd.

```bash
python -m venv .venv
# Linux / macOS
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab notebooks/landuse_deep_learning.ipynb
```

Pak de dataset buiten Git uit in een map met deze indeling:

```text
data/external/landuse-scene-classification/
  train.csv
  validation.csv
  test.csv
  label_map.json
  images_train_test_val/
    train/<klassenaam>/*.jpg
    validation/<klassenaam>/*.jpg
    test/<klassenaam>/*.jpg
```

De exacte mapnamen kunnen afwijken na het uitpakken. Stel `LANDUSE_DATA_DIR` in op de map die de CSV-bestanden en `images_train_test_val/` bevat als je de gegevens elders bewaart. In Google Colab kun je desgewenst eerst Google Drive koppelen en vervolgens de omgevingsvariabele instellen. De publieke notebook bevat geen persoonlijk opslagpad meer.

Start Jupyter vanuit de hoofdmap van het project. **Let op:** `tf.data.cache()` kan meerdere gigabytes RAM verbruiken bij het bufferen van gedecodeerde afbeeldingen. Schakel de geheugen-cache uit als er te weinig werkgeheugen beschikbaar is.

## Uitgevoerde modellen en methoden

1. Verkennende datasetanalyse en controle van de gebalanceerde train-, validatie- en testverdeling.
2. Volledig verbonden baseline-netwerk met verkleinde afbeeldingen van 32 × 32 pixels.
3. Eigen CNN met convolutielagen, pooling, GlobalAveragePooling en Dropout.
4. Convolutionele autoencoder voor beeldreconstructie, geëvalueerd met MSE.
5. EfficientNetB0, vooraf getraind op ImageNet, met een aangepaste classificatielaag.
6. Fine-tuning van EfficientNetB0 met een lagere learning rate.
7. Een eenvoudige CNN in PyTorch met `Dataset`, `DataLoader` en een zelfgeschreven trainingslus.

![Accuratesse van de CNN](assets/cnn_accuracy.png)

## Aanvullende documentatie

- [Publieke notebook](notebooks/landuse_deep_learning.ipynb)
- [Modelresultaten en beperkingen](docs/MODEL_RESULTS.md)
- [Projectbeschrijvingen voor LinkedIn](docs/PROJECT_DESCRIPTIONS.md)
- [Datasetbron en mappenstructuur](data/README.md)

## Beperkingen

**Mogelijke datalek tussen de subsets:** verschillende geaugmenteerde versies van dezelfde oorspronkelijke opname kunnen in de trainings- en testset terechtgekomen zijn. De koppeling met de oorspronkelijke bronafbeeldingen is niet onafhankelijk gecontroleerd. Een groepsgewijze opsplitsing per originele opname is aan te raden om de generaliseerbaarheid betrouwbaarder te beoordelen.

De resultaten zijn afkomstig uit het oorspronkelijke uitgevoerde notebook, niet uit een nieuwe trainingsronde. Er worden geen modelgewichten of inference-service gepubliceerd. Voor het Dense-baselinemodel en het PyTorch-experiment is geen vergelijkbare definitieve testaccuratesse beschikbaar.

## Academische context

Individueel uitgewerkt voor **M2.875 — Deep Learning (PEC1)**, Master in Data Science aan de **Universitat Oberta de Catalunya (UOC)**, academiejaar 2025–2026. De originele opdrachtteksten worden in de portfolio-versie niet herpubliceerd.
