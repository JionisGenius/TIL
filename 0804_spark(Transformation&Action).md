0804

# Cache & Persist



지연연산(lazy transformation)이 유리한이유

: task 를 disk 저장하고 다시 꺼내서 연산하는 것을 반복하며 생기는 속도저하

<img width="500" alt="image"  src="https://user-images.githubusercontent.com/98302106/182750977-e0d179f6-de80-40aa-9907-cd5bae291cf1.png" >

in-memory: task에서 task 로 주고받을때 in-memory 방식이 빠르다

<img width="300" alt="image" src="https://user-images.githubusercontent.com/98302106/182751165-eeca3648-c2d9-4bda-b63a-21364347ae48.png"  >

In-memory는 어떤 데이터를 메모리에 남겨야 할지 알아야 가능하다

Transformation은 지연 실행되기 때문에 메모리에 저장해둘 수 있다.



데이터를 메모리에 남겨두고 싶을때 사용할수있는 함수

- **Cache()**

- **Persist()**

**데이터를 메모리에 저장**해두고 사용이 가능

Ex. Regression 반복연산할때 성능좋아짐



### Cache vs Persist

Cache 함수

- default Storage Level 를 사용한다
- RDD: memory_only
- DF: memory_and_disk

Persist 함수

- Storage level 을 사용자가 원하는대로 지정가능



# Cluster Topology

Spark 클러스터의 내부구조

**Marster Wokrer** 와 **Topology**로 구성 되어있다.

스파크를 쓰면서 잊지말아야할점

- 항상 데이터가 여러곳에 분산되어있다는것 
- 같은 연산이어도 여러 노드에 걸쳐서 실행된다는 점



<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/182762063-05fcbad8-ed69-4098-b1d4-9ec4085498af.png">



> spark의 마스터 로써 Driver Program
>
> 워커로써 Worker Node가 있다.
>
> Spark context는 새로운 RDD를 생성한다.
>
> 개발자가 작업할때는  드라이버프로그램 중심으로 만들고 드라이버프로그램들이 워커노드들에게 연산할 작업을 보낸다
>
> 드라이버 프로그램은 작업을 직업수행하지않지만 작업들을 조직한다
>
> 클러스터매니저로 작업끼리 소통한다 , excutor 수집, 내부프로그램에 다시 전송
>
> worker node에는 연산을 실행하고 저장하는 Excutor , 저장공간을 제공하는 cache가 있다
>
> ~~~python
> RDD.foreach(lambda x: print(x))
> ~~~
>
> >  위의 코드가 출력이 보이지 않았던이유는
> >
> > foreach가 액션이였기때문에 excutor 바로 실행되었기 때문이다.
> >
> > 그러므로 print동작은 워커노드에서 일어나게 되고 드라이버 프로그램에서는 출력결과를 확인할 수 없다
>
> ~~~~python
> foods = sc.parallelize(["짜장면","마라탕",...])
> three=foods.take(3)
> ~~~~
>
> > three 는 드라이버 프로그램에서 존재한다
> >
> > 출력 확인할 수 있다.



# Reduction Operations

### **Rediction**

- 요소들을 모아서 하나로 합치는 작업
- 많은 spark 연산들이 reduction이라 할수있다.



그중에서도 **병렬/분산된 환경**에서의 reduction 작업



대부분의 action은 reduction

Reduction: 근접하는 요소들을 모아서 하나의 결과로 만드는 일

- 파일저장,collect()와같은 액션이 아닌 리덕션도 존재



**병렬처리가 가능한 예**

A+B 처럼 두개의 요소를 모아서 하나의 결과로 합친다

각 파티션과 task는 독립적이다

<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/182766546-31c3a3f2-8ccb-4a32-95c0-665850467d29.png">





**병렬처리 불가능한 예 (병렬처리의 의미가 없는경우)**

(병렬처리를 할때에는)파티션 마다 독립적으로 작업할수있어야하는데, 그렇지 않고 작업의 선후관계가 필요할때

