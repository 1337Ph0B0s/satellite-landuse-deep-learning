# Recorded model results and evaluation notes

All values below were extracted from the executed notebook `M2_875_20252_PEC1_Pazmiño_Naranjo_Pablo.ipynb`. **No independent retraining has been performed for the public portfolio adaptation.**

## Dataset and experimental setup

- 21 land-use scene classes.
- 10,500 images (augmented UC Merced); 7,350 training, 2,100 validation, 1,050 test.
- TensorFlow: **2.19.0**; Keras: **3.13.2** (as printed in the original notebook).
- Main image input shape: **224 × 224 × 3**; the Dense baseline downsamples to 32 × 32.
- Original runtime: GPU reported by TensorFlow; data read from Google Drive in Colab.
- Training pipeline used `tf.data.cache()` and `prefetch(tf.data.AUTOTUNE)` for acceleration.

## Performance comparison

| Model | Evaluation metric | Recorded value | Source cell |
| --- | --- | ---: | --- |
| Dense baseline | Best validation accuracy (approx.) | ~24% | 23–25 (no test accuracy) |
| Custom CNN | Test accuracy | **86.57%** | 30 |
| Custom CNN | Test loss | 0.3969 | 30 |
| Custom CNN | Best validation accuracy | 87.43% | 30 |
| Convolutional autoencoder | Test MSE, inputs [0,1] | **0.001367** | 48 |
| EfficientNetB0 transfer learning (frozen backbone) | Test accuracy | **97.90%** | 56 |
| EfficientNetB0 transfer learning | Test loss | 0.0578 | 56 |
| EfficientNetB0 fine-tuning | Test accuracy | **98.10%** | 62 |
| EfficientNetB0 fine-tuning | Test loss | 0.0797 | 62 |
| PyTorch simple CNN | Training loss first → fifth epoch | 3.0010 → 2.6727 | 69 |

**Comparative improvement:** fine-tuning improved test accuracy from 97.90% to 98.10% (**0.20 percentage points**), based on the recorded runs. This is not a statistical confidence statement.

The custom CNN was trained with three convolutional blocks and a classification head. The autoencoder used an encoder–decoder design with **92,995 trainable parameters**. The original EfficientNetB0 transfer-learning classifier trained **65,121 parameters** with its pretrained backbone frozen. Fine-tuning later enabled training of the backbone.

## Training curves from the original run

![CNN accuracy](../assets/cnn_accuracy.png)
![CNN loss](../assets/cnn_loss.png)
![Autoencoder MSE](../assets/autoencoder_loss.png)
![EfficientNetB0 accuracy](../assets/efficientnet_accuracy.png)
![EfficientNetB0 fine-tuning accuracy](../assets/finetuning_accuracy.png)

## Limitations

1. **Data lineage:** The supplied dataset was augmented. There is no independent verification that transformed versions of each original image remain in the same split. Potential source-image leakage could inflate reported accuracy.
2. **Comparability:** The PyTorch example reports only training loss, not test accuracy. The Dense baseline does not show an equivalent test-set score.
3. **Model outputs:** Training logs and metric graphics are provided; no checkpoints or trained model weights are distributed.
4. **No new experiment:** This repository records historical academic results only and does not claim a new independent replication.
5. **Review original academic notebook carefully:** Some narrative text in the original submission disagrees with adjacent log output (for example, stated training epochs and `EarlyStopping` restoration). The values here follow the saved **numeric execution outputs** where available.
