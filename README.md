# Deep-Learning-Task1---Image-segmentation

Semantic segmentation project for detecting person, car, dog, and background in images. The repository includes a custom U-Net implementation (Image segmentation.optimized.final.ipynb FILE) trained from scratch in PyTorch, along with a benchmark using a pre-trained SegFormer model (Image segmentation.segformer.ipynb FILE) fine-tuned on the same dataset.

The goal of the project is to compare a self-built segmentation model with a modern pre-trained alternative and analyze performance using metrics such as pixel accuracy, IoU, precision, recall, and F1-score.

The dataset contains three foreground classes, with approximately 256 car images and fewer samples for person and dog, so augmentation was used to balance the training set to 400 samples per class. Each image has a corresponding segmentation mask, and the project uses a train/validation/test split together with an additional external evaluation on 100 unseen Open Images validation images.

---- RESULTS ----

## On the external OpenImages-100 benchmark, the custom U-Net achieved 55.2% pixel accuracy with a foreground mean IoU of 0.114, showing that it struggled to generalize well to unseen images. Its strongest foreground class was dog with IoU=0.260 and F1=0.413, while person and car performed poorly, indicating weak transfer outside the local dataset.

## The fine-tuned pre-trained SegFormer performed substantially better on the same benchmark, reaching 74.2% pixel accuracy and mIoU_fg=0.325. It gave much stronger results for car (IoU=0.354, F1=0.523) and especially dog (IoU=0.537, F1=0.699), while person remained the most difficult class for both models. Overall, the benchmark shows that the pre-trained SegFormer generalizes much better than the U-Net trained from scratch.
