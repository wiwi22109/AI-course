---
layout: post
title: Convolutional Neural Networks
author: [Richard Kuo]
category: [Lecture]
tags: [jekyll, ai]
---


```
from tensorflow.keras import models, layers, datasets
import matplotlib.pyplot as plt

# Load Dataset
mnist = datasets.mnist # MNIST datasets
(x_train_data, y_train),(x_test_data,y_test) = mnist.load_data()

# data normalization
x_train, x_test = x_train_data / 255.0, x_test_data / 255.0 
 
print('x_train shape:', x_train.shape)
print('train samples:', x_train.shape[0])
print('test samples:', x_test.shape[0])

# reshape for input
x_train = x_train.reshape(-1,28,28,1)
x_test  = x_test.reshape(-1,28,28,1)

# Build Model
num_classes = 10 # 0~9

model = models.Sequential()
model.add(layers.Conv2D(32, kernel_size=(5, 5),activation='relu', padding='same',input_shape=(28,28,1)))
model.add(layers.MaxPooling2D(pool_size=(2, 2)))

model.add(layers.Conv2D(64, (5, 5), activation='relu', padding='same'))
model.add(layers.MaxPooling2D(pool_size=(2, 2)))
model.add(layers.Dropout(0.25))

model.add(layers.Flatten())
model.add(layers.Dense(128, activation='relu'))
model.add(layers.Dropout(0.5))
model.add(layers.Dense(num_classes, activation='softmax'))

model.summary() 

model.compile(loss='sparse_categorical_crossentropy', optimizer='adam',metrics=['accuracy'])

# Train Model
epochs = 12 
batch_size = 128

history = model.fit(x_train, y_train, batch_size=batch_size, epochs=epochs, verbose=1, validation_data=(x_test, y_test))

# Save Model
models.save_model(model, 'models/mnist_cnn.h5')

# Evaluate Model
score = model.evaluate(x_train, y_train, verbose=0) 
print('\nTrain Accuracy:', score[1]) 
score = model.evaluate(x_test, y_test, verbose=0)
print('\nTest  Accuracy:', score[1])
print()

# Show Training History
keys=history.history.keys()
print(keys)

def show_train_history(hisData,train,test): 
    plt.plot(hisData.history[train])
    plt.plot(hisData.history[test])
    plt.title('Training History')
    plt.ylabel(train)
    plt.xlabel('Epoch')
    plt.legend(['train', 'test'], loc='upper left')
    plt.show()
	
show_train_history(history, 'loss', 'val_loss')
show_train_history(history, 'accuracy', 'val_accuracy')
```

