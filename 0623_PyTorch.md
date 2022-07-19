



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

print(x.size())z
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



### Tensor +Tensor



~~~python
x = torch.FloatTensor([[1, 2]])
y = torch.FloatTensor([[3],
                       [5]])

print(x.size())
print(y.size())

***
#torch.Size([1, 2])
#torch.Size([2, 1])
~~~

~~~python
z = x + y
print(z)
print(z.size())

***
#tensor([[4., 5.],
#        [6., 7.]])
#torch.Size([2, 2])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629144325809.png" alt="image-20220629144325809" style="zoom:50%;" />

> 복사해서 더한다.

> **Note that you need to be careful before using broadcast feature.**



### Failure Case

브로드캐스트가 적용이 안되는 사례

Extand사용해라

~~~python
x = torch.FloatTensor([[[1, 2],
                        [4, 8]]])
y = torch.FloatTensor([[1, 2, 3],
                       [4, 5, 6],
                       [7, 8, 9]])

print(x.size())
print(y.size())

z = x + y
***
#torch.Size([1, 2, 2])
#torch.Size([3, 3])
~~~





>  04-06-PyTorch Tensor Manipulations

## PyTorch Tensor Manipulations

~~~ python
import torch

~~~



### Tensor Shaping

#### reshape Function: Change Tensor Shape

Reshape( 내가 원하는 tensor 의 size )



~~~
print(x.reshape(12)) # 12 = 3 * 2 * 2 # 차원이 하나인 벡터
print(x.reshape(-1)) # 결과같은데 편함
~~~

<img src="/Users/krc/Documents/TIL/image-20220629145418870.png" alt="image-20220629145418870" style="zoom:50%;" />





~~~python
# size 같으므로 차원 상관없음

print(x.reshape(3, 4)) # 3 * 4 = 3 * 2 * 2
print(x.reshape(3, -1)) # 동일한 결과 # 만약 12로 나눠지지 않는 숫자면 erorr # -1 은 한번만 쓴다
~~~



<img src="/Users/krc/Documents/TIL/image-20220629145503551.png" alt="image-20220629145503551" style="zoom:50%;" />

~~~python
print(x.reshape(3, 1, 4)) 
print(x.reshape(-1, 1, 4)) # -1 부분 "알아서 채워~"
~~~



<img src="/Users/krc/Documents/TIL/image-20220629145936645.png" alt="image-20220629145936645" style="zoom:50%;" />

~~~python
print(x.reshape(3, 2, 2, 1)) #4차원
~~~

<img src="/Users/krc/Documents/TIL/image-20220629150117772.png" alt="image-20220629150117772" style="zoom: 50%;" />



> Contaiguous(같은 메모리로 re alocation) + view =reshape
>
> View 코드 를 보면 reshape과 같은것이다





#### squeeze: Remove dimension which has only one element.



특정 원소가 1개인 차원 삭제

~~~python
x = torch.FloatTensor([[[1, 2],
                        [3, 4]]])
print(x.size())

***
#torch.Size([1, 2, 2]) 뒤의 2, 2  차원만 살아남을 예정
~~~

>  Remove any dimension which has only one element. 

~~~python
print(x.squeeze()) #짰다... 1만있는 차원 삭제됨
print(x.squeeze().size()) # 차원확인
***
#tensor([[1., 2.],
#        [3., 4.]])
#torch.Size([2, 2]) 		# 2,2 살아있다
~~~

> Remove certain dimension, if it has only one element.
>
> If it is not, there would be no change.



특정 dimension 에 대해서만 짤수있다!

~~~~python
print(x.squeeze(0).size())
print(x.squeeze(1).size())

***
#torch.Size([2, 2])
#torch.Size([1, 2, 2])
~~~~





#### unsqueeze: Insert dimension at certain index.

차원인덱스 생성

~~~python
x = torch.FloatTensor([[1, 2],
                       [3, 4]])
print(x.size())
***
#torch.Size([2, 2])
~~~

~~~python
print(x.unsqueeze(2))
print(x.unsqueeze(-1))
print(x.reshape(2, 2, -1))
***
tensor([[[1.],
         [2.]],

        [[3.],
         [4.]]])
