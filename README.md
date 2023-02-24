# Eva8_s8_HRF

### This repo is about Higher Receptive Fields in CNNs.

### Assignment

1. Write a custom ResNet architecture for CIFAR10 that has the following architecture:
    1. PrepLayer - Conv 3x3 s1, p1) >> BN >> RELU [64k]
    2. Layer1 -
        1. X = Conv 3x3 (s1, p1) >> MaxPool2D >> BN >> RELU [128k]
        2. R1 = ResBlock( (Conv-BN-ReLU-Conv-BN-ReLU))(X) [128k] 
        3. Add(X, R1)
    3. Layer 2 -
        1. Conv 3x3 [256k]
        2. MaxPooling2D
        3. BN
        4. ReLU
    4. Layer 3 -
        1. X = Conv 3x3 (s1, p1) >> MaxPool2D >> BN >> RELU [512k]
        2. R2 = ResBlock( (Conv-BN-ReLU-Conv-BN-ReLU))(X) [512k]
        3. Add(X, R2)
    5. MaxPooling with Kernel Size 4
    6. FC Layer 
    7. SoftMax
2. Uses One Cycle Policy such that:
    1. Total Epochs = 24
    2. Max at Epoch = 5
    3. LRMIN = FIND
    4. LRMAX = FIND
    5. NO Annihilation
3. Uses this transform -RandomCrop 32, 32 (after padding of 4) >> FlipLR >> Followed by CutOut(8, 8)
4. Batch size = 512
5. Target Accuracy: 90% (93.8% quadruple scores). 

