---
layout: post
title:  "Course : Deep Learning for Human Language Processing - Speech Separation"
date:   2020-04-12 23:39:30 +0800
categories: NLP
tags : NLP Speech-Separation TasNet
mathjax: true
---
# TasNet - Time-domain Audio Separation Network
## Architecture
![](https://i.imgur.com/q59RTC7.png)

* Encoder
    
	* 512-d : 不一定要positive
    
	![](https://i.imgur.com/5AADpzn.png)

* Decoder
    
	* 跟Encoder互為Inverse的效果沒比較好

    ![](https://i.imgur.com/TnXlyBr.png)

* Separator
    * Network Compression
        * Depthwise Separable Convolution
            * Steps
                1. Depthwise Convolution 
                    * Filter number = Input channel number
                    * Each filter only consider on channel
                    * The filters are k x k matrices
                    * There is no interaction between channels
                2. Pointwise Convolution
                    * 1 x 1 filter
            * Application
                * SqueezNet
                * MobileNet
                * ShuffleNet
                * Xception

    ![](https://i.imgur.com/myJBKC9.png)

## Experiment
![](https://i.imgur.com/oeNTNma.png)


## Reference
* [Conv-TasNet: Surpassing Ideal Time-Frequency Magnitude Masking for Speech Separation, Y Luo, 2019](https://arxiv.org/abs/1809.07454)
* [李宏毅-Speech Separation (2/2) - TasNet](https://www.youtube.com/watch?v=G0O1A7lONSY)
* [Speech Separation-李宏毅 HUNG-YI LEE](http://speech.ee.ntu.edu.tw/~tlkagk/courses/DLHLP20/SP%20(v3).pdf)
* [李宏毅-Network Compression (5/6)
](https://www.youtube.com/watch?v=L0TOXlNpCJ8&feature=youtu.be)