순서대로 해야하는 작업이면 분산처리하는 의미가없다

<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/182766785-5b0fd806-e82c-48a5-adc0-354c486f957a.png">



## 대표적인 Reduction Actions

- Reduce
- Fold
- Group by
- Aggregate



### Reduce

- RDD.reduce(<function>)

~~~python
from operator import add # python의 operator 에서 add함수를 가져옴
sc.parallelize([1,2,3,4,5]).reduce(add) # 12345가 들어있는 RDD를 만든다 #add 요소더해줌
~~~



파티션이 등장하면서 결과값이 달라질수있다.

분산된 파티션들의 연산과 합치는 부분을 나눠서 생각해야한다

~~~python
>>> sc.parallelize([1,2,3,4]).reduce(lambda x,y: (x*2)+y) # 파티션 지정 안함
26
>>> sc.parallelize([1,2,3,4],1).reduce(lambda x,y: (x*2)+y)
26
>>> sc.parallelize([1,2,3,4],2).reduce(lambda x,y: (x*2)+y)
18
>>> sc.parallelize([1,2,3,4],3).reduce(lambda x,y: (x*2)+y)
18
>>> sc.parallelize([1,2,3,4],4).reduce(lambda x,y: (x*2)+y)
26
~~~



<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/182768637-d8f4fe17-436e-4fb7-9b90-e99cd413a951.png">



파티션이 어떻게 나뉠지 프로그래머가 정확하게 알기 어려우므로

- 연산의 순서와 상관없이 결과값을 보장하려면

  - $$
    교환법칙: a*b=b*a
    $$

  - $$
    결합법칙: (a*b)*c= a*(b*c)
    $$



### Fold

reduce와 비슷하지만 zero velue가 들어간다 **(시작값)**

- RDD.fold(zeroValue,<function>)

~~~python
from operator import add
sc.paralleize([1,2,3,4,5]).fold(0,add)
# 15
~~~

 

Fold& partition

~~~python
rdd=sc.parallelize([2,3,4],4)
rdd.reduce(lambda x,y: x*y)
# 24
rdd.fold(1, lambda x,y: x*y)
# 24
~~~

$$
(2*3*4)=24 \\ (1*2*3*4)=24
$$



~~~python
rdd=sc.parallelize([2,3,4],4)
rdd.reduce(lambda x,y:x+y) # 요소는 3개인데 파티션은 4개 이므로 빈공간 더해줌(0으로 표시함)
# 9
rdd.fold(1,lambda x,y: x+y) # zero value:1 # 각 파티션의 시작값이 각1 더함 # 빈공간의 시작값도 1이므로 (1+1)
# 14
~~~

$$
0+2+3+4=9
\\
(1+1)+(1+2)+(1+3)+(1+4)=14
$$

### Group by

- RDD.groupBy(<기준함수>)

~~~python
rdd.sc.parallelize([1,1,2,3,5,8])
result=rdd.groupBy(lambda x: x % 2).collect()
sorted([ ( x,sorted(y) ) for (x,y) in result])
# [ ( 0,[2,8] ),( 1,[1,1,3,5] ) ] #나머지가 0인 짝수들과 나머지가 1인 홀수들 분류
~~~



### Aggregate

- RDD데이터 타입과 Action **결과 타입이 다를 경우** 사용
- <u>파티션 단위</u>의 연산 결과를 합치는 과정을 거친다
- RDD.aggregate(zeroValue,seqOp,combOp)
  - zero value : 시작값
  - seqOp: 타입 변경 함수 sequential #map() 이랑 비슷한 개념
  - combOp: 합치는 함수 combine # reduce 하는 개념

~~~python
seqOp= (lambda x, y: (x[0]+y, x[1]+1))
combOp=(lambda x,y:(x[0]+y[0],x[1]+y[1]))
sc.parallelize([1,2,3,4]).aggregate((0,0),seqOp, combOp)
# (10,4)
sc.parallelize([]).aggrgate((0,0),seqOp,combOp)
# (0,0)
~~~