tensor([[[1.],
         [2.]],

        [[3.],
         [4.]]])
tensor([[[1.],
         [2.]],

        [[3.],
         [4.]]])
~~~





> 04-07-tensor_slicing_and_concat

## Slicing and Concatenation



### Indexing and Slicing

타겟준비

~~~python
x = torch.FloatTensor([[[1, 2],
                        [3, 4]],
                       [[5, 6],
                        [7, 8]],
                       [[9, 10],
                        [11, 12]]])
print(x.size())
***
#torch.Size([3, 2, 2])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629155636454.png" alt="image-20220629155636454" style="zoom:50%;" />



~~~python
# 빨강
print(x[0])
print(x[0, :])
print(x[0, :, :])

***
#tensor([[1., 2.],
#        [3., 4.]])
#tensor([[1., 2.],
#        [3., 4.]])
#tensor([[1., 2.],
#        [3., 4.]])
~~~

~~~python
#파랑
print(x[-1])
print(x[-1, :])
print(x[-1, :, :])
***
#tensor([[ 9., 10.],
#        [11., 12.]])
#tensor([[ 9., 10.],
#        [11., 12.]])
#tensor([[ 9., 10.],
#        [11., 12.]])
~~~

~~~python
#초록
print(x[:, 0, :])
***
#tensor([[ 1.,  2.],
#        [ 5.,  6.],
#        [ 9., 10.]])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629155923273.png" alt="image-20220629155923273" style="zoom:50%;" />



**range**로도 자를수 있다

- Range 특: 해당 dimension 사라진다

>  Access by range. Note that the number of dimensions would not be changed. 차원바뀌지 않는다

~~~python
print(x[1:3, :, :].size())
print(x[:, :1, :].size())
print(x[:, :-1, :].size())
***
#torch.Size([2, 2, 2])
#torch.Size([3, 1, 2])
#torch.Size([3, 1, 2])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629165055818.png" alt="image-20220629165055818" style="zoom:50%;" />



### split: Split tensor to desirable shapes

특정 **사이즈**로 쪼개기



~~~python
x = torch.FloatTensor(10, 4)

splits = x.split(4, dim=0) # 0번 dimension이 4가 되도록 쪼개라 (남은건 알아서~)
for s in splits:
    print(s.size())

***
#torch.Size([4, 4])
#torch.Size([4, 4])
#torch.Size([2, 4])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629173533507.png" alt="image-20220629173533507" style="zoom:50%;" />



### chunk: Split tensor to number of chunks

**개수**로 쪼갠다



~~~python
x = torch.FloatTensor(8, 4)

chunks = x.chunk(3, dim=0) 

for c in chunks:
    print(c.size())
    
***
#torch.Size([3, 4])
#torch.Size([3, 4])
#torch.Size([2, 4])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629174118146.png" alt="image-20220629174118146" style="zoom:50%;" />

### index_select: Select elements by using dimension index



~~~python
x = torch.FloatTensor([[[1, 1],
                        [2, 2]],
                       [[3, 3],
                        [4, 4]],
                       [[5, 5],
                        [6, 6]]])
indice = torch.LongTensor([2, 1])

print(x.size())
***
#torch.Size([3, 2, 2])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629174613700.png" alt="image-20220629174613700" style="zoom:50%;" />

~~~python
y = x.index_select(dim=0, index=indice)

print(y)
print(y.size())
***
#tensor([[[5., 5.],
#         [6., 6.]],
#
#        [[3., 3.],
#         [4., 4.]]])
#torch.Size([2, 2, 2])
~~~

<img src="/Users/krc/Documents/TIL/image-20220629174740829.png" alt="image-20220629174740829" style="zoom:50%;" />

### cat: Concatenation of multiple tensors in the list



~~~~python
x = torch.FloatTensor([[1, 2, 3],
                       [4, 5, 6],
                       [7, 8, 9]])
y = torch.FloatTensor([[10, 11, 12],
                       [13, 14, 15],
                       [16, 17, 18]])

print(x.size(), y.size())
***
#torch.Size([3, 3]) torch.Size([3, 3])
~~~~







![image-20220624183349106](/Users/krc/Documents/TIL/image-20220624183349106.png)





