# Image Segmentation Project

Semantic segmentation project for detecting `background`, `person`, `car`, and `dog` in images.

The repository includes:
- a custom U-Net model trained from scratch in PyTorch
- a benchmark based on a pre-trained SegFormer model fine-tuned on the same classes
- evaluation on local test data and an external `OpenImages-100` benchmark
- checkpoint saving and presentation-time inference on unseen images

## Dataset

The local dataset contains three foreground classes:
- `car`
- `person`
- `dog`

The class distribution was not fully balanced, so augmentation was used to increase the training set to `400` samples per class.

Expected local folder structure:

```text
data/
  car/
    images/
    masks/
  person/
    images/
    masks/
  dog/
    images/
    masks/
```

Dog masks are expected to use the `annotated_` filename prefix in the notebook pipeline.

### Data format

Each class must be stored in its own folder, and every image must have a matching segmentation mask.

- `images/` contains the original RGB images
- `masks/` contains the corresponding segmentation masks
- supported image extensions include `.jpg`, `.jpeg`, `.png`, and `.webp`
- supported mask extensions include `.png`, `.jpg`, `.jpeg`, and `.gif`

For `car` and `person`, the mask should normally have the same filename stem as the image. Example:

```text
images/car_001.jpg
masks/car_001.png
```

For `dog`, the notebook expects the mask filename to use the `annotated_` prefix. Example:

```text
images/dog_001.jpg
masks/annotated_dog_001.png
```

The masks should be binary:
- background pixels should be `0`
- object pixels should be non-zero

During preprocessing, the binary masks are converted into the semantic class IDs used by the model:
- `0 = background`
- `1 = person`
- `2 = car`
- `3 = dog`

## Models

### U-Net

A custom U-Net was implemented and trained from scratch for 4-class semantic segmentation.

### SegFormer

A pre-trained SegFormer model was fine-tuned as a benchmark to compare against the custom U-Net.

## Evaluation

The project reports:
- pixel accuracy
- IoU
- precision
- recall
- F1-score

Evaluation is performed on:
- a local validation/test split
- `100` unseen Open Images validation images containing at least one of `Person`, `Car`, or `Dog`

## Data And Weights

Large datasets and trained checkpoints are not stored in this GitHub repository.

## Main Notebooks

- [Image segmentation.optimized.v2.ipynb](./Image%20segmentation.optimized.v2.ipynb): U-Net training, checkpoint saving, and presentation inference
- [Image segmentation.optimized.v4.segformer-standalone.ipynb](./Image%20segmentation.optimized.v4.segformer-standalone.ipynb): standalone SegFormer benchmark notebook

