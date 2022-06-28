# PyThorch

- 장점:
  -  Numpy 와 비슷한 문법
  - 동적으로 back-propagation 경로 생성 가능
  - 훌륭한 documentaion
  - 난이도에 비해 자유도 높음

- 단점:
  -  TensorFlow(compiler방식)에 비해 낮은 상용화



 TensorFlow(compiler방식) / PyTorch (interpreter)



## 데이터셋

- object: 주어진 데이터에 대해서 결과를 내는 가상의함수를 **모사**하는 함수를 만드는것

- In order to do this : 

  - 가상의 함수 ground-truth를 통해 데이터 N 개의 쌍 (x,y)을 수집해 데이터셋을 만들고, 우리의 함수(신경망)가 데이터를 통해 가상의 함수를 모방하도록한다
  - <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623163213720.png" alt="image-20220623163213720" style="zoom:50%;" />

  > 튜플로 N개의 (x,y)한 쌍 

- 이때, **𝑥와𝑦는** 각각 𝑛차원과 m차원의 **벡터로 표현**될수있다.
  - <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623163259585.png" alt="image-20220623163259585" style="zoom:50%;" />

> x집합에 속하는 n차원의 Real value 벡터

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623171010849.png" alt="image-20220623171010849" style="zoom:50%;" />

> 784차원의 벡터로 볼수있다



- Column :벡터의 차원
- row가 하나의 샘플



# Tensor?



![image-20220623171624338](/Users/krc/Library/Application Support/typora-user-images/image-20220623171624338.png)



### Tensor shape

[차원그릴때 습관들이기]

첫번째차원 k / 두번째차원 n /세번째 차원 m

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623171723917.png" alt="image-20220623171723917" style="zoom:50%;" />



### Matrix shape



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623171911854.png" alt="image-20220623171911854" style="zoom:50%;" />



### Typical Tensor shape: tabular dataset



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172053572.png" alt="image-20220623172053572" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172104992.png" alt="image-20220623172104992" style="zoom:50%;" />

> 한 샘플을 골라낸다 
>
> x는 n 차원의 벡터

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172227623.png" alt="image-20220623172227623" style="zoom:50%;" />

> 합치면 N*n차원의 벡터



### Mini-batch: coinsider Parallel operations

배치로 연산한다

k개의 샘플 한번에 처리할수있다



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172556471.png" alt="image-20220623172556471" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172605129.png" alt="image-20220623172605129" style="zoom:50%;" />

> n 차원인 함수 t에서 k개 샘플링 했으므로
>
> k개의 샘플 뽑았으니까 n차원의 벡터가 k개 있는것



### Typical Tensor shape : natural language processing

전형적인 텐서

- NLP로 예시를 들었을때
  - y=f(x):  x는 문장

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623172949061.png" alt="image-20220623172949061" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623173132257.png" alt="image-20220623173132257" style="zoom:50%;" />

> 2번째 sentences에서 2번째 words 의 특징들



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623173304790.png" alt="image-20220623173304790" style="zoom:50%;" />

> i번째 문장에 j번째 단어

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623173714468.png" alt="image-20220623173714468" style="zoom:50%;" />

> i번째 문장



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220623173819403.png" alt="image-20220623173819403" style="zoom:50%;" />

> Number of words
>
> Number of features



(Row,column,value,dimension)



### Typical Tensor shape : computer vision (grayscale)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624135139890.png" alt="image-20220624135139890" style="zoom:50%;" />

> White

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624135216924.png" alt="image-20220624135216924" style="zoom:50%;" />







>  이미지가 여러장 쌓여있다
>
> **|x|= (num of imgs, h, w)** ==> gray scale
>
> 그레이스케일 x 를 => y = f(x) 형태의 NN에 대입 



### Typical Tensor shape : computer vision (color)



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624135550909.png" alt="image-20220624135550909" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624140524375.png" alt="image-20220624140524375" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624140603249.png" alt="image-20220624140603249" style="zoom:50%;" />

> RGB
>
> **|x|=(num of img , num of channels , hight , weight )** 4차원
>
> i번째 이미지 (한장의 이미지를 볼 경우)=> **|xi|= (num of channels, hight, weight)**





####  Tensor + Vector

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624165203123.png" alt="image-20220624165203123" style="zoom:50%;" />

~~~python
x = torch.FloatTensor([[1, 2]])
y = torch.FloatTensor([[3],
                       [5]])

print(x.size())
print(y.size())

***
#torch.Size([2, 2])
#torch.Size([2])
~~~

~~~python
z = x + y
print(z)
print(z.size())

***
#tensor([[ 4.,  7.],
#        [ 7., 13.]])
#torch.Size([2, 2])
~~~





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220624165529770.png" alt="image-20220624165529770" style="zoom:50%;" />

~~~python
x = torch.FloatTensor([[[1, 2]]])
y = torch.FloatTensor([3,
                       5])

print(x.size())
print(y.size())

***
#torch.Size([1, 1, 2])
#torch.Size([2])
~~~

~~~python
z = x + y
print(z)
print(z.size())

***
#tensor([[[4., 7.]]])
#torch.Size([1, 1, 2])
~~~



![image-20220624183349106](/Users/krc/Documents/TIL/image-20220624183349106.png)