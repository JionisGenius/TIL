0802

# Spark Operation

**Spark operartion** = transformations + Actions



**Trainsformation** : 결과값으로 새로운 RDD를 변환

- Lazy Execution 지연실행 : action 만날때 실행



**Actions**: 결과값을 연산하여 출력하거나 저장

- Eager Excution 즉시 실행



<img src="https://user-images.githubusercontent.com/98302106/182306288-d6efe26b-df0f-4b89-aa0c-f61c9cc95c12.png" style="zoom:33%;" >



- jupyter notebook 실습

~~~python
from pyspark import SparkConf, SparkContext
conf=sparkConf().setMaster("local").setAppName("transfomations_actions") #환경:로컬 , 이름설정

# spark context 생성
sc=SparkContext(conf=conf) 

#설정리스트 뽑아보기
sc.getConf().getAll()

#스파크 콘텍스트 삭제
sc.stop()

# 삭제 확인
type(sc)
sc
# no type .... <- 삭제잘됨

#다시 생성
sc=sparkContext("conf=conf")

#list 생성
foods= sc.parallelize(["짜장면", "마라탕", "짬뽕", "떡볶이", "쌀국수", "짬뽕", "짜장면", "짜장면", "짜장면",  "라면", "우동", "라면"])

# RDD확인
foods

# collect() 모든 데이터를 가져오는 액션. 디버깅외에 프로덕션에 사용하기에는 낭비이니 쓰지않는것을 지향하자
foods.collect()
#['짜장면', '마라탕', '짬뽕', '떡볶이', '쌀국수', '짬뽕', '짜장면', '짜장면', '짜장면', '라면', '우동', '라면']


# 밸류 값들을 셀수있다. key:value
foods.countByValue()

~~~

<img src="https://user-images.githubusercontent.com/98302106/182325696-f98bbb0b-b1d0-403b-9ea1-7867d57ac5be.png" style="zoom: 50%;" >



~~~python
#  3개 가져오기
foods.take(3)

# 첫번째 데이터 string 출력
foods.first()

#총 element 개수 count // 중복포함
foods.count()

# 중복제외 distinct 
foods.distinct()
foods.distinct().collect() # 유니크한 값들 출력
foods.distinct().count() #유니크한 값 카운트




~~~



### foreach

**액션**이기 때문에 <u>워커노드</u>에서 실행된다.

(쥬피터 노트북 프로그램) <u>드라이버 프로그램</u>(spark context가 있는)이므로 액션이 실행되는것이 보이지 않는다.

요소를 하나하나씩 꺼내는 하나의 **함수**로 작용 

<u>값을 리턴하지않는다</u>

~~~python
# foreach 요소를 하나하나씩 꺼내는 하나의 함수로 작용// 값을 리턴하지않는다
foods.foreach(lamda x : print(x))


~~~



**Transformations**  = narrow + wide 

- Narrow transformation

  - 1:1 변환
  - Filter(), map(), flatmap(),sample(),union()
  - 1열을 조작하기위해 다른 열/파티션의 데이터를 참고하거나 쓸필요가 없다
  - 정렬 필요하지않은 경우

  

- wide transformation

  - shuffling
  - Intersection and join , distinct, cartesian, reduceByKey(), groupByKey()
  - 아웃풋 RDD의 파티션에 다른 파티션의 데이터가 들어갈수있음 (coupling) 많은 리소스 요구해서 성능위해 최적화 필요하다

  

  

  ### narrow transformations

  

  ~~~python
  sc.parallelize([1,2,3]).map(lambda x: x+2)
  
  sc.parallelize([1,2,3]).map(lambda x: x+2).collect() # collect 액션
  #[3,4,5]
  
  movies = [
      "그린 북",
      "매트릭스",
      "토이 스토리",
      "캐스트 어웨이",
      "포드 V 페라리",
      "보헤미안 랩소디",
      "빽 투 더 퓨처",
      "반지의 제왕",
      "죽은 시인의 사회"
  ]
  
  
  mobiesRDD=sc.parallelize(movies)
  
  moviesRDD.collect()
  
  #flatMap() 요소들을 어떻게 펼칠지 
  flatMovies =moviesRDD.flatMap(lambda x:x.split(" ")) # split 공백 기준으로  
  
  flatMovies.collect() #RDD 임
  
  ~~~

  <img src="https://user-images.githubusercontent.com/98302106/182372041-6f921ef1-630d-4445-b542-5e4f6fde1b06.png" style="zoom:50%;" >

  

~~~~python
# 매트릭스 제외한 필터링
filteredMovies = flatMovies.filter(lambda x:x != "매트릭스")

filteredMovies.collect()
~~~~



~~~python
num1 = sc.parallelize([1, 2, 3, 4])
num2 = sc.parallelize([4, 5, 6, 7, 8, 9, 10])

# intersection 교집합
num1.intersection(num2).collect()
# [4]

# union 합집합 (중복포함)
num1.union(num2).collect()
# [1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10]

# subtract 차집합 (정렬안함)
num1.subtract(num2).collect()
# [2, 1, 3]

~~~



### **Sampling**

무작위 값을 가져올때 쓰임

샘플링의 첫번째 파라미터는 with replacement (복원추출) 할지말지 결정 (True면 복원추출)

두번째 파라미터는 몇퍼센트를 샘플링할것인지 // 확률적으로 뽑히는것이라 리스트 길이를 보장하지는않음 (중복숫자 일경우 더 길수있으니까)

세번째 파라미터 seed 출력값고정

~~~python
# 유니온으로 합침
numUnion = num1.union(num2)
numUnion.collect()
# [1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10]


# 복원추출 ,50퍼센트 확률 샘플링 ,5개출력
# numUnion.sample(T)
# [1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10]
numUnion.sample(True, .5, seed=5).collect()

~~~



### **group by**

~~~python
foods = sc.parallelize(["짜장면", "마라탕", "짬뽕", "떡볶이", "쌀국수", "짬뽕", "짜장면", "짜장면", "짜장면",  "라면", "우동", "라면", "치킨", "돈까스", "회", "햄버거", "피자"])

# x의 0번째 글자로 그룹바이
foodsGroup= foods.groupBy(lambda x:x[0])
res= foodsGroup.collect()

for (k,y) in res:
  print(k,list(v))
~~~

<img src="https://user-images.githubusercontent.com/98302106/182378242-1dd56a12-59fb-4374-a5fc-2555db75e4fd.png" style="zoom:50%;" >



~~~python
# 2로 나누는 함수
nums=sc.parallelize([1,2,3,4,5,6,7,8,9,10])

list(nums.groupBy(lambda x: x % 2)[]
~~~

