0905

# 기업 연계 프로젝트

>  (주)리테일앤인사이트

## 프로젝트 주제: 빅데이터 분석 프레임워크 구축

#### **프로젝트 내용 및 요구사항:** 

> 금융, 유통군들에서 빅데이터는 필수불가결한 솔루션
>
> 
>
> 빅데이터 구축 시 마다 분석/설계 부터 인력 투입 커스터마이징까지 많은 견적이 요소가 포함되어
>
> 금액 부담이 많음
>
> 
>
> 빅데이터 엔지니어링을 통한 수집부터, MLOps를 구축하여 파이프라이닝되는 과정까지를 표준화 하여
>
> 공통된 Middleware 프레임워크를 구축하기 위한 핵심 요소를 구현
>
> 
>
> 향후
>
> 프레임워크가 솔루션화가 된다면
>
> 입력되는 데이터와 출력되는 결과물은 최초는 커스터마이징/구축 형태에서
>
> 점차 템플릿화 하여 제공한다면 실제 솔루션 제품까지도 생산이 가능하게 되고
>
> 이를 위한 핵심기술에 참여/기여 하게 된다면 좋은 포트폴리오와 비전을 가지게 될 것으로 기대

#### **참고 자료:**

>  빅데이터 저장 - Hadoop & Hive, Apache HBase
> Cluster&Configuration Center - Zookeeper
> Cluster computing framework - Spark
> ML Pipeline - MLOps 환경

https://wikidocs.net/26170



#### **필요스택:**  **Hadoop, Zookeeper, Hive, Map reducing 활용, HBase, Spark, MLOps**



#### **프로젝트 진행단계:** 

> 1. 시장 조사
>
> 2. 구현 범위
>
> 3. 핵심 기술 및 구성
>
> 4. 데모 제작
>
> 5. 시연
>
> 6. 보완



------



**9/5 회의**

1. 최소한 RFB에 나와있는 프레임워크들이 어떤 기능을 하는지 전체적인 흐름에서 알아오기. + 전체적인 흐름과 각 과정에서 사용되는 프레임워크 알아오기
2. 기획에 대해서 큰 틀을 생각해오기(어떤 아이템이 있을지 등등)
3. 하루마다 공부한 것 공유하기! (너무 자세하게 설명할 필요 없이 무엇을 공부했는지 정도)

다음 회의 : 9/6 화 14:00 회의 시작



----------

출처: https://wikidocs.net/26170



# 빅데이터 에코시스템

수집, 정제, 적재, 분석, 시각화 단계를 거치는 동안 사용하는 기술들을 통틀어 빅데이터 에코시스템 이라고 한다.



## 1. 데이터 수집 기술



> - **Flume**
>
>   - 플룸은 많은 양의 로그 데이터를 효율적으로 **수집, 취합, 이동**하기 위한 **분산형 소프트웨어**
>   - 플룸은 클라우데라에서 개발한 서버 로그 수집 도구 입니다. 각 서버에 에이전트가 설치 되고, 에이전트로부터 데이터를 전달 받는 콜렉터로 구성됩니다.
>
> - **Kafka**
>
>   - 오픈 소스 **메시지 브로커** 프로젝트. 메시징, 메트릭 수집, 로그 수집, 스트림 처리 등
>     카프카는 링크드인에서 개발한 분산 메시징 시스템입니다. **대용량 실시간 로그 처리에 특화** 되어 있습니다.
>
>   - 특징: 
>
>     - 빠르다: **Fast**
>
>       수 천개의 데이터 소스로 부터 초당 수백 메가바이트의 데이터를 입력 받아도 안정적으로 처리 가능
>
>     - 확장가능: **Scalable**
>
>       메시지를 파티션으로 분리햐여 분산 저장, 처리할 수 있어 클러스터로 구성하여 확장 가능
>
>     - 안정적이다: **Durable**
>
>       클러스터에 파티션 복제하여 장애 내구성을 가짐
>
> - Sqoop
>
>   - Sqoop은 관계형 데이터베이스와 하둡 HDFS간에 데이터를 전송할 수 있도록 설계된 오픈소스 소프트웨어 입니다.
>     대용량 데이터들을 효율적으로 변환





## 2. 작업 관리 기술

작업 관리 기술은 빅데이터를 분석하는 여러가지 단계를 효율적으로 생성, 관리하고 모니터링 할 수 있게 도와주는 기술 입니다.