<img width="800" alt="image" src="https://user-images.githubusercontent.com/98302106/182787839-f3635767-5db7-4739-8cfc-657e08f193c7.png">

> 제로벨류(0,0) 각각의 파티션에 들어간다 (왼쪽 \\ 오른쪽)





# Key-Value RDD Transformations & Actions

### Transformations

- groupby
- reduceBykey
- mapValues
- keys
- Join(+leftOuterJoin, rightOuterJoin)

### Actions

- countBykey



###  groupBy

주어지는 **함수** 기준으로 group

- RDD.groupBy(<u>**f,**</u>numpartitions=None,partitionFunc=<function portable_hash>)

~~~python
rdd=sc.parallelize([1,1,2,3,5,8]) # prallelize rdd생성
result=rdd.groupBy(lambda x:x % 2).collect() 
sorted([(x,sorted(y)) for (x,y) in result ])
# [(0, [2,8]), (1,[1,1,3,5])]
~~~



### groupByKey

주어지는 **key**를 기준으로 group

- RDD.groupByKey(numpartitions=None,partitionFunc=<function portable_hash>)

~~~python
rdd=sc.parallelize([("a",1),("b",1),("a",1)]) # key	별로 rdd 생성 #transformation이라서 바로 실행x
sorted(rdd.groupByKey().mapValues(len).collect()) # mapValue함수로 불러서 실행
# [('a',2),('b',1)] # a는 2개의 값을, b는 1개의 값을 가지고있다
sorted(rdd.groupByKey().mapValues(list).collect())
# [('a',[1,1]), ('b',[1])] # a는 2개의 1,  b는 1개의 1 가지고있다
# groupByKey() 에 파라미터 입력하면 파티션
~~~



### Reduce

주어지는 함수를 기준으로 요소들을 합침(action)

- RDD.reduce(f)

~~~python
sc.parallelizee([1,2,3,4,5]).reduce(add)
#15
~~~



### ReduceByKey

key를 기준으로 그룹을 만들고 합침(trans)

개념적으로는 groupByKey+reduction

하지만 groupbykey보다 훨씬 빠르다

- RDD.reduceByKey(func, numpartitions=None, partitionFunc=<function portable_hash>)

~~~python
rdd=sc.parallelize([("a",1),("b",1),("a",1)])
sorted(rdd.reduceByKey(add).collect())
# [('a',2),('b',1)]
~~~



### Map Values

함수를 **벨류에게만 적용**한다

파티션과 키는 그대로 (장점: 파티션과 키가 왔다갔다 이동하려면 네트워크 코스트가 증가=> 성능저하로 이어지기때문에)

~~~python
x=sc.parallelize([("a",["apple","banana","lemon"]),("b",["grapes"])])
def f(x): return len(x)
x.mapValues(f).collect()
# [('a',3),('b',1)]
~~~



### countByKey

각 키가 가진 요소들을 센다

~~~~python
rdd=sc.parallelize([("a",1),("b",1),("a",1)])
sorted(rdd.countByKey().items())
# [('a',2),('b',1)]
~~~~



### keys()

하나의 transformation임

모든 key를 가진 RDD를 생성한다

values는 action 이다

keys는 파티션을 유지하거나 key의 종류가 많을것이기때문에 대기하기위해 transformation임

~~~python
m= sc.parallelize([(1,2),(3,4)]).keys()
m.collect()
# [1,3]
~~~

예제

유니크한 키값 출력

<img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/182825063-8c1851b7-44eb-4ac9-8745-01f2a8979059.png">



### Join

- transformation

- 여러개의 RDD를 합치는데 사용

- 대표적으로 두가지의 Join 방식이 존재

  - inner join
  - Outer join(left, right)

  기준데이터를 모두출력 반대편데이터에서 데이터가 없는 경우 None

  <img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/182826179-93117547-b235-47a5-8536-6ff1f61dbf88.png">



# Shuffling & Partitioning

### Shuffling

