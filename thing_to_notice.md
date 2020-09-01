----
----

# Things to notice

These are things that we would like candidates to notice:

- Data:
  - No train / validation / test split.
  - Data is not normalized.
  - Data is highly imbalanced (``y.mean()=0.1``).

- Model:
  - Feature extractor:
    - VGG16 is probably not optimal model choice for feature extractor.
    - Model was pretrained on ImageNet, may not be ideal for pathology data (due to domain shift).
    - Max pooling is uncommon for final layer in feature extractor (average pooling is more common).
  - Classifier:
    - New classifier is probably too simple, single layer, no regularization such as Dropout etc.
    - Output layer of new classifier could be reduced to single sigmoid output (for this binary classifier).

- Loss:
  - Crossentropy loss is very not robust against data imbalance, e.g. focal loss would be better.
  - No weighting is applied to counter data imbalance.
  - Categorical crossentropy could be reduced to binary crossentropy (for single sigmoid output).

- Training:
  - Data augmentation: 
    - No data augmentation is applied.
  - Transfer learning: 
    - Classifier is not pre-trained with frozen feature extractor.
    - 
  - Optimizer: 
    - SGD with lr=1e-5 is probably not an ideal optimizer. Optimizer with adaptive learning rates (e.g. Adam) may be better.
  - Scheduling:
    - No learning rate schedule (e.g. reduce on plateau).
    - No checkpointing (save best model).
    - No early stopping (to prevent)
  - Data loading:
    - Batch size (32) can be increased to fill the GPU memory (e.g. 512),  increase training performance and make convergence faster (fewer backprop steps, larger steps).
  - Metrics:
    - Accuracy is not appropriate for imbalanced data (random expectation = 0.9).
    - No validation metrics are computed during training. No way to check overfitting.

- Analysis:
  - History:
    - The model probably overfits, but we can't know due to lack of train / validation / test split.
    - Accuracy is not appropriate for imbalanced data (random expectation = 0.9).
  - Evaluation:
    - Based on training set instead of an independent validation or test set.
    - AUC should be assessed on validation set.
  - Prediction / inference:
    - Inference is done on training sample instead of sample from validation or test set. 
    - Test time augmentation could be applied, e.g. under different rotations and brightnesses.
    - Model outputs should not be interpreted as actually class probabilities or (un)certainties due to poor model calibration / overconfidence.
