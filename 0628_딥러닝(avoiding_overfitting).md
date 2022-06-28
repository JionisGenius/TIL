0628



# Avoiding overfitting



## 1. Drop out

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628195401493.png" alt="image-20220628195401493" style="zoom:50%;" />

> **퍼셉트론**이 고양이의 각 중요한 **특징** ==> 눈 코 입 꼬리 등등

> 퍼셉트론을 랜덤하게 일부 꺼트려 놓음
>
> 일부특징 귀 꼬리 를 안보고 맞추며 학습한다.



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628195541655.png" alt="image-20220628195541655" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628195553208.png" alt="image-20220628195553208" style="zoom:50%;" />





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628195931490.png" alt="image-20220628195931490" style="zoom:50%;" />

> 서로다른 신경망 조합: 2^4 개
>
> **Model ensemble** 효과



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628200023373.png" alt="image-20220628200023373" style="zoom: 67%;" />

> 마지막에 대부분(p==0.5) 적용한다





## 2) Batch Normalization

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628200318585.png" alt="image-20220628200318585" style="zoom:50%;" />

> - MinMax normalization 를 적용하면 각각의 열들이 1과 0사이로 맞춰짐
>
> - Standardization 평균: 0 표준편차: 1 맞춰줌
>   - feature(column) 마다 scale 차이가 심할때 



- Column = Attribute = Dimension= Feature(이미지 특징/ x 데이터의 열)



1. **Min-Max Algorithm** : min 열=0 , max 열= 1

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628202000977.png" alt="image-20220628202000977" style="zoom:50%;" />

2. **Standardization** : mean열=0 , std(열)= 1

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628202056979.png" alt="image-20220628202056979" style="zoom:50%;" />



> Train data 기준으로 계산하고
>
> Test, Train data 모두에게 적용한다. 



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628203143785.png" alt="image-20220628203143785" style="zoom:50%;" />



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628203713060.png" alt="image-20220628203713060" style="zoom: 67%;" />

> 100행 3열 짜리 데이터 들어옴
>
> 행렬곱 [**100** * 3]  [3 * **4**] => **[100 * 4]** 
>
>  껍데기 함수 씌우기 **g(x)** 차원 바뀌지않음 [100 * 4]



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628204141456.png" alt="image-20220628204141456" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628205654889.png" alt="image-20220628205654889" style="zoom: 67%;" />



### NNOptimization 

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220628205616845.png" alt="image-20220628205616845" style="zoom: 67%;" />