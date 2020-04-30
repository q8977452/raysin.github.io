---
layout: post
title:  "Objection Detection Introduction"
date:   2020-03-29 15:12:30 +0800
categories: Objection_Detection
tags : 物件偵測 Classification Localization Bounding-Box-Regression One-Stage Two-Stage
mathjax: true
---
* content 
{:toc}
2-nd-DL-CVMarathon Learning Notes2




## 什麼是Object Detection

與一般基本的 CNN 分類器不同的是，Object Detection 能告訴你**物件**在照片中的**位置**，並同時能偵測**多個**不同的物件

## How?
如何做到偵測**物件**在照片中的**位置**並同時能偵測**多個**不同的物件？
* 透過模型架構與 Loss Function 設計使我們能同時定位物件並偵測物件

### Loss Function

儘管 Object Detetcion 的演算法很多種，Loss Function 都不會脫離兩大核心。
* Bounding Box Regression
	Loss 的其中一項就在學習怎麼將這個預測框精準的定位( 一種 Regression )，使其完整的包涵物件。
	
* Classification
	當我們成功定位物件後，使用與一般 CNN 分類器相同的Loss ( Softmax+Cross Entropy )即可。

---		

Object Detection 是同時在學習『定位』與『分類』這兩件事情，所以可以共享 CNN 層。

## 分類

* One Stage
		
	* 是一次到位。
		
	* 直接對 Default Bounding Box 進行回歸與分類。
		* Default Bounding Box :直接以 Pixels 為中心點提出『Region Proposal』
		
	
* Two Stage 
		
	* 先找出 Region Proposal，再做回歸與分類

	* 方法
			
		* Selective Search 
			是圖像 segmentation 的一種，其主要是基於圖像的顏色，紋理，大小和形狀兼容計算相似區域的分層分組。
				 
			* Hierarchical Grouping Algorithm
				 
				1. 計算所有鄰近區域之間的相似性
				2. 兩個最相似的區域被組合在⼀起
				3. 計算合併區域和相鄰區域的相似度
				4. 重複 2、3 過程，直到整個圖像變為多個地區，，由於 RCNN 預設是 1000-2000 個proposal，所以分到一定數量左右就會停止。
				 
			* Diversification Strategies
				 
				1. 利用各種不同不變性的色彩空間
				2. 採用不同的相似性度量，如顏色、紋理、尺度等。

					
		* RPN

兩者主要差異在於是否需要『透過特定運算算法』來提出可能包含物件的『框 ( Region Proposal )』

## Reference

* [Object Detection Sliding Window](https://www.youtube.com/watch?v=5e5pjeojznk)
* [Selective Search 原理與教學](selective)