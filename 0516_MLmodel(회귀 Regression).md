# 머신러닝 모델 (회귀 Regression)

[회귀와 분류 명확하게 구분하기]



* 관계를 찾기/측정/예측

회귀의 (비교적) 엄밀한 정의 (Formal Definition) 

  "In statistical modeling, **regression analysis** is a set of statistical processes  `for estimating the relationships` between a **dependent variable(y)** and one or  more **independent variables(X)**."



* **회귀 = real value 찾고싶다!!!** (예측)



![image-20220516103945830](/Users/krc/Library/Application Support/typora-user-images/image-20220516103945830.png)

벡터를 통해서 특정값을 찾고싶은것임 : how -> 관계를 통해서 찾고싶은것



> Ex.

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516103412293.png" alt="image-20220516103412293" style="zoom:50%;" />



> 변수 x, y를 예측



회귀의 직관적인 의미 
 \- 주어진 데이터(**X**)와 원하는 값(**y**) 사이의 관계를 찾는 방법 
 \- 주어진 데이터(**X**)를 통해서 원하는 값(**y = target value**)을 예측하는 방법 

X = feature vector 

y= target value

  e.g. 부동산 매물 관련된 여러 가지 데이터(**X**)가 주어졌을 때, 집값(**y**)을 예측하는 작업



### **집값 예측**(House Price Prediction)

**부동산 정보를 토대로 집값 예측하기**



![image-20220516104509386](/Users/krc/Library/Application Support/typora-user-images/image-20220516104509386.png)



## 1) Linear Regression

**가장 직관적이고 많이 사용되는 선형 회귀 모델**

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516110911037.png" alt="image-20220516110911037" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516131813144.png" alt="image-20220516131813144" style="zoom:50%;" />

> Xi : (i번째 벡터)

> **x들에 대해서 linear** : x들이 합으로 표시되는데 그 앞에 적당한 숫자 (w) 가지고 고차항이 없는 경우 (상수거나 거듭제곱이 아닌 x 를 말한다)



![image-20220516111445789](/Users/krc/Library/Application Support/typora-user-images/image-20220516111445789.png)

>  x들로 y를 표현

> y= ax+b (1차함수) (선 위를 지나가는 두 점을 주어지거나/ 기울기 y절편 주어지거나)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516111740659.png" alt="image-20220516111740659" style="zoom:50%;" />

> z= ax+by+c 면 하나를 주거나 / 면을 지나가는 점3개를 주어지거나

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516111845282.png" alt="image-20220516111845282" style="zoom:50%;" />



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516111942983.png" alt="image-20220516111942983" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516112130258.png" alt="image-20220516112130258" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516112503582.png" alt="image-20220516112503582" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516112607491.png" alt="image-20220516112607491" style="zoom:50%;" />

![image-20220516113506530](/Users/krc/Library/Application Support/typora-user-images/image-20220516113506530.png)

> (테이블의 Row 하나하나) 데이터 하나하나를 점으로 찍은것
>
> 이 데이터들의 관계를 구할수있다 -> 둘사이에 선을 긋는다 [f]
>
> 선위에 지나가지않는 점은 설명할수없다  (a,b,c)를 가지고 예측할수없음
>
> 다 지나가는 (정확한 선)을 그리지 못한다면 
>
> 덜 틀린 선을 그리자 y hat
>
> 어떻게하면 덜 틀릴까
>
> 직선 g를 긋는다
>
> y랑 g중에 뭐가 덜 틀렸나?
>
> 틀렸다의 정의: 제곱합/데이터 갯수
>
> (yi -yi hat) 다 더한다 , 틀린것이 상쇄되면 안되므로 제곱을 한다, 양수이므로 갯수가 늘면  차이가 커지므로 점(데이터) 개수로 나눈다 [n개의 데이터 평균값] [평균값을 기준으로 업데이트한다]



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516113829803.png" alt="image-20220516113829803" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516114605049.png" alt="image-20220516114605049" style="zoom:50%;" />

> (x,y)데이터가 있다



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516113926317.png" alt="image-20220516113926317" style="zoom:50%;" />

> w1에대한 loss
>
> 우리가 필요한것은 최소값이므로
>
> Optimiztion (gradient 사용)

> 데이터 하나에 대해서 최소를 찾는것 

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516131451148.png" alt="image-20220516131451148" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516131535754.png" alt="image-20220516131535754" style="zoom:50%;" />

