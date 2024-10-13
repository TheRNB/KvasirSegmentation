# KvasirSegmentation
**This is an implementation of U-Net model for Kvasir-SEG dataset for Polyp Segmentation in internal digestion system photos.**

<p align="center">
  <img src="https://github.com/TheRNB/KvasirSegmentation/blob/main/exampleSegment.png" width="512">
</p>

# Execution
The requirements are as follows:
```
  matplotlib==3.8.2
  numpy==1.26.4
  scikit-image==0.22.0
  scikit-learn==1.3.2
  tensorflow==2.15.0
  tqdm==4.64.1
```

# Description
## Dataset
This model is trained on the Kvasir-SEG dataset freely available for educaitonl use on [here](https://datasets.simula.no/kvasir-seg/).

## Model Design
This model uses the U-Net architecture introduced on [this wonderful paper](https://arxiv.org/abs/1505.04597) by Ronneberger. My implementation consists of 3 different sized models to test the effect of size on segmentation accuracy and overall accuracy:
* The first implementation of the UNet model is an encoder-decoder with these layers:
  + Encoder:
    - 2 × ((3, 3), 16) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 32) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 64) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 128) Convulotion layer + (2, 2) Maxpooling layer
  + Bottleneck:
    - 2 × ((3,3), 256) Convulotion
  + Decoder:
    - ((3, 3), 128) Convolution Transpose layer + 2 × ((3, 3), 128) Convulotion + (2, 2) Maxpooling layer
    - ((3, 3), 64) Convolution Transpose layer + 2 × ((3, 3), 64) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 32) Convolution Transpose layer + 2 × ((3, 3), 32) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 16) Convolution Transpose layer + 2 × ((3, 3), 16) Convulotion + (2, 2) Maxpooling layer
    
* The second implementation is a lopsided encoder-decoder with these layers:
  + Encoder:
    - 2 × ((3, 3), 32) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 64) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 128) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 256) Convulotion layer + (2, 2) Maxpooling layer
  + Bottleneck:
    - 2 × ((3,3), 512) Convulotion
  + Decoder:
    - ((3, 3), 128) Convolution Transpose layer + 2 × ((3, 3), 256) Convulotion + (2, 2) Maxpooling layer
    - ((3, 3), 64) Convolution Transpose layer + 2 × ((3, 3), 128) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 32) Convolution Transpose layer + 2 × ((3, 3), 64) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 16) Convolution Transpose layer + 2 × ((3, 3), 32) Convulotion + (2, 2) Maxpooling layer
   
* The third and final implementation is an encoder-decoder with these layers:
  + Encoder:
    - 2 × ((3, 3), 16) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 32) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 64) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 128) Convulotion layer + (2, 2) Maxpooling layer
    - 2 × ((3, 3), 256) Convulotion layer + (2, 2) Maxpooling layer
  + Bottleneck:
    - 2 × ((3,3), 512) Convulotion
  + Decoder:
    - ((3, 3), 256) Convolution Transpose layer + 2 × ((3, 3), 256) Convulotion + (2, 2) Maxpooling layer
    - ((3, 3), 128) Convolution Transpose layer + 2 × ((3, 3), 128) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 64) Convolution Transpose layer + 2 × ((3, 3), 64) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 32) Convolution Transpose layer + 2 × ((3, 3), 32) Convulotion + (2, 2) Maxpooling layer
    - ((3,3), 16) Convolution Transpose layer + 2 × ((3, 3), 16) Convulotion + (2, 2) Maxpooling layer

# Results
  Using these models we can achieve these accuracies on test data:
  |  | Model 1 | Model 2 | Model 3 |
  | --- | --- | --- | --- |
  | **Accuracy** | **92.5312%** | **91.5110%** | **92.8794%** |

  Also calculating the DICE scores on test data, we get:
  |  | Model 1 | Model 2 | Model 3 |
  | --- | --- | --- | --- |
  | **DICE** | **72.56** | **71.31** | **76.60** |

# Conclusion
As seen by accuracy values, it seems that size has an indistinguishable difference on the accuracy, as all numbers are in margin of error of each other. However, as clearly seen by the DICE metric, The bigger models help with finding the location of the polyp more accurately.

# Contact
If there are any problems or questions about the model, feel free to reach out at aaron@bateni.org.
