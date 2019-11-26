---
layout: post
title: 'DeepLearning Part2 : 케라스를 이용한 딥러닝 구조 정의'
author: luke
date: 2019-10-28 13:11
categories: [techCamp]
tags: [ai]
comments: true
---
## 케라스를 이용한 딥러닝 구조 정의

### 전체 구성도
그림 필요

### 모델 구성
* Model : CNN 등, 특징을 뽑아내고, 예측을 하는 과정에 대한 정의 (함수와 비슷한의미?)
* Layer : 모델은 레이어를 여러겹을 쌓아 올려서 만들어 진다. 여러층을 쌓았기 때문에 딥러닝이라고 함
  * Layer를 여러개를 사용하는 것은 다양한 피처들을 추출하기 위함이다.

[케라스모델 시작하기] https://keras.io/ko/getting-started/sequential-model-guide/

###### Feature Extraction Layers

1단계. Convolution Layer (합성곱): 특징 추출 연산
   * 특징 추출 이미지에 이미지필터 적용하는 연산이다. 
   * 이미지 전처리 과정에서 픽셀별로 적용되는 이미지 전처리 필터를 의미함
   * 특징을 추출하는 과정
   
2단계. Pooling Layer : 이미지를 압축하는 과정

   * 32x32 > 16x16 등으로 축소하는 과정
``` 
   * Max Pooling : 영역에서 가장 큰 값만 추출
``` 

###### Fully Connected Layers
3단계. Fully Connected Layers
``` 
   * Dense : Just your regular densely-connected NN layer.
``` 

###### Layer에 적용되는 Activation Function (Or Layer)
Activation Function : 불필요한 값 제거 과정
``` 
   * ReLU : 음수값 제거 (음수는 0으로) 
   * Sigmoid :
   * Maxout :
   * SoftMax : 결과값을 전체 합이 1인 확률값으로 표현
```

###### Sample Code
```python
from keras.models import Sequential
from keras.layers import Dense, Activation

model = Sequential([
    Dense(32, input_shape=(784,)),
    Activation('relu'),
    Dense(10),
    Activation('softmax'),
])
``` 


###  모델 컴파일 단계

* Loss(Cost) Function : 훈련과정에서 실제값과 예측값의 차이를 정의하는 함수

* Optimization : Loss를 줄이 수 있도록 모델을 업데이트해가는 과정
```    
    * Adam : Adaptive Moment Estimation
    * SGD : 확률적 경사 하강법(Stochastic Gradient Descent, SGD) 옵티마이저
    * RmsProp: 
```

* Metrics
    * 측정항목은 모델의 성능을 평가하는데 사용되는 함수입니다
    * Loss Function과 기능은 동일하지만, Matrics는 학습용으로 사용하지 않는다
```
  * MAE : Mean Abs Error
  * MSE : Mean Square error
```

###### Sample Code
```python
model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy'])
``` 
              
####  학습 과정
* [케라스 모델 함수] https://keras.io/ko/models/sequential/
* 하이퍼파라미터 
   * 학습과정에서 사용자가 설정해주는 값들

    * Learning Rate : Cost를 낮추기 위해 파라미터를 조절하는 과정
    * Epoch : 같은 이미지를 몇번 반복해서 훈련시킬 것인가
    * Batch Size : 트레이닝시에 한번에 몇장의 이미지를 입력할 것인가

* Overfiting : 과적합

```python
# Train the model, iterating on the data in batches of 32 samples
model.fit(data, labels, epochs=10, batch_size=32)              
``` 

#### 평가 & 예측

```
loss, mae, mse = model.evaluate(normed_test_data, test_labels, verbose=2)
print("테스트 세트의 평균 절대 오차: {:5.2f} MPG".format(mae))

test_predictions = model.predict(normed_test_data).flatten()
```

#### 모델의 저장과 로딩
```

```

#### 기타 용어
* Train Set: 학습지 풀기
* Test Set: 모의고사 풀기

* Label : 데이타의 정답지
* Ground Truth : 정답?

### 레퍼런스
[케라스 가이드] 
https://keras.io/
https://keras.io/ko

[옵티마이저에 대한 개념정의]
http://shuuki4.github.io/deep%20learning/2016/05/20/Gradient-Descent-Algorithm-Overview.html
