0725

# 병렬처리와 분산처리

#### Parallel & Distributed

data parallel : 

1. 데이터를 여러개로 쪼개고 
2. 여러 스레드에서 각자 task를 적용 (동시에 연산)
3. 각자 만든 결과값을 합치는 과정

~~~
RDD.map(<task>)
~~~



Distributed data-parallel:

1. 데이터 쪼개서 여러노드로 보낸다
2. 여러 노드에서 독립적으로 task적용
3. 각자의 결과 합치는 과정



분산된 환경에서도 spark를 이용하면 일반적인 병렬처리를 하듯 코드를 짜는게 가능하다

spark는 분산된 환경에서 데이터 병렬 모델을 구현해 추상화 시켜주기 때문 (**RDD**)

~~~python
RDD.map(<task>)
~~~

> RDD쓰면 알아서 해준다
>
> 그러므로 노드 간 통신에 신경써야한다



### 분산처리 문제

- **부분실패** : 노드가 프로그램과 상관없는 이유로 인해 실패
- **속도**: 많은 네트워크 통신을 필요로 하는 작업은 속도가 저하

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725170326331.png" alt="image-20220725170326331" style="zoom:50%;" />

> 다른 성능
>
> 1번째코드가 성능좋음 필터로 먼저 데이터양을 줄였기 때문



### 연산속도 

메모리> 디스크 > 네트워크 (메모리보다 100만배 느림;)



## Structured Data & RDD

**Single value RDD**

ex. 넷플 드라마가 받은 평균 평점(날짜,고객수)



### **Key-value RDD**

key와 value쌍을 갖는 key-value RDD (pairs RDD라고도함)

ex. 텍스트에 등장하는 단어 수 세기(날짜)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725171002682.png" alt="image-20220725171002682" style="zoom:43%;" />



~~~python
pairs =rdd.map(lambda x: (x,1)) # 람다함수 x라는 값을 받고 튜플을 돌려준다 # 첫번째는 x리턴 ,두번째는 1리턴
# 단순 값 뿐만아니라 list도 가능하다
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725172859145.png" alt="image-20220725172859145" style="zoom:40%;" />



#### key

- reduceByKey()-키값을 기준으로 테스크 처리
- groupByKey()- 키값을 기준으로 벨류를 묶는다
- sortByKey()- 키값을 기준으로 정렬
- Keys()-  키값 추출
- Values() - 벨류값 추출

~~~python
pairs =rdd.map(lamda x: (x,1))
count = pairs.reduceByKey(lambda a,b,:a+b) #키값기준으로 벨류값을 합치는 결과
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725173433012.png" alt="image-20220725173433012" style="zoom:50%;" />

> 데이터 / pairs RDD / 결과

#### join

- join
- rightOuterJoin
- leftOuterJoin
- subtractByKey



#### mapping values

key value 데이터에서 key를 바꾸지 않는 경우

map()대신 value만 다루는 mapValues()함수를 써주자

Spark내부에서 파티션을 유지할수있어 더욱 효율적

- mapValues()
- flatMapValues()

> Value만 다루는 연산들이지만 RDD에서 key는 유지된다