### Solution
1. Please refer the the [codebase](https://github.com/Rohithmarktricks/eva8_codebase), this repo contains the source code for most of the modules and utils that were used in the assignment solution.
2. [custom_resnet.py](https://github.com/Rohithmarktricks/eva8_codebase/blob/main/models/custom_resnet.py) contains the source code of the ResNet model that has been used in the assignment.

The model has the following structure:
```
----------------------------------------------------------------
        Layer (type)               Output Shape         Param #
================================================================
            Conv2d-1           [-1, 64, 32, 32]           1,728
       BatchNorm2d-2           [-1, 64, 32, 32]             128
              ReLU-3           [-1, 64, 32, 32]               0
            Conv2d-4          [-1, 128, 32, 32]          73,728
         MaxPool2d-5          [-1, 128, 16, 16]               0
       BatchNorm2d-6          [-1, 128, 16, 16]             256
              ReLU-7          [-1, 128, 16, 16]               0
            Conv2d-8          [-1, 128, 16, 16]         147,456
       BatchNorm2d-9          [-1, 128, 16, 16]             256
             ReLU-10          [-1, 128, 16, 16]               0
           Conv2d-11          [-1, 128, 16, 16]         147,456
      BatchNorm2d-12          [-1, 128, 16, 16]             256
             ReLU-13          [-1, 128, 16, 16]               0
         ResBlock-14          [-1, 128, 16, 16]               0
           Conv2d-15          [-1, 256, 16, 16]         294,912
        MaxPool2d-16            [-1, 256, 8, 8]               0
      BatchNorm2d-17            [-1, 256, 8, 8]             512
             ReLU-18            [-1, 256, 8, 8]               0
           Conv2d-19            [-1, 512, 8, 8]       1,179,648
        MaxPool2d-20            [-1, 512, 4, 4]               0
      BatchNorm2d-21            [-1, 512, 4, 4]           1,024
             ReLU-22            [-1, 512, 4, 4]               0
           Conv2d-23            [-1, 512, 4, 4]       2,359,296
      BatchNorm2d-24            [-1, 512, 4, 4]           1,024
             ReLU-25            [-1, 512, 4, 4]               0
           Conv2d-26            [-1, 512, 4, 4]       2,359,296
      BatchNorm2d-27            [-1, 512, 4, 4]           1,024
             ReLU-28            [-1, 512, 4, 4]               0
         ResBlock-29            [-1, 512, 4, 4]               0
        MaxPool2d-30            [-1, 512, 1, 1]               0
           Linear-31                   [-1, 10]           5,130
          Softmax-32                   [-1, 10]               0
================================================================
Total params: 6,573,130
Trainable params: 6,573,130
Non-trainable params: 0
----------------------------------------------------------------
Input size (MB): 0.01
Forward/backward pass size (MB): 6.75
Params size (MB): 25.07
Estimated Total Size (MB): 31.84
----------------------------------------------------------------
```

3. [main.py](https://github.com/Rohithmarktricks/eva8_codebase/blob/main/main.py) contains the helper class ```Trainer``` that helps in training, testing, getting misclassified images (predictions) and other training/testing metrics.
4. [Cifar10Dataset](https://github.com/Rohithmarktricks/eva8_codebase/blob/main/utils/data_utils.py) contains the source code requried for the creation of dataloader (for train/test)
5. [LRFinderCustom](https://github.com/Rohithmarktricks/eva8_codebase/blob/main/utils/lr_finder_custom.py) contains the source code for ```lr_finder``` that can be used to find the optimum ```learning_rate``` for the optimizer.

![alt text](/images/output.png)


#### Setting for the OneCycleLR:
![alt text](/images/lr_values.png)

6. Finally, please refer to [CIFAR_10.ipynb](/CIFAR10_lr.ipynb) Jupyter notebook for the solution of this assignment.

#### Training Logs:
```
Epoch: 1
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.08it/s]
training loss: 1.9938, accuracy: 46.45, Optimzer LR: 1.8791236162361622e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.90it/s]
Test loss: 1.9418, accuracy: 53.50
Epoch: 2
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.11it/s]
training loss: 1.9358, accuracy: 52.93, Optimzer LR: 3.465747232472324e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.92it/s]
Test loss: 1.8949, accuracy: 57.89
Epoch: 3
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.07it/s]
training loss: 1.8896, accuracy: 57.01, Optimzer LR: 5.052370848708487e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.84it/s]
Test loss: 1.8529, accuracy: 62.31
Epoch: 4
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.11it/s]
training loss: 1.8416, accuracy: 62.29, Optimzer LR: 6.638994464944649e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.02it/s]
Test loss: 1.8087, accuracy: 66.87
Epoch: 5
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.12it/s]
training loss: 1.8047, accuracy: 66.27, Optimzer LR: 8.22561808118081e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.96it/s]
Test loss: 1.7730, accuracy: 69.73
Epoch: 6
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.10it/s]
training loss: 1.7748, accuracy: 69.48, Optimzer LR: 9.812241697416973e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.05it/s]
Test loss: 1.7425, accuracy: 74.46
Epoch: 7
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.09it/s]
training loss: 1.7371, accuracy: 74.32, Optimzer LR: 0.00011398865313653137
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.11it/s]
Test loss: 1.7043, accuracy: 77.63
Epoch: 8
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.16it/s]
training loss: 1.7083, accuracy: 77.76, Optimzer LR: 0.00011135752139486151
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.50it/s]
Test loss: 1.7041, accuracy: 77.85
Epoch: 9
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.12it/s]
training loss: 1.6872, accuracy: 79.60, Optimzer LR: 0.00010439325309129009
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.94it/s]
Test loss: 1.7052, accuracy: 77.34
Epoch: 10
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.11it/s]
training loss: 1.6717, accuracy: 81.00, Optimzer LR: 9.742898478771866e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.88it/s]
Test loss: 1.6626, accuracy: 81.41
Epoch: 11
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.07it/s]
training loss: 1.6558, accuracy: 82.75, Optimzer LR: 9.046471648414723e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.80it/s]
Test loss: 1.6538, accuracy: 82.63
Epoch: 12
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.08it/s]
training loss: 1.6465, accuracy: 83.49, Optimzer LR: 8.35004481805758e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.91it/s]
Test loss: 1.6504, accuracy: 82.88
Epoch: 13
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.03it/s]
training loss: 1.6329, accuracy: 85.08, Optimzer LR: 7.653617987700438e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.66it/s]
Test loss: 1.6297, accuracy: 84.70
Epoch: 14
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.06it/s]
training loss: 1.6217, accuracy: 85.89, Optimzer LR: 6.957191157343294e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.49it/s]
Test loss: 1.6212, accuracy: 85.50
Epoch: 15
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.05it/s]
training loss: 1.6110, accuracy: 87.14, Optimzer LR: 6.260764326986151e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.92it/s]
Test loss: 1.6236, accuracy: 85.30
Epoch: 16
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.10it/s]
training loss: 1.6009, accuracy: 88.08, Optimzer LR: 5.5643374966290096e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.27it/s]
Test loss: 1.6071, accuracy: 86.90
Epoch: 17
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.12it/s]
training loss: 1.5899, accuracy: 89.23, Optimzer LR: 4.867910666271867e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.24it/s]
Test loss: 1.6042, accuracy: 86.97
Epoch: 18
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.18it/s]
training loss: 1.5819, accuracy: 89.53, Optimzer LR: 4.1714838359147214e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 12.12it/s]
Test loss: 1.5948, accuracy: 88.22
Epoch: 19
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.16it/s]
training loss: 1.5714, accuracy: 90.68, Optimzer LR: 3.47505700555758e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.03it/s]
Test loss: 1.5879, accuracy: 88.76
Epoch: 20
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.16it/s]
training loss: 1.5629, accuracy: 91.50, Optimzer LR: 2.7786301752004373e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.31it/s]
Test loss: 1.5903, accuracy: 88.52
Epoch: 21
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.10it/s]
training loss: 1.5554, accuracy: 92.43, Optimzer LR: 2.0822033448432946e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.04it/s]
Test loss: 1.5774, accuracy: 89.54
Epoch: 22
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.16it/s]
training loss: 1.5457, accuracy: 92.95, Optimzer LR: 1.3857765144861518e-05
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.66it/s]
Test loss: 1.5715, accuracy: 90.13
Epoch: 23
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:23<00:00,  4.23it/s]
training loss: 1.5380, accuracy: 93.62, Optimzer LR: 6.8934968412900775e-06
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.70it/s]
Test loss: 1.5673, accuracy: 90.34
Epoch: 24
100%|██████████████████████████████████████████████████████████████████████████████████| 98/98 [00:24<00:00,  4.01it/s]
training loss: 1.5319, accuracy: 94.25, Optimzer LR: -7.077146228134988e-08
100%|██████████████████████████████████████████████████████████████████████████████████| 20/20 [00:01<00:00, 13.77it/s]Test loss: 1.5637, accuracy: 90.85
```

