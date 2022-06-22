# 딥러닝-1

#### 딥러닝이란? DNN : Deep Neural Networks 을 학습시켜 문제해결 하는 것

인공신경망 ANN Artificial Neural Networks 의 적통을 이어받음

기존신경망에비하여 **더깊은구조**를 갖는것이 **특징**



why DNN? 

비선형 함수로 기존 ML에 비해 **패턴인식**능력이 월등

이미지,텍스트,음성 분야에서 비약적 성능개선

> 기존 ML과 달리 **hand-crafted feature가 필요 없음**
>
> 단순히 raw 값을 넣는 것으로, **자동으로 특징(feature)을 학습**함



**딥러닝의 역사와 패러다임의 변화**

• 기존 패러다임

> **Hand-crafted feature**를 추출하여 머신러닝 모델에 넣고 학습.  {가정들을 도입함}
>
> **여러 단계의 sub-module**로 이루어져 있었음 • e.g.음성인식,기계번역등 {데이터가 무겁고 느리다}

• 새로운 패러다임  {성능 월등함}

> Raw 값을 신경망에 넣으면, **자동으로 특징(feature)을 학습** 
>
> 하나의 task에 대해서, 하나의 신경망 모델이 존재하는 **end-to-end 방식** {사람이 해석하기 힘들다}



- s오픈소스문화 + 빠른(논문)공유

> gitHub, arXiv.org



### 인공지능 모델이란?



- x가 주어졌을때, y를 반환하는 함수  y=f(x)

- **파라미터** (weight parameter, **𝜃** ) : 함수가 어떻게 동작하는지 결정해준다(x가 들어왔을때 어떤 y를 돌려줄것인가)

- **학습**이란:
  -  𝑥와𝑦의쌍으로이루어진데이터가주어졌을때,**𝑥로부터𝑦로가는관계를배우는것** 
  -  x와 y를 통해 적절한 파라미터(𝜃)를 찾아내는 것

- 모델이란
    상황에 따라 알고리즘 자체를 이르거나, 파라미터를 이름

#### 좋은 인공지능 모델이란?

- 일반화(Generalization)를 잘 하는 모델
  - 보지 못한(unseen) 데이터에 대해서 좋은 예측(prediction)을 하는 모델 
  - 모든경우의수에대해서데이터를모을수없기때문
  - 보지못한경우에대해서, 모델은좋은판단을내릴수있어야함



#### 기존 머신러닝의 한계

- 기존 머신러닝은 주로 선형 또는 낮은 차원의 데이터를 다루기 위해 설계되었음

- Kernel 등을 사용하여 비선형 데이터를 다룰 수 있지만, 한계가 명확함
  - 이미지,텍스트,음성과같은훨씬더높은차원의데이터들에대해낮은성능을보임



#### Objective

- 주어진 데이터에 대해서 결과를 내는 가상의 함수를 모사하는 함수를 만드는 것 
  -  e.g.주어진 숫자 그림을 보고 숫자맞추기



#### Working process

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622150141779.png" alt="image-20220622150141779" style="zoom:50%;" />



#### 문제정의

- 가장 중요한 부분!!!!
- 풀고자하는 문제를 **단계별로 나눈다** {simply}
- 신경망이라는 함수에 넣기위한 x와 결과y가 정의 되어야한다
  - y=f(x)

#### 데이터수집

- 문제정의에 따라 정해진 x와 y.
- 풀고자 하는 **문제의 영역**에 따라 **수집방법**도 다르다
- 필요에 따라 레이블링(Labeling)작업을 수행 <u>{직접해보면서 노하우를 기르자!}</u>
  - 자동적으로 레이블(label)이 y로 주어질 수도있음
  - 하지만 대부분의 경우 레이블이 따로 필요함
  - 비지도학습을 기대하지말자 (unsupervised learning)



#### 데이터 전처리 및 분석

- 수집된 데이터를 신경망에 넣어주기위한 형태로 가공하는 과정 (cleaning & normalization)

- 탐험적분석(Exploratory Data Analaysis EDA) 필요
  - 데이터에 알맞는 알고리즘을 찾기위함 / NLP CV에서 생략
- CV영상처리 의 경우 데이터증강 (augmentation)이 수행됨
  - Rotation, flipping, shifting 등의 간단한 연산



#### 알고리즘 적용

- 데이터에 대해 **가설**을 세우고, 해당 가설을 위한 **알고리즘(모델)**을 **적용**



#### 평가

- 문제 정의에 따른 공정하고 올바흔 평가방법 필요
  - 가설을 검증하기 위한 실험 설계

- 테스트 셋 test set 구성
  - 너무 쉽거나 어렵다면 판별떨어짐
  - 실제 데이터와 가장 가깝게 구성되어야 함
- **정량적 평가** extrinsic evaluation 와 **정성적 평가** intrinsic evaluation로 나뉨





#### 베포

- 학습과 평가가 완료된 모델 weights 파일을 배포함
- RESTful API 등을 통해 wrapping 후 배포
- 데이터의 분포의 변화에 따른 모델 업데이트 및 유지/보수가 필요할 수 있음





## Appendix : Basic Math

### 지수와 로그

- 지수함수

![image-20220622161320085](/Users/krc/Library/Application Support/typora-user-images/image-20220622161320085.png)

- 지수 기본 법칙

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161409536.png" alt="image-20220622161409536" style="zoom:50%;" />

- 지수 연산 법칙

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161523545.png" alt="image-20220622161523545" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161600766.png" alt="image-20220622161600766" style="zoom:50%;" />

- 로그함수

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161740744.png" alt="image-20220622161740744" style="zoom:50%;" />

- 지수와 로그의 관계



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161842699.png" alt="image-20220622161842699" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161902638.png" alt="image-20220622161902638" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622162300418.png" alt="image-20220622162300418" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622161921309.png" alt="image-20220622161921309" style="zoom:50%;" />

#### Summation & product

- sum

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622162046938.png" alt="image-20220622162046938" style="zoom:50%;" />

- product

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622162407075.png" alt="image-20220622162407075" style="zoom:50%;" />

- argmax
  - pick the **argument** that makes **max value**: 최대값을 만드는 argment

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622174250479.png" alt="image-20220622174250479" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622174305724.png" alt="image-20220622174305724" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220622174242694.png" alt="image-20220622174242694" style="zoom:50%;" />