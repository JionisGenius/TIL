0725

# Apache Spark

빅데이터 처리를 위한 오픈소스 고속 분산처리 엔진

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725140954519.png" alt="image-20220725140954519" style="zoom: 40%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725141417573.png" alt="image-20220725141417573" style="zoom: 40%;" />

> 거의 모든회사가 apache spark를 사용하고있다 
>
> 이 회사들이 느끼는 공통적인 문제 : 빅데이터문제 [크기,속도,다양성] 빠르게 증가되고있다
>
> 그래서 야후가 hadoop을 개발한다
>
> 하둡은 파일시스템/연산 엔진/ 리소스 관리 클러스터 매니저 세파트로 나누어져있다.
>
> 그 중 스파크는 map reduce 를 대체하는 프로젝트이다

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725141626348.png" alt="image-20220725141626348" style="zoom: 45%;" />



> 컴퓨터가 연산을 시작하면 하드디스크에서 cpu까지 데이터가 위로 이동한다.
> cpu가 데이터를 더 빨리 접근하기 위해, 연산에 자주 쓰이는 데이터는 위로 가게된다.
>
> 연산에 잘 쓰이지 않는 데이터는 아래로 가게된다.
>
> 한칸씩 이동할때마다 속도차이가 심하게 난다
>
>  ex.작은데이터 여러개 HDD에서 RAM으로 하나씩 이동하면서 쓰기에 너무느려짐
>
> > 데이터 처리 : **데이터**를 여러개로 **쪼개기**/ 쪼갠 데이터를 여러 노드의 메모리에 **동시에 처리**한다
> >
> > <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725142522976.png" alt="image-20220725142522976" style="zoom:33%;" />
> >
> > 그러므로 여러컴퓨터를 사용하면서 **InMemory** 가능하게 되었다.
> >
> > 고속= 빅데이터의 in-memory연산



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725142846494.png" alt="image-20220725142846494" style="zoom:43%;" /><img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725142907591.png" alt="image-20220725142907591" style="zoom: 35%;" />



#### pandas vs spark

pandas는 35GB에 도달했을 쯤부터 속도가 느려지고 메모리 에러로 연산을 할수없게된다.

spark는 확장성을 고려했기 때문에 데이터양이 증가해도 꾸준한 속도를 유지하면서 연산을 할 수 있다.

spark는 노드를 필요에 따라 계속 늘릴수 있기 때문이다.

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725143238297.png" alt="image-20220725143238297" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725145608954.png" alt="image-20220725145608954" style="zoom:40%;" />



#### hadoop의 mapReduce보다 빠르다 

(메모리상 100배 디스크상 10배)

#### Lazy Evaluation 

결과가 필요할때 연산한다 , 기다리면서 연산과정을 최적화시킬 수 있다.

#### RDD Resilien Distributed Dataset

- 여러분산노드에 걸쳐서 저장
- 변경이 불가능
- 여러개의 파티션으로 분리



#### Spark 구성

- Spark Core
- Spark SQL
- Spark Streaming
- MLlib
- GraphX

(위에서부터 기능 추가되었음)



## RDD

#### Resilien Distributed Dataset

**탄력적인 분산 데이터셋**



Sc-> SparkContext

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725150713601.png" alt="image-20220725150713601" style="zoom:50%;" />

> lines 가 RDD



### RDD의 5가지 특징

1. 데이터 추상화
2. Resilient & Immutable 탄력적이고 불변하다
3. Type-safe
4. Unstructured / Structured Data
5. Lazy



1. 데이터 추상화 

데이터는 클러스터에 흩어져있지만 하나의 파일인것 처럼 사용이 가능하다

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725150934654.png" alt="image-20220725150934654" style="zoom:35%;" />



2. Resilient & Immutable

데이터가 여러군데에서 연산

여러노드중 하나가 망가지는 일이 많다.

- 네트워크 장애
- 하드웨어 /메모리 문제
- 알수없는 갖가지 이유

 데이터가 불변하면 문제가 일어났을때 데이터 복원이 가능하다



**Immutable** :**RDD1이 변환**을 거치면 RDD1이 바뀌는게아니라 **새로운 RDD2**가 만들어진다

변환을 거칠때마다 연산의 기록이 남는다

변환 과정은 비순환 그래프 (Acyclic Graph)로 그릴수 있게 된다.

