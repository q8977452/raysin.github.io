---
layout: post
title:  "Objection Detection Evolution"
date:   2020-03-29 22:32:30 +0800
categories: Objection_Detection
tags : 物件偵測 One-Stage Two-Stage R-CNN Fast_R-CNN Faster_R-CNN YOLO SSD RetinaNet
mathjax: true
---
# Objection Detection Evolution

## Two Stage

### R-CNN : Regions with CNN features

* 特色

	* 運用Selective Search 提出Region Proposal
	* Resize到相同到大小
	* 透過SVM做分類，分類完再使用Regrssor修正BBOX

* 問題

	* 那 R-CNN 有什麼樣的問題呢？
	* 簡而言之就是『速度慢』，其原因主要有以下兩點
	
		1. 經由 Selective Search 提出的 Region Proposal 都要獨自經過CNN 做特徵提取，運算速度相當緩慢。
		
		2. 經過 CNN 得到 feature map，再用這些 feature 當成 SVM 的input 當成訓練資料，
		因此並不是一個 end-to-end 模型 ( SVM 的LOSS 並不會改動到 feature map 的數值)

### Fast_R-CNN

* 特色
	
	* 運用Selective Search 提出Region Proposal
	* 整張圖片送進去 CNN 提取特徵
	* ROI Pooling 是⼀種能將不同 Region proposals (候選框)，Pooing 成⼀樣⼤⼩的⼀個⽅法。
	
		* ROI 就是 Region Proposals
	
	* Loss Function

		* Softmax 取代原本SVM
		* 同時做BBOX regression
* 問題

Fast R-CNN 解決了什麼問題？

	* 原本每一個 Region Proposal 都要經過 CNN 提取特徵現在只需要將整張圖送入，速度上有很大的優勢。
	
	* 運用Softmax 取代 SVM，並同時加入了 BBOX regression 到模型中與分類一起收斂。

Fast R-CNN 有什麼問題？

	*  還是需要透過 Selective Search 提出 Region Proposal，沒辦法統整成 End-to-End model 訓練。

### Faster_R-CNN

* 特色

	* 原圖直接通過 CNN 提取特徵
	* 透過 RPN(Region Proposal Network) 結構提出 Region Proposal
	* 一樣要運用 ROI Pooling 將候選框變成一樣大小，後面結構則是跟 FastRCNN相同
	
* 問題

Faster R-CNN 解決了什麼問題？
		
	* 不用 Selective Search，而改用 (Region Proposal Network) 來提取proposals ，達到真正的 End-to-End
	
Faster R-CNN 有什麼問題？

	* 還是需要先提出 Region Proposal，整體而言速度受到一定的限制。
	
## One Stage
One Stage 核心觀念-不要先浪費時間提出 Region Proposal，而是以 Default Anchor Box 取代

### YOLO系列V1-V3
![Yolo 與 RCNN 系列的比較](https://github.com/q8977452/q8977452.github.io/tree/master/images/objection-detection-evolution-1.png)

YoloV1解決了什麼問題
	
* 不用花費時間提出 Region Proposal。

YoloV1 有什麼問題？

* 只透過最後一層預測，而由於最後一層的 Feature Map尺吋只有7*7，已經喪失許多空間訊息，因此對小物件不敏感。
* FC層造成空間訊息損失。
	
### SSD(Single Shot Multibox Dectector) 

SSD 解決了什麼問題

* 多尺度預測，對於物件更敏感。

SSD 有什麼問題？

* 正負樣本不平均，太多背景，使用 Online hard example mining(OHEM)使易分類樣品消失 。
* 運用淺層 Feature Map 偵測小物件，但特徵訊息不夠豐富對小物件敏感度仍比不上 Faster R-CNN。

### RetinaNet
RetinaNet 的主架構與 SSD 很像，重點在於加入了 Feature Pyramid Network (FPN) 的結構，把淺層的 Feature Map 與深層的 Feature Map 疊加後做預測，確保淺層語義訊息也能夠豐富。

RetinaNet 解決了什麼問題?

* 多尺度預測，對小物件更敏感。

RetinaNet 有什麼問題？

* Focal Loss 中 Hyperparameters 的調整對結果影響大。

## Reference

* [Object Detection SppNet](https://zhuanlan.zhihu.com/p/24774302)
* [Object Detection ROIPooling](https://blog.csdn.net/JNingWei/article/details/78822159)
* [Object Detection OHEN與Focal Loss](https://www.itread01.com/content/1543549147.html)