- 그룹핑시 데이터를 한 노드에서 다른노드로 옮길때
- 성능을 많이 저하시킨다

#### Shuffle을 일으킬수 있는 작업들

<img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/182827498-3b27d559-67ee-45a1-9f57-41a2f068844e.png">
<img width="300" alt="image" src="https://user-images.githubusercontent.com/98302106/182827553-274f0472-814f-4544-af8c-fee342a58f36.png">



#### Shuffle 이 발생할때

- 결과로 나오는 RDD가 원본 RDD의 다른 요소를 참조하거나
- 다른 RDD를 참조할때



Partitioning이용한 성능 최적화

- reducebykey

<img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/182828201-56766f86-a25a-4b64-8cff-deb23ba0bded.png">

- Groupbykey + reduce

<img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/182828393-4a2f9c62-91ad-4d0d-84a0-709d3412ac51.png">



#### shuffle 최소화

- 미리 파티션을 만들어 두고 캐싱 후 reduceByKey실행
- 미리 파티션을 만들어두고 캐싱 후 join실행
- 둘다 파티션과 캐싱을 조합해서 최대한 로컬환경(각 파티션 내부)에서 연산이 실행되도록 하는 방식

>  셔플최소화해서 10배이상 성능향상이 가능하다!!

<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/182829918-637c4767-fd94-4ae2-929f-58f3cb64ce8d.png">



### Partition

#### partiotion의 목적

데이터를 최대한 균일하게 퍼트리고 쿼리가 같이 되는 데이터를 최대한 옆에 두어 검색 성능을 향상시키는 것

셔플링을 줄여 최소화하고 성능을 향상시키기위함

#### partition의 특징

- RDD는 쪼개져서 여러 파티션에 저장됨
- 하나의 파티션은 하나의 노드 (서버)에
- 하나의 노드는 여러개의 파티션을 가질수 있다
- 파티션의 크기와 배치는 자유롭게 설정
- Key-value RDD사용할때만 의미가 있다

> 스파크 파티셔닝 == 일반프로그래밍에서 자료구조를 선택하는것

#### partition 의 종류

- Hash partition
- Range partition

### Hash partition

데이터를 여러 파티션에 균일하게 분배하는 방식

<img width="400" alt="image" src="https://user-images.githubusercontent.com/98302106/182831230-45a24870-4ad9-4f38-a812-23acfb778db0.png">

> 잘못사용하면 한쪽에만 데이터가 쌓이는 경우가 생김 -> **partition skew** 발생



### Range partition

순서가 있는 정렬된 파티셔닝

- **키** 순서
- **키 집합**의 순서

서비스의 쿼리 패턴이 날짜 위주면 일별 Range Partition고려



#### Memory % Disk Partition 

디스크에서 파티션하기

- PartitionBy() : 사용자가 지정한 파티션을 가지는 RDD를 생성하는 함수

<img width="700" alt="image" src="https://user-images.githubusercontent.com/98302106/182835546-2ac06052-84c2-42d2-9814-f0194d8ea7f1.png">

<img width="700" alt="image" src="https://user-images.githubusercontent.com/98302106/182837060-9547a190-152d-4c11-98f5-b4576c083004.png">

> glom으로 파티션이 어떻게 형성되는지 확인할수있다 (transformation)

메모리에서 파티션하기

- Repartition() : 파티션의 크기를 줄이거나 늘리는데 사용됨
- Coalesce() : 파티션의 크기룰 줄이는데 사용됨 (줄일때는 coalesce가 성능 더 좋다)

: 둘다 파티션의 갯수를 조절하는데 사용한다

shuffling을 동반한 비싼작업



#### Map vs MapValue

Map vs MapValue(연산중 파티션을 만들 수 있다)

flatMap vs flatMapValues(연산중 파티션을 만들 수 있다)

map과 flatMap은 key의 변형이 가능하기때문에 연산중 파티션을 만들수있는 함수가 될수없다( 연산중에 키가 바뀌면 파티션 자체가 망가지기 때문)