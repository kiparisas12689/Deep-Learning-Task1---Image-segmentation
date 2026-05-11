## Author Information

- Name and surname: `Kipras Zenkevičius`
- LSP number: `2516007`

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

## Results

### External benchmark: OpenImages-100

The final external benchmark was performed on `100` unseen Open Images validation images.

#### U-Net

- loss: `2.2296`
- pixel accuracy: `0.552`
- foreground mIoU: `0.114`

| Class | IoU | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| background | 0.570 | 0.891 | 0.612 | 0.726 |
| person | 0.043 | 0.044 | 0.618 | 0.082 |
| car | 0.039 | 0.432 | 0.041 | 0.076 |
| dog | 0.260 | 0.368 | 0.469 | 0.413 |

The custom U-Net struggled to generalize to the external Open Images benchmark. Its strongest foreground class was `dog`, while `person` and `car` remained difficult.

#### SegFormer

- loss: `1.4462`
- pixel accuracy: `0.742`
- foreground mIoU: `0.325`

| Class | IoU | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| background | 0.706 | 0.972 | 0.721 | 0.828 |
| person | 0.083 | 0.097 | 0.353 | 0.153 |
| car | 0.354 | 0.419 | 0.694 | 0.523 |
| dog | 0.537 | 0.545 | 0.973 | 0.699 |

The fine-tuned pre-trained SegFormer generalized substantially better than the U-Net, especially for `car` and `dog`. `Person` remained the most challenging class for both models.

#### Summary

Overall, the benchmark shows that the pre-trained SegFormer is a much stronger external baseline than the custom U-Net trained from scratch. It achieved higher pixel accuracy, higher foreground mIoU, and better per-class F1 scores on most classes.

## Data And Weights

Large datasets and trained checkpoints are not stored in this GitHub repository.

## Main Notebooks

- Image segmentation.optimized.final.ipynb: U-Net training, checkpoint saving, and presentation inference
- Image segmentation.segformer.ipynb: standalone SegFormer benchmark notebook

