# 04 — CNN Architectures From Scratch

**VGG-16** and **DenseNet-121** implemented from scratch in PyTorch — every
conv/block/transition layer hand-built, not `torchvision.models` — trained on
a 4-class pet facial-expression classification task.

## Notebooks

| Notebook | Model |
|---|---|
| `vgg16.ipynb` | VGG-16 (with BatchNorm): stacked 3×3 conv blocks + max-pool, 3-layer FC classifier head with dropout |
| `densenet121.ipynb` | DenseNet-121 layout: dense blocks (6/12/24/16 layers, growth rate 32) + 1×1-conv transition layers |
| `densenet121_dropout.ipynb` | Same architecture with dropout in the dense/transition blocks and classifier head |

## Setup

- Custom `Dataset` with on-the-fly augmentation (4 variants per training
  image: original + horizontal/vertical/both flips), `DataLoader` pipeline,
  per-epoch train/val metrics with a final test evaluation, loss & accuracy
  curves.
- Training: Adam (lr=1e-4) + CrossEntropyLoss + `ReduceLROnPlateau`
  (factor 0.1, patience 3), configured for 30 epochs.
- Dataset: [`anshtanwar/pets-facial-expression-dataset`](https://www.kaggle.com/datasets/anshtanwar/pets-facial-expression-dataset)
  (4 classes: happy, sad, angry, other) — fetched automatically via `kagglehub`
  on first run. Needs a Kaggle API token at `~/.kaggle/kaggle.json`.

## Running

```bash
pip install -r ../../requirements.txt
jupyter notebook vgg16.ipynb
```

A GPU is strongly recommended — from-scratch training takes a while on CPU.