**Resilient**: 덕분에 문제가 생길때 쉽게 전 RDD로 돌아갈수 있다.

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725151425014.png" alt="image-20220725151425014" style="zoom:40%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725151535551.png" alt="image-20220725151535551" style="zoom:40%;" />

3. Type-safe

컴파일시 Type을 판별할 수 있어 문제를 일찍 발견할수있다. -> 개발자친화적임

4. Unstructured / Structured 둘다 담을 수 있다.

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725152632021.png" alt="image-20220725152632021" style="zoom:45%;" />

5. Lazy하다.  결과가 필요할때까지 연산을 하지않는다

T=변환

A=액션

액션A을 할때까지 변환T은 실행되지 않는다

액션을 만나야지 전부실행된다 => Lazy Evaluation

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725152827340.png" alt="image-20220725152827340" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725163006326.png" alt="image-20220725163006326" style="zoom:50%;" />



### why use RDD

- 유연하다
- 짧은코드로 많은것을 할 수 있다.
- 개발할때 **무엇**보다는 **어떻게**에 더 생각하게 된다.
  - 게으른 연산 덕분에 데이터가 어떻게 변환될지 생각
  - 데이터가 지나갈 길을 닦는 느낌





## 모빌리티 데이터 다운로드



~~https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page~~ ~~페이지 이사감~~

~~https://tlcanalytics.shinyapps.io/datahub/ parquet~~
https://data.cityofnewyork.us/widgets/8wbx-tsch?mobile_redirect=true csv

Taxi&Limousine Commission

#### NewYork TLC Trip Record Data 

1. 10+년 이상의 택시와 모빌리티 서비스 기록 (2009-2021)
2. 매년 20GB씩 쌓이는 dataset (승하차 시간 장소, 소요시간,요금 데이터 포함)
3. 2020년 우버 데이터 사용

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220721161728772.png" alt="image-20220721161728772" style="zoom: 50%;" />

![image-20220725113026257](/Users/krc/Library/Application Support/typora-user-images/image-20220725113026257.png)



~~~python
#spark 에서 parquet 읽기
val data= spark.read.parquet("PATH")data.write.parquet("PATH")
df=pd.read._parquet('sample.parquet')
~~~



> hvfhs_license_num : 우버 회사와 리프트 회사 라이선스넘버
>
> - HV002:  juno
> - HV003: Uber
> - HV004: VIa
> - HV005: Lyft
>
> Dispatching_base_num: 장소마다 디스패치되었는지 베이스넘버
>
> Pickip_datetime:승객이 탄 시간
>
> Dropoff_datatime: 하차시간
>
> PULocationID: Pickup loation ID (장소마다 id를 부여해 줌) 승차한 장소 id
>
> DOLocationID: Dropoff loation ID 하차한 장소의 id
>
> SR_FLag: shelld ride? 맞으면1 아니면 null(0)=> 직행

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220725114314571.png" alt="image-20220725114314571" style="zoom: 50%;" />



~~~python
# 패키지를 가져오고
from pyspark import SparkConf, SparkContext
import pandas as pd

# Spark 설정
conf = SparkConf().setMaster("local").setAppName("uber-date-trips") #마스터 로컬 설정 # 앱 이름 설정
sc = SparkContext(conf=conf) # conf 초기화

# 우리가 가져올 데이터가 있는 파일
directory = "/Users/krc/Documents/Spark/data"
filename = "2022FHV.csv"

# 데이터 파싱
lines = sc.textFile(f"file:///{directory}/{filename}") 
header = lines.first() 
filtered_lines = lines.filter(lambda row:row != header) 

# 필요한 부분만 골라내서 세는 부분
# countByValue로 같은 날짜등장하는 부분을 센다
dates = filtered_lines.map(lambda x: x.split(",")[2].split(" ")[0]) #날짜만 추출
result = dates.countByValue() # 추출한 데이터 세기

# 아래는 Spark코드가 아닌 일반적인 파이썬 코드
# CSV로 결과값 저장 
pd.Series(result, name="FHV").to_csv("FHV_date.csv") #판다스 시리즈로 바꾼다음 csv로 저장

# 터미널 실행 spark-submit count_trips.py
# 검색창 http://localhost:4040/jobs 현재 실행되는것 볼수있다.

~~~