> - **airflow**
>
> - <img width="591" alt="image" src="https://user-images.githubusercontent.com/98302106/188376310-0fd2ce08-6eb4-4ca4-afd7-181017198ff5.png">
>
>   > 이미지 출처: https://wikidocs.net/22651#_2
>
>   - 에어플로우는 에어비앤비에서 개발한 **데이터 흐름의 시각화, 스케쥴링, 모니터링이 가능한 워크플로우** 플랫폼입니다. 하이브, 프레스토, DBMS 엔진과 결합하여 사용 할 수 있습니다.
>   - 파이썬 기반으로 태스크 코드를 작성할수있다.
>     여러 머신에 분산하여 수행 장점
>     GCP권장
>     Airflow 콘솔이 따로 존재해 Task 관리를 서버에서 들어가 관리하지 않아도 되고, 각 작업별 시간이 나오기 때문에 bottleneck을 찾을 때에도 유용함



## 3. 데이터 직렬화

빅데이터 에코 시스템이 다양한 기술과 언어로 구현되기 때문에 각 언어간에 내부 객체를 공유해야 하는 경우가 있습니다. 이를 효율적으로 처리하기 위해서 데이터 직렬화기술을 이용합니다.

> - Avro
> - Thrift
> - Protocol Buffers



## 4. 저장

빅데이터는 대용량의 데이터를 저장하기 때문에 데이터의 저장의 안정성과 속도가 중요합니다. HDFS외에도 아마존 AWS의 S3, MS Azure의 Data Lake, Blob Storage, Google의 Cloud Storage가 있습니다.

> - **HDFS**
>   - 하둡 분산 파일 시스템(HDFS, Hadoop distributed file system)은 하둡 프레임워크를 위해 자바 언어로 작성된 분산 확장 파일 시스템입니다. HDFS는 범용 컴퓨터를 클러스터로 구성하여 대용량의 파일을 블록단위로 분할하여 여러서버에 복제하여 저장합니다.
> - S3
>   - S3는 아마존에서 제공하는 인터넷용 저장소입니다.



## 5. NoSQL 