> +b 를 해야 원점에서 돌지않는다



#### 어떻게 직선을 찾을것인가?

![image-20220516132042644](/Users/krc/Library/Application Support/typora-user-images/image-20220516132042644.png)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516132239436.png" alt="image-20220516132239436" style="zoom:50%;" />

> Training data 에 대한 최적의 모델 
>
> 트레이닝 데이터의 퍼포먼스 measure 높아야하고
>
> 테스트 데이터의 퍼포먼스 measure 높아야한다
>
> 

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516132407371.png" alt="image-20220516132407371" style="zoom:50%;" />



Training data 에 대한 최적의 모델 -> 학습에 규제를 추가 (오버피팅 예방) 해서 학습시킨다

=> Lasso , Ridge 모델



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516133310813.png" alt="image-20220516133310813" style="zoom:50%;" />



> Feature engineering 을 사용하여 더 좋은 표현을 만들어주고 



## 2) LightGBM Regressor (중요!!)

![image-20220516133528841](/Users/krc/Library/Application Support/typora-user-images/image-20220516133528841.png)

> 가성비 최강모델 실제 현업 회귀 테스크에서 많이 쓴다



![image-20220516133730064](/Users/krc/Library/Application Support/typora-user-images/image-20220516133730064.png)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516133739344.png" alt="image-20220516133739344" style="zoom:33%;" />

> 발전과정 



* AutoML

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516134000582.png" alt="image-20220516134000582"  />

![image-20220516134234328](/Users/krc/Library/Application Support/typora-user-images/image-20220516134234328.png)

> autoML 라이브러리 : pycaret



## 회귀모델 평가

머신러닝의 평가 기준은 다양함

> 주어진 데이터로 모델을 학습시키는 것은 `지정한 성능 평가 지표를 향상시키는 과정`입니다.
>
> 성능 평가 지표의 값은 **“예측 성능”을 기준**으로 합니다. 
>
>  정량적 기준을 설정하고, 달성할 때까지 모델을 학습시키고 성능을 개선합니다. 
>
>  목표한 성능에 도달한 모델을 실제 서비스에 적용합니다.



## 성능 평가

대표적인 회귀 모델 **평가 지표**

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516135156023.png" alt="image-20220516135156023" style="zoom:33%;" />

1. **MSE(Mean Squared Error)** 

   <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516135125940.png" alt="image-20220516135125940" style="zoom:33%;" />

2. RMSLE(Root Mean Squared Log Error) 

   > 가끔씀 
   >
   > <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516141120944.png" alt="image-20220516141120944" style="zoom:25%;" />
   >
   > * 로그 yi 를 대체가능
   >
   > <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516141320406.png" alt="image-20220516141320406" style="zoom:25%;" />
   >
   > 
   >
   > * 2가지 장점
   >
   >   <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516141851796.png" alt="image-20220516141851796" style="zoom:33%;" />
   >
   >   

3. MAE(Mean Absolute Error) 

   <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516135142081.png" alt="image-20220516135142081" style="zoom:33%;" />

4. R2 Score(Coefficient of Determination)

>
>
><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516142104834.png" alt="image-20220516142104834" style="zoom: 50%;" />
>
>키 몸무게 
>
><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516142220189.png" alt="image-20220516142220189" style="zoom:33%;" />
>
><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516142359570.png" alt="image-20220516142359570" style="zoom: 33%;" />
>
>* 분산
>
><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516142600995.png" alt="image-20220516142600995" style="zoom:33%;" />
>
>
>
><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516142630382.png" alt="image-20220516142630382" style="zoom:33%;" />
>
>![image-20220516143025330](/Users/krc/Library/Application Support/typora-user-images/image-20220516143025330.png)
>
>* Worst 0 / best 1
>* SSE는  SST보다 항상 작거나같다 -> 선이기 때문에



## 실습



https://scikit-learn.org/stable/ 싸이킷런

https://www.fun-mooc.fr/en/courses/machine-learning-python-scikit-learn/ 무료강의



![image-20220516155201303](/Users/krc/Library/Application Support/typora-user-images/image-20220516155201303.png)

![image-20220516155353134](/Users/krc/Library/Application Support/typora-user-images/image-20220516155353134.png)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220516155829001.png" alt="image-20220516155829001" style="zoom:50%;" />