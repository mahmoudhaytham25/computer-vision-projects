# 05 — Steel Surface Defect Segmentation

Semantic segmentation of manufacturing defects on steel surfaces — from the
[Severstal Steel Defect Detection](https://www.kaggle.com/c/severstal-steel-defect-detection)
Kaggle competition (4 defect classes, run-length-encoded masks).

Same validation image — annotated defect mask vs. model prediction:

![gt vs pred](outputs/pred_vs_gt_1.png)

![gt vs pred](outputs/pred_vs_gt_2.png)

A typical failure mode — the model localizes the defect but under-segments it:

![gt vs pred](outputs/pred_vs_gt_3.png)

Another failure mode — false-positive defects on a clean surface:

![gt vs pred](outputs/pred_vs_gt_4.png)

## What's inside

- **RLE → mask pipeline** — decodes the competition's run-length encodings,
  reshapes the per-class mask columns, and filters mostly-empty samples.
- **Custom `UNetResNet`** — U-Net decoder with skip connections on top of a
  ResNet-50 encoder, hand-built in PyTorch.
- **Architecture comparison** — `UNetPlusPlusResNet`, `PSPNetResNet`, and
  `FPNResNet` variants defined alongside (also via
  `segmentation_models_pytorch`).
- **Metrics** — Dice coefficient and mIoU tracked per epoch;
  `training_metrics_UnetResNet50.pt` checkpoints let runs resume.

## Results (15 epochs)

Final epoch, with validation peak in parentheses:

| Metric | Train | Validation |
|---|---|---|
| mIoU | ~0.38 | ~0.34 (peak ~0.39) |
| Dice | ~0.55 | ~0.48 (peak ~0.55) |

![loss & dice curves](outputs/loss_dice_curves.png)

## Running

Designed for Kaggle notebooks (GPU quota + the competition dataset mounted at
`/kaggle/input/severstal-steel-defect-detection`). Locally:

```bash
pip install -r ../../requirements.txt
jupyter notebook steel_defect_segmentation.ipynb
```

Download the dataset via the competition page or
`kaggle competitions download -c severstal-steel-defect-detection`, then point
the paths in the first cells at your copy.

## Credits

Built with Mahmoud Al-Zamly.