> - **HBase**
>   -  HDFS 기반의 칼럼 기반 **NoSQL 데이터베이스**입니다. 실시간 랜덤 조회 및 업데이트가 가능하며, 각 프로세스는 개인의 데이터를 비동기적으로 업데이트할 수 있습니다.
>   - HBase는 압축, 인메모리 처리, 초기 빅테이블에 제시되어 있는 [Bloom 필터](https://ko.wikipedia.org/wiki/블룸_필터) 기능을 제공한다.[[2\]](https://ko.wikipedia.org/wiki/아파치_HBase#cite_note-2) HBase에 있는 테이블들은 하둡에서 동작하는 [맵리듀스](https://ko.wikipedia.org/wiki/맵리듀스) 작업을 위한 입출력을 제공하며 자바 [API](https://ko.wikipedia.org/wiki/API)나 [REST](https://ko.wikipedia.org/wiki/REST), [Avro](https://ko.wikipedia.org/wiki/아파치_아브로) 또는 [Thrift](https://ko.wikipedia.org/wiki/Thrift) 게이트웨이를 통하여 접근할 수 있다.
>     출처:https://ko.wikipedia.org/wiki/%EC%95%84%ED%8C%8C%EC%B9%98_HBase



## 6. 데이터 처리

데이터 처리는 빅데이터를 분석하는 기술입니다. 하둡의 맵리듀스를 기반으로, 스파크, 하이브, HBase, 임팔라등 여러가지 기술이 있습니다.

> - **spark**
>   - 스파크(Spark)는 인메모리 기반의 범용 데이터 처리 플랫폼입니다. 배치 처리, 머신러닝, SQL 질의 처리, 스트리밍 데이터 처리, 그래프 라이브러리 처리와 같은 다양한 작업을 수용할 수 있도록 설계되어 있습니다. 
> - **hive**
>   - 하이브(Hive)는 하둡 기반의 데이터웨어하우징용 솔루션입니다. 페이스북에서 개발했으며, 오픈소스로 공개되며 주목받은 기술입니다. SQL과 매우 유사한 HiveQL이라는 쿼리 언어를 제공합니다. 그래서 자바를 모르는 데이터 분석가들도 쉽게 하둡 데이터를 분석할 수 있게 도와줍니다. HiveQL은 내부적으로 맵리듀스 잡으로 변환되어 실행됩니다.



## 7. 클러스터 관리

빅데이터는 단일 시스템이 보다는 보통 클러스터로 처리 되기 때문에 자원의 효율적인 사용이 필요합니다. 클러스터의 관리를 위한 여러가지 기술이 있습니다.

> - **YARN**
>   - 얀(YARN)은 데이터 처리 작업을 실행하기 위한 클러스터 자원(CPU, 메모리, 디스크등)과 스케쥴링을 위한 프레임워크입니다. 기존 하둡의 데이터 처리 프레임워크인 맵리듀스의 단점을 극복하기 위해서 시작된 프로젝트이며, 하둡2.0부터 이용이 가능합니다. 맵리듀스, 하이브, 임팔라, 타조, 스파크 등 다양한 애플리케이션들은 얀에서 리소스를 할당받아서, 작업을 실행하게 됩니다.
> - Mesos



## 8. 분산 서버관리

클러스터에서 여러가지 기술이 이용될 때 하나의 서버에서 모든 작업이 진행되면 이 서버가 단일실패지점(SPOF)가 됩니다. 이로 인한 리스크를 줄이기 위해 분산 서버 관리 기술을 이용합니다.

> - **Apache Zookeeper**
>
>   - 분산 환경에서 **서버 간의 상호 조정**이 필요한 다양한 서비스를 제공하는 시스템으로, 크게 다음과 같은 네 가지 역할을 수행합니다.
>
>     첫째, 하나의 서버에만 서비스가 집중되지 않게 서비스를 알맞게 **분산해 동시에 처리**하게 해줍니다.
>     둘째, 하나의 서버에서 처리한 결과를 다른 서버와도 동기화해서 데이터의 **안정성을 보장**합니다. 
>     셋째, 운영(active) 서버에 문제가 발생해서 서비스를 제공할 수 없을 경우, 다른 대기 중인 서버를 운영 서버로 바꿔서 서비스가 **중지 없이 제공**되게 합니다. 
>     넷째, 분산 환경을 구성하는 서버의 환경설정을 **통합적으로 관리**합니다.
>
>     예를 들면 HA(High Availability) 구성된 HDFS 네임노드의 Active 노드 선출, HBase 리전서버의 Active 서버 선출, Hiveserver2의 다중 선택등을 지원합니다.



## 9. 시각화

> - Zeppelin
>   - 한국의 NFLab이라는 회사에서 개발하여 Apache top level 프로젝트로 최근 승인 받은 오픈소스 솔루션으로, Notebook 이라고 하는 웹 기반 Workspace에 Spark, Tajo, Hive, ElasticSearch 등 다양한 솔루션의 API, Query 등을 실행하고 결과를 웹에 나타내는 솔루션입니다.



## 10. 보안 

> - Ranger
>   - 레인저는 하둡 클러스터의 각 모듈에 대한 보안 정책을 관리할 수 있습니다. HDFS의 ACL, Hive 데이터베이스의 접근권한 등의 보안 정책과 각 모듈에 대한 접근 기록(Audit)을 보관합니다.



## 11.데이터 거버넌스

데이터 거버넌스는 기업의 여기저기 산재한 데이터를 같은 저장소에 관리, 비정형 데이터를 규칙에 맞게 표준화하는 전사 차원의 빅데이터 관리 체계입니다.

> - Atlas
>   - 아틀라스는 데이터 거버넌스로 조직이 보안/컴플라이언스 요구사항을 준수할 수 있도록 지원합니다. 
>     데이터 자원에 대한 태깅, 다운스트림 데이터셋에 대한 태그 전파, 메타 데이터 접근에 대한 보안등 다양한 기능을 가지고 있습니다. 
>     메타데이터 변경 알림 기능을 제공하고, Hive, HBase, Kafka의 데이터가 변경되는 것을 알리는 기능을 제공합니다.



-----



# 빅데이터 시스템 예제

출처: https://wikidocs.net/82569

-----





## 하둡

하둡은 하나의 성능 좋은 컴퓨터를 이용하여 데이터를 처리하는 대신, 적당한 성능의 범용 컴퓨터 여러 대를 클러스터화하고, **큰 크기의 데이터를 클러스터에서 병렬로 동시에 처리하여 처리 속도를 높이는 것**을 목적으로 하는 **분산처리를 위한 오픈소스 프레임워크**라고 할 수 있습니다.



**하둡의 구성 모듈**

- Hadoop Common
  - 하둡의 다른 모듈을 지원하기 위한 공통 컴포넌트 모듈
- Hadoop HDFS
  - 분산저장을 처리하기 위한 모듈
  - 여러개의 서버를 하나의 서버처럼 묶어서 데이터를 저장
- Hadoop YARN
  - 병렬처리를 위한 클러스터 자원관리 및 스케줄링 담당
- Hadoop Mapreduce
  - 분산되어 저장된 데이터를 병렬 처리할 수 있게 해주는 분산 처리 모듈
- Hadoop Ozone
  - 하둡을 위한 오브젝트 저장소



**장단점**

- **장점**
  - 오픈소스로 라이선스에 대한 **비용 부담이 적음**
  - **시스템을 중단하지 않고, 장비의 추가가 용이(Scale Out)**
  - 일부 장비에 장애가 발생하더라도 전체 시스템 사용성에 영향이 적음(Fault tolerance)
  - 저렴한 구축 비용과 비용대비 **빠른 데이터 처리**
  - 오프라인 **배치** 프로세싱에 **최적화**
- **단점**
  - HDFS에 저장된 **데이터를 변경 불가**
  - **실시간** 데이터 분석 같이 신속하게 처리해야 하는 작업에는 **부적합**
  - 너무 많은 버전과 부실한 서포트
  - 설정의 어려움



## 하이브

하이브는 하둡 에코시스템 중에서 데이터를 모델링하고 프로세싱하는 경우 가장 많이 사용하는 **데이터 웨어하우징용 솔루션**입니다.

RDB의 데이터베이스, 테이블과 같은 형태로 HDFS에 저장된 데이터의 구조를 정의하는 방법을 제공하며, 이 데이터를 대상으로 SQL과 유사한 HiveQL 쿼리를 이용하여 데이터를 조회하는 방법을 제공합니다.

하이브는 SQL을 하둡에서 사용하기 위한 프로젝트로 시작되었습니다. 페이스북에서 자사의 데이터 분석을 위해 개발하여 아파치 오픈소스 프로젝트로 넘어왔습니다.



**구성요소**

- UI
  - 사용자가 쿼리 및 기타 작업을 시스템에 제출하는 사용자 인터페이스
  - CLI, Beeline, JDBC 등
- Driver
  - 쿼리를 입력받고 작업을 처리
  - 사용자 세션을 구현하고, JDBC/ODBC 인터페이스 API 제공
- Compiler
  - 메타 스토어를 참고하여 쿼리 구문을 분석하고 실행계획을 생성
- Metastore
  - 디비, 테이블, 파티션의 정보를 저장
- Execution Engine
  - 컴파일러에 의해 생성된 실행 계획을 실행



<img width="747" alt="image" src="https://user-images.githubusercontent.com/98302106/188393015-bcdb77fb-f4ae-45f5-8151-d71e6a3f89c5.png">

> 이미지 출처 :https://wikidocs.net/23282



## HDFS

HDFS(Hadoop Distributed File System)는 범용 하드웨어에서 동작하고, 장애 복구성을 가지는 분산 파일 시스템을 목표로 합니다.

HDFS는 실시간 처리보다는 배치처리를 위해 설계되었습니다. 따라서 빠른 데이터 응답시간이 필요한 작업에는 적합하지 않습니다. 또한 네임노드가 단일 실패 지점(SPOF)이 되기 때문에 네임노드 관리가 중요합니다.



#### 특징

- 블록 단위 저장
- 블록 복제를 이용한 장애 복구
- 읽기 중심
- 데이터 지역성



#### 구조

HDFS는 마스터 슬레이브 구조로 하나의 네임노드와 여러 개의 데이터노드로 구성됩니다. 네임노드는 메타데이터를 가지고 있고, 데이터는 블록 단위로 나누어 데이터노드에 저장됩니다. 사용자는 네임노드를 이용해 데이터를 쓰고, 읽을 수 있습니다.



## 맵리듀스 map reduce

맵리듀스는 간단한 단위작업을 반복하여 처리할 때 사용하는 프로그래밍 모델입니다. 간단한 단위작업을 처리하는 맵(Map) 작업과 맵 작업의 결과물을 모아서 집계하는 리듀스(Reduce) 단계로 구성됩니다.

하둡에서 분산처리를 담당하는 맵리듀스 작업은 맵과 리듀스로 나누어져 처리됩니다. 맵, 리듀스작업은 병렬로 처리가 가능한 작업으로, 여러 컴퓨터에서 동시에 작업을 처리하여 속도를 높일 수 있습니다.

​            