---
### hiraganamnist
**Dataset:** [Kuzushiji-MNIST](https://github.com/rois-codh/kmnist)<br>
![](https://github.com/rois-codh/kmnist/raw/master/images/kmnist_examples.png)

**Kaggle:** [https://www.kaggle.com/rkuo2000/hiraganamnist](https://www.kaggle.com/rkuo2000/hiraganamnist)<br>

---
### Sign-Language MNIST 
**Dataset:** [Sign-Language MNIST](https://www.kaggle.com/datasets/datamunge/sign-language-mnist)<br>
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/Sign_Language_MNIST.png?raw=true)

21964筆28x28彩色手勢字母圖片之訓練集<br>
5491筆28x28彩色手勢字母圖片之測試集<br>

**Kaggle:** [https://www.kaggle.com/code/rkuo2000/sign-language-mnist](https://www.kaggle.com/code/rkuo2000/sign-language-mnist)

---
### FashionMNIST
**Dataset:** [FashionMNIST](https://www.kaggle.com/zalando-research/fashionmnist)<br>
![](https://github.com/zalandoresearch/fashion-mnist/raw/master/doc/img/fashion-mnist-sprite.png)

28x28 grayscale images<br>
10 classes: [T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot]<br>
60000 train data<br>
10000 test data<br>

**Kaggle:** [https://www.kaggle.com/rkuo2000/fashionmnist-cnn](https://www.kaggle.com/rkuo2000/fashionmnist-cnn)<br>

---
### AirDigit (空中手寫數字)
**Dataset:**<br>
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/AirDigit_dataset.png?raw=true)
* [https://www.kaggle.com/datasets/rkuo2000/airdigit](https://www.kaggle.com/datasets/rkuo2000/airdigit)
* [https://www.kaggle.com/datasets/rkuo2000/airdigit-mpu6050](https://www.kaggle.com/datasets/rkuo2000/airdigit-mpu6050)

**Kaggle:** [https://www.kaggle.com/rkuo2000/airdigit-cnn](https://www.kaggle.com/rkuo2000/airdigit-cnn)<br>

---
### [心電圖診斷理論基礎與系統](http://rportal.lib.ntnu.edu.tw:8080/server/api/core/bitstreams/9ae9fc6a-fa31-4bdf-b3ed-486881f61af8/content)
ECG 心電圖分類：<br>
1. **Normal (正常心跳)**
2. **Artial Premature (早發性心房收縮)**
  - 早發性心房收縮就是心房在收到指令前提早跳動，由於打出的血量少而造成心跳空虛的症狀，再加上舒張期變長，回到心臟的血流增加使下一次的心跳力道較強，造成心悸的感覺，當心臟長期處於耗能異常的狀態，就容易併發心臟衰竭。
3. **Premature ventricular contraction (早發性心室收縮)**
  - 早發性心室收縮是心律不整的一種，是指病人的心室因為電位不穩定而提早跳動，病人會有心跳停格的感覺。因為病人心跳的質量差，心臟打出的血液量不足，卻仍會消耗能量，長期下來就可能衍生出心臟衰竭等嚴重疾病。早發性心室收縮的症狀包括心悸、頭暈、冷汗和胸悶等。
4. **Fusion of ventricular and normal (室性融合心跳)**
  - 室性融合波是由於兩個節律點發出的衝動同時激動心室的一部分形成的心室綜合波，是心律失常的干擾現象範疇
5. **Fusion of paced and normal (節律器融合心跳)**

---
### ECG Classification (心電圖分類)
**Paper:** [ECG Heartbeat Classification: A Deep Transferable Representation](https://arxiv.org/abs/1805.00794)<br>
![](https://d3i71xaburhd42.cloudfront.net/0997b7e7aa68708414fdb3257263f81f9d9c33ae/2-Figure1-1.png)
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/ECG_CNN.png?raw=true)
**Kaggle: [https://www.kaggle.com/rkuo2000/ecg-classification](https://www.kaggle.com/rkuo2000/ecg-classification)

---
### PPG2ABP (PPG偵測動脈血壓)
**Paper:** [PPG2ABP: Translating Photoplethysmogram (PPG) Signals to Arterial Blood Pressure (ABP) Waveforms using Fully Convolutional Neural Networks](https://arxiv.org/abs/2005.01669)<br>
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/PPG2AGP_pipeline.png?raw=true)
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/PPG2AGP_pipeline_output.png?raw=true)
**Code:** [nibtehaz/PPG2ABP](https://github.com/nibtehaz/PPG2ABP)<br>
**Kaggle:** [https://www.kaggle.com/code/rkuo2000/ppg2ab](https://www.kaggle.com/code/rkuo2000/ppg2ab)<br>

---
### Sound Digit CNN (語音數字分類)
**Dataset:** [台語0~9](https://www.kaggle.com/datasets/rkuo2000/sounddigittw)<br>
1. 使用手機錄音App(如: Voice Recorder) 將語音之數字存成.wav
2. 錄音時間長度= 1秒(語音錄得越長，訓練跟辨識的時間都較久)
3. 訓練資料集:每個數字錄20次 0_000.wav, 0_019.wav, 1_001.wav … 1_019.wav, …. 9_000.wav, … 9_019.wav
4. 測式資料集:每個數字錄2次 0_000.wav, 0_001.wav, 1_000.wav, 1_001.wav … 9_000.wav, 9_001.wav
5. 尋找手機檔案夾, 將其命名為SoundDigit, 底下有兩個目錄train (訓練集)跟test (測試集) 並壓縮成SoundDigit.zip
6. 上傳至你的Kaggle.com建立一個新的Dataset (例如: https://kaggle.com/rkuo2000/SoundDigitTW 是台語0~9)

**Kaggle:** [https://www.kaggle.com/code/rkuo2000/sounddigit-cnn](https://www.kaggle.com/code/rkuo2000/sounddigit-cnn/)<br>
* librosa to plot the waveform
```
for i in range(10):
    y, sr = librosa.load('Dataset/Train/'+str(i)+'_000.wav')
    plt.figure()
    plt.subplot(3,1,1)
    librosa.display.waveplot(y, sr=sr)
    plt.title('Waveform')
```
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/Sound_Digit_waveform.png?raw=true)
<br>
* extract MFCC spectorgram
```
x_train = list()
y_train = list()
(row, col) = (40,62)

for FILE in TRAIN_FILES:
    filename = FILE.replace('.3gp','.wav')
    mfcc = extract_feature('Dataset/Train/'+filename)
    # print(mfcc.shape)
    if (mfcc.shape[1]!=col):
        mfcc = np.resize(mfcc, (row,col))
    x_train.append(mfcc)
    y_train.append(FILE[0]) # first charactor of filename is the classname
    if FILE[2:5]=="000":
        print(mfcc.shape)
        display_mfcc(mfcc)  
```
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/Sound_Digit_mel_freq_spectrogram.png?raw=true)

---
### Urban Sound Classification (都市聲音分類)
**Dataset:** [https://www.kaggle.com/chrisfilo/urbansound8k](https://www.kaggle.com/chrisfilo/urbansound8k)<br>
10 classes: air conditioner, car horn, children playing, dog bark, drilling, engine idling, gun shot, jackhammer, siren, street music<br>
**Kaggle:** [https://www.kaggle.com/rkuo2000/urban-sound-classification](https://www.kaggle.com/rkuo2000/urban-sound-classification)<br>
![](https://github.com/rkuo2000/AI-course/blob/gh-pages/images/urban_sound_cnn.png?raw=true)

---
### Speech Commands (語音命令辨識)
**Dataset:** [Google Speech Commands](https://www.kaggle.com/datasets/neehakurelli/google-speech-commands)<br>
Speech Commands: "yes", "no", "up", "down", "left", "right", "on", "off", "stop", "go"<br>

**Code:** [tensorflow/examples/speech_commands](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/speech_commands)<br>
**Kaggle:** [https://www.kaggle.com/code/rkuo2000/qcnn-asr](https://www.kaggle.com/code/rkuo2000/qcnn-asr)<br>

---
## [CNN architectures](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d)

* LeNet-5 (1998) [Gradient-Based Learning Applied to Document Recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf)
![](https://miro.medium.com/max/700/1*aQA7LuLJ2YfozSJa0pAO2Q.png)

* AlexNet (2012) [ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)
![](https://miro.medium.com/max/700/1*2DT1bjmvC-U-lrL7tpj6wg.png)

* VGG-16 (2014)
![](https://miro.medium.com/max/700/1*_vGloND6yyxFeFH5UyCDVg.png)

* Inception-v1 (2014)
![](https://miro.medium.com/max/700/1*KnTe9YGNopUMiRjlEr3b8w.png)

* Inception-v3 (2015)
![](https://miro.medium.com/max/700/1*ooVUXW6BIcoRdsF7kzkMwQ.png)

* ResNet-50 (2015)
![](https://miro.medium.com/max/700/1*zbDxCB-0QDAc4oUGVtg3xw.png)

* Xception (2016)
![](https://miro.medium.com/max/700/1*NNsznNjzULwqvSER8k5eDg.png)

* Inception-v4 (2016)
![](https://miro.medium.com/max/700/1*15sIUNOxqVyDPmGGqefyVw.png)

* Inception-ResNets (2016)
![](https://miro.medium.com/max/700/1*xpb6QFQ4IknSmxmgai8w-Q.png)

* DenseNet (2016)
![](https://miro.medium.com/max/1302/1*Cv2IqVWmiakP_boAJODKig.png)

* ResNeXt-50 (2017)
![](https://miro.medium.com/max/700/1*HelCJiQZEuwuKakRwDdGPw.png)

* EfficientNet (2019)
![](https://1.bp.blogspot.com/-DjZT_TLYZok/XO3BYqpxCJI/AAAAAAAAEKM/BvV53klXaTUuQHCkOXZZGywRMdU9v9T_wCLcBGAs/s640/image2.png)

![](https://1.bp.blogspot.com/-oNSfIOzO8ko/XO3BtHnUx0I/AAAAAAAAEKk/rJ2tHovGkzsyZnCbwVad-Q3ZBnwQmCFsgCEwYBhgL/s640/image3.png)

---
### CNN model comparison
![](https://miro.medium.com/max/1316/1*XoakexX4n9YSEalWxePqqw.png)

---
### HarDNet (2019)
**Paper:** [HarDNet: A Low Memory Traffic Network](https://arxiv.org/abs/1909.00948)<br>
**Blog:** [鎖定高解析串流分析，清大開源CNN架構HarDNet，影像分類速度比常見ResNet-50架構快30%](https://www.ithome.com.tw/news/134809)<br>
**Code:** [PingoLH/Pytorch-HarDNet](https://github.com/PingoLH/Pytorch-HarDNet)<br>
![](https://s4.itho.me/sites/default/files/images/%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202019-12-13%20%E4%B8%8A%E5%8D%8811_07_01.png)

---
### GhostNet (2019)
**Paper:** [GhostNet: More Features from Cheap Operations](https://arxiv.org/abs/1911.11907)<br>
**Code:** [ghostnet_pytorch](https://github.com/huawei-noah/Efficient-AI-Backbones/tree/master/ghostnet_pytorch)<br>
**Blog:** [A Dive Into GhostNet with PyTorch and TensorFlow](https://blog.paperspace.com/ghostnet-cvpr-2020/)<br>

---
### CSPNet (2019)
**Paper:** [CSPNet: A New Backbone that can Enhance Learning Capability of CNN](https://arxiv.org/abs/1911.11929)<br>
**Blog:** [Review — CSPNet: A New Backbone That Can Enhance Learning Capability of CNN](https://sh-tsang.medium.com/review-cspnet-a-new-backbone-that-can-enhance-learning-capability-of-cnn-da7ca51524bf)<br>
**Code:** [https://github.com/WongKinYiu/CrossStagePartialNetworks](https://github.com/WongKinYiu/CrossStagePartialNetworks)<br>
**Cross Stage Partial DenseNet (CSPDenseNet)**<br>
![](https://miro.medium.com/max/1400/1*s_sZ8Lja_JV6ceJBR-7K5A.png)

---
### Vision Transformer (2020)
**Paper:** [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929)<br>
**Code:** [https://github.com/google-research/vision_transformer](https://github.com/google-research/vision_transformer)<br>
![](https://github.com/google-research/vision_transformer/raw/main/vit_figure.png)

---
### RepVGG (2021)
**Paper:** [Making VGG-style ConvNets Great Again](https://arxiv.org/abs/2101.03697)<br>
**Code:** [https://github.com/DingXiaoH/RepVGG](https://github.com/DingXiaoH/RepVGG)<br>
![](https://github.com/DingXiaoH/RepVGG/raw/main/arch.PNG)

---
### NFNets (2021)
**Paper:** [High-Performance Large-Scale Image Recognition Without Normalization](https://arxiv.org/abs/2102.06171)<br>
**Code:** <br>
* [https://github.com/deepmind/deepmind-research/tree/master/nfnets](https://github.com/deepmind/deepmind-research/tree/master/nfnets)
* [https://github.com/vballoli/nfnets-pytorch](https://github.com/vballoli/nfnets-pytorch)
**Tube:** [https://www.youtube.com/embed/rNkHjZtH0RQ](https://www.youtube.com/embed/rNkHjZtH0RQ)<br>
**Blog:** [NFNet (Normalizer-Free ResNets)論文閱讀](https://medium.com/ching-i/nfnet-normalizer-free-resnets-%E8%AB%96%E6%96%87%E9%96%B1%E8%AE%80-ce7235d1b123)<br>
![](https://miro.medium.com/max/1328/1*NdVDJSEOndmUEjsZwdcKRA.png)

---
## Pytorch Image Models (https://rwightman.github.io/pytorch-image-models/models/)

### Big Transfer ResNetV2 (BiT) [resnetv2.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/resnetv2.py)
* Paper: [Big Transfer (BiT): General Visual Representation Learning](https://arxiv.org/abs/1912.11370)
* Code: [https://github.com/google-research/big_transfer](https://github.com/google-research/big_transfer)

### Cross-Stage Partial Networks [cspnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/cspnet.py)
* Paper: [CSPNet: A New Backbone that can Enhance Learning Capability of CNN](https://arxiv.org/abs/1911.11929)
* Code: [https://github.com/WongKinYiu/CrossStagePartialNetworks](https://github.com/WongKinYiu/CrossStagePartialNetworks)

### DenseNet [densenet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/densenet.py)
* Paper: [Densely Connected Convolutional Networks](https://arxiv.org/abs/1608.06993)
* Code: [https://github.com/pytorch/vision/tree/master/torchvision/models](https://github.com/pytorch/vision/tree/master/torchvision/models)

### DLA [dla.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/dla.py)
* Paper: [Deep Layer Aggregation](https://arxiv.org/abs/1707.06484)
* Code: [https://github.com/ucbdrive/dla](https://github.com/ucbdrive/dla)

### Dual-Path Networks [dpn.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/dpn.py)
* Paper: [Dual Path Networks](https://arxiv.org/abs/1707.01629)
* Code: [https://github.com/rwightman/pytorch-dpn-pretrained](https://github.com/rwightman/pytorch-dpn-pretrained)

### GPU-Efficient Networks [byobnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/byobnet.py)
* Paper: [Neural Architecture Design for GPU-Efficient Networks](https://arxiv.org/abs/2006.14090)
* Code: [https://github.com/idstcv/GPU-Efficient-Networks](https://github.com/idstcv/GPU-Efficient-Networks)

### HRNet [hrnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/hrnet.py)
* Paper: [Deep High-Resolution Representation Learning for Visual Recognition](https://arxiv.org/abs/1908.07919)
* Code: [https://github.com/HRNet/HRNet-Image-Classification](https://github.com/HRNet/HRNet-Image-Classification)

### Inception-V3 [inception_v3.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/inception_v3.py)
* Paper: [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/abs/1512.00567)
* Code: [https://github.com/pytorch/vision/tree/master/torchvision/models](https://github.com/pytorch/vision/tree/master/torchvision/models)

### Inception-V4 [inception_v4.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/inception_v4.py)
* Paper: [Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning](https://arxiv.org/abs/1602.07261)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### Inception-ResNet-V2 [inception_resnet_v2.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/inception_resnet_v2.py)
* Paper: [Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning](https://arxiv.org/abs/1602.07261)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### NASNet-A [nasnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/nasnet.py)
* Papers: [Learning Transferable Architectures for Scalable Image Recognition](https://arxiv.org/abs/1707.07012)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### PNasNet-5 [pnasnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/pnasnet.py)
* Papers: [Progressive Neural Architecture Search](https://arxiv.org/abs/1712.00559)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### EfficientNet [efficientnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/efficientnet.py)
* Paper: [EfficientNet (B0-B7)](https://arxiv.org/abs/1905.11946)
  - [EfficientNet NoisyStudent (B0-B7, L2)](https://arxiv.org/abs/1911.04252)
  - [EfficientNet AdvProp (B0-B8)](https://arxiv.org/abs/1911.09665)
* Code: [https://github.com/rwightman/gen-efficientnet-pytorch](https://github.com/rwightman/gen-efficientnet-pytorch)

### MobileNet-V3 [mobilenetv3.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/mobilenetv3.py)
* Paper: [Searching for MobileNetV3](https://arxiv.org/abs/1905.02244)
* Code: [https://github.com/tensorflow/models/tree/master/research/slim/nets/mobilenet](https://github.com/tensorflow/models/tree/master/research/slim/nets/mobilenet)

### RegNet [regnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/regnet.py)
* Paper: [Designing Network Design Spaces](https://arxiv.org/abs/2003.13678)
* Code: [https://github.com/facebookresearch/pycls/blob/master/pycls/models/regnet.py](https://github.com/facebookresearch/pycls/blob/master/pycls/models/regnet.py)

### RepVGG [byobnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/byobnet.py)
* Paper: [Making VGG-style ConvNets Great Again](https://arxiv.org/abs/2101.03697)
* Code: [https://github.com/DingXiaoH/RepVGG](https://github.com/DingXiaoH/RepVGG)

### ResNet, ResNeXt [resnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/resnet.py)
**ResNet (V1B)**<br>
* Paper: [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)
* Code: [https://github.com/pytorch/vision/tree/master/torchvision/models](https://github.com/pytorch/vision/tree/master/torchvision/models)

**ResNeXt**<br>
* Paper: [Aggregated Residual Transformations for Deep Neural Networks](https://arxiv.org/abs/1611.05431)
* Code: [https://github.com/pytorch/vision/tree/master/torchvision/models](https://github.com/pytorch/vision/tree/master/torchvision/models)

**ECAResNet (ECA-Net)**<br>
* Paper: [ECA-Net: Efficient Channel Attention for Deep CNN](https://arxiv.org/abs/1910.03151v4)
* Code: [https://github.com/BangguWu/ECANet](https://github.com/BangguWu/ECANet)

### Res2Net [res2net.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/res2net.py)
* Paper: [Res2Net: A New Multi-scale Backbone Architecture](https://arxiv.org/abs/1904.01169)
* Code: [https://github.com/gasvn/Res2Net](https://github.com/gasvn/Res2Net)

### ResNeSt [resnest.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/resnest.py)
* Paper: [ResNeSt: Split-Attention Networks](https://arxiv.org/abs/2004.08955)
* Code: [https://github.com/zhanghang1989/ResNeSt](https://github.com/zhanghang1989/ResNeSt)

### ReXNet [rexnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/rexnet.py)
* Paper: [ReXNet: Diminishing Representational Bottleneck on CNN](https://arxiv.org/abs/2007.00992)
* Code: [https://github.com/clovaai/rexnet](https://github.com/clovaai/rexnet)

### Selective-Kernel Networks [sknet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/sknet.py)
* Paper: [Selective-Kernel Networks](https://arxiv.org/abs/1903.06586)
* Code: [https://github.com/implus/SKNet](https://github.com/implus/SKNet), [https://github.com/clovaai/assembled-cnn](https://github.com/clovaai/assembled-cnn)

### SelecSLS [selecsls.py]
* Paper: [XNect: Real-time Multi-Person 3D Motion Capture with a Single RGB Camera](https://arxiv.org/abs/1907.00837)
* Code: [https://github.com/mehtadushy/SelecSLS-Pytorch](https://github.com/mehtadushy/SelecSLS-Pytorch)

### Squeeze-and-Excitation Networks [senet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/senet.py)
* Paper: [Squeeze-and-Excitation Networks](https://arxiv.org/abs/1709.01507)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### TResNet [tresnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/tresnet.py)
* Paper: [TResNet: High Performance GPU-Dedicated Architecture](https://arxiv.org/abs/2003.13630)
* Code: [https://github.com/mrT23/TResNet](https://github.com/mrT23/TResNet)

### VGG [vgg.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/vgg.py)
* Paper: [Very Deep Convolutional Networks For Large-Scale Image Recognition](https://arxiv.org/pdf/1409.1556.pdf)
* Code: [https://github.com/pytorch/vision/blob/master/torchvision/models/vgg.py](https://github.com/pytorch/vision/blob/master/torchvision/models/vgg.py)

### Vision Transformer [vision_transformer.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/vision_transformer.py)
* Paper: [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929)
* Code: [https://github.com/google-research/vision_transformer](https://github.com/google-research/vision_transformer)

### VovNet V2 and V1 [vovnet.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/vovnet.py)
* Paper: [CenterMask : Real-Time Anchor-Free Instance Segmentation](https://arxiv.org/abs/1911.06667)
* Code: [https://github.com/youngwanLEE/vovnet-detectron2](https://github.com/youngwanLEE/vovnet-detectron2)

### Xception [xception.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/xception.py)
* Paper: [Xception: Deep Learning with Depthwise Separable Convolutions(https://arxiv.org/abs/1610.02357)
* Code: [https://github.com/Cadene/pretrained-models.pytorch](https://github.com/Cadene/pretrained-models.pytorch)

### Xception (Modified Aligned, Gluon) [gluon_xception.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/gluon_xception.py)
* Paper: [Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation](https://arxiv.org/abs/1802.02611)
* Code: [https://github.com/dmlc/gluon-cv/tree/master/gluoncv/model_zoo](https://github.com/dmlc/gluon-cv/tree/master/gluoncv/model_zoo), [https://github.com/jfzhang95/pytorch-deeplab-xception/](https://github.com/jfzhang95/pytorch-deeplab-xception/)

### Xception (Modified Aligned, TF) [aligned_xception.py](https://github.com/rwightman/pytorch-image-models/blob/master/timm/models/aligned_xception.py)
* Paper: [Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation](https://arxiv.org/abs/1802.02611)
* Code: [https://github.com/tensorflow/models/tree/master/research/deeplab](https://github.com/tensorflow/models/tree/master/research/deeplab)

<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*

