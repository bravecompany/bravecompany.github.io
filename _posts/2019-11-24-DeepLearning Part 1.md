---
layout: post
title: 'DeepLearning Part 1: 딥런닝에 대하여'
author: luke
date: 2019-10-28 13:11
categories: [techCamp]
tags: [ai]
comments: true
---

용감한테크캠프 
-----------------
###### 주제: Deep-Learning에 대한 이해와 Tensorflow 구조
###### 발표자: 이종옥


머신런닝의 목적
----------------
##### 머신러닝(딥러닝)으로 무엇을 해결하려고 하는가?
> 주어진 데이타에 가장 근접한 Y=wX+b의 방정식의 w(Weight),b(Bias)값을 찾아라
<img src="{{'/assets/post_assets/post1/그래프.png'}}" width="70%" height="70%" title="" alt=""></img>
    * X는 형태와 성격 따라 Input variable, feature 등으로 불려집니다.
    * X는 스칼라 값(1차원) 이기도 하고, 벡터값(2차원) 이기도 하고, 그 이상의 차수를 가진 다차원행렬(Tensor)일 수도 있습니다.
    * w는 feature들에 주어지는 가중치(Weight) 값이며, 
    * b는 feature의 시작값을 조절하는 바이어스(Bias) 값입니다.


딥러닝의 역사
-----------------
##### 머신러닝의 시작 (1980)
>머신러닝의 개념은 1980년에 등장하였다. (Kunihiko Fukushima) 하지만, 이 당식 뉴럴네트웍을 이용한 머신 러닝은 Overfitting이 심각하였고, 일정 수준 이상의 러닝을 시도할 수록 추가적인 학습이 되지 않는 현상이 발생하였다. 결론적으로 효과적이지 못한 방법으로 인식되었다.
* Overfitting : 훈련데이타에 최적화 되어버리는 현상

##### 딥러닝의 등장 (2000)
>2000년대 제프리힌튼과 Ruslan Salakhutdinov에 의해  신경망을 여러겹으로 쌓아서 학습시키는 방식을 이용하여 오버피팅 문제를 해결할 수 있는 방법을 고안하게 된다. 
☛ Deep-Learning이라는 용어는 여러 계층으로 쌓여진 신경망을 통해 학습시칸다는 의미를 내포하고 있습니다.


모델의 구성 (딥러닝 모델은 어떻게 구성되어 있나?)
--------
<img src="{{'/assets/post_assets/post1/image2.png'}}" width="70%" height="70%" title="" alt=""></img>

##### Model의 구성
   + Feature Extraction Layers
        * Convolution Layer : 특징 추출
          * Pooling Layer : 데이타 압축
             * Activation Layer : 불필요한 데이타 제거
   + Fully Connected  (Classification Layers)
       * 예측을 담당하는 레이어
       * ex) Dense
   + SoftMax Layer : 확률로 결과값 Normalization

##### 잘아려진 Model의 사례
<img src="{{'/assets/post_assets/post1/image3.png'}}" width="70%" height="70%" title="" alt=""></img>

<img src="{{'/assets/post_assets/post1/image4.png'}}" width="70%" height="70%" title="" alt=""></img>


학습 과정 (Loss/Optimization)
--------
<img src="{{'/assets/post_assets/post1/image5.png'}}" width="70%" height="70%" title="" alt=""></img>


* Loss(Cost) Function : 얼마나 틀렸는지 계산하는 함수
* Optimization : Loss를 줄이 수 있도록 모델을 업데이트해가는 과정
    * Adam, SGD 등이 있음
    * 경사하강학습


실전 코딩
----------------
#### 필기체 인식을 위한 Tensorflow 실전 코드

숙제! 필기체로 작성된 숫자(0~9)를 인식할 수 있는 딥런닝 코드를 작성해봅시다. 이 코드를 작성하는데 몇 라인의 코드가 필요할까요?




----------------------
참고자료
-----
* TensorFlow https://www.tensorflow.org/
* 딥러닝 위키 https://ko.wikipedia.org/wiki/%EB%94%A5_%EB%9F%AC%EB%8B%9D
* 딥러닝과 머신러닝의 차이 https://brunch.co.kr/@itschloe1/8

