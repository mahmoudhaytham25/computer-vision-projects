# 04 — CNN Architectures From Scratch

**VGG-16** and **DenseNet-121** implemented from scratch in PyTorch — every
conv/block/transition layer hand-built, not `torchvision.models` — trained on
a 4-class pet facial-expression classification task.

## Notebooks

| Notebook | Model |
|---|---|
| `vgg16.ipynb` | VGG-16: stacked 3×3 conv blocks + max-pool, FC classifier head |
| `densenet121.ipynb` | DenseNet-121: dense blocks (growth rate 32), bottleneck 1×1 convs, transition layers |
| `densenet121_dropout.ipynb` | Same architecture with dropout in the dense/transition blocks and classifier head |

## Setup

- Custom `Dataset` with on-the-fly augmentation (4 variants per image),
  `DataLoader` pipeline, per-epoch train/val/test metrics, loss & accuracy
  curves.
- Training: Adam + CrossEntropyLoss + StepLR, 30 epochs.
- Dataset: [`anshtanwar/pets-facial-expression-dataset`](https://www.kaggle.com/datasets/anshtanwar/pets-facial-expression-dataset)
  (4 classes: happy, sad, angry, other) — fetched automatically via `kagglehub`
  on first run. Needs a Kaggle API token at `~/.kaggle/kaggle.json`.

## Running

```bash
pip install -r ../../requirements.txt
jupyter notebook vgg16.ipynb
```

A GPU is strongly recommended — from-scratch training takes a while on CPU.

