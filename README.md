# Deep-Learning-Task1---Image-segmentation
Semantic segmentation project for detecting person, car, dog, and background in images. The repository includes a custom U-Net implementation trained from scratch in PyTorch, along with a benchmark using a pre-trained SegFormer model fine-tuned on the same dataset.

The goal of the project is to compare a self-built segmentation model with a modern pre-trained alternative and analyze performance using metrics such as pixel accuracy, IoU, precision, recall, and F1-score.

The dataset contains three foreground classes, with approximately 256 car images and fewer samples for person and dog, so augmentation was used to balance the training set to 400 samples per class. Each image has a corresponding segmentation mask, and the project uses a train/validation/test split together with an additional external evaluation on 100 unseen Open Images validation images.
