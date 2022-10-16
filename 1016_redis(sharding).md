1016

# redis-sharding



## partitioning

### vertical partition : 컬럼 나눈다 (schema 달라짐)

### horizontal partition [=sharding] : row 나눈다 (schema 달라지지 않음)



### sharding이슈 - sharding 했을때 어디에 저장해야 할까?

**sharding 의 핵심** - "특정 key를 어디에 저장할 것인가? " == "(어디서/어떻게) **특정 key를 찾는 방법**"

특정 key를 찾는 방법 - "**특정 정보는 어디에 있을까?**"  "특정 유저의 데이터는 어디에 있을까?" 



#### range : 특정 범위 대역으로 나누기 

장점: 새로운 샤딩추가 쉽다

단점: 샤딩간의 데이터가 균등하지못할가능성 높다

> User 1000명당 하나의 서버로 나눈다
> <img width="795" alt="image" src="https://user-images.githubusercontent.com/98302106/196032725-0a5ed87d-73b4-4512-8929-668dc131af0b.png">



#### modular: 서버대수로 나누기 

단점: 서버 늘어났을때 취약함 , 데이터의 이동이 심해진다 (rebelancing 이 자주 일어나면 전제 부하증가 - 읽고복사 자주해서..)

대안 : 2의 배수로 늘리기 (서버1의 데이터 절반이 서버3으로 이동 , 서버 2의 데이터가 서버4로 이동)

특징: preshared를 한다음 scale-up 전략 취한다(최대 서버개수 정하고 장비성능 올리는 방식)

<img width="419" alt="image" src="https://user-images.githubusercontent.com/98302106/196032743-588335d0-df3d-4619-bf1c-999bfd35ba87.png">

<img width="653" alt="image" src="https://user-images.githubusercontent.com/98302106/196029846-20b87406-6f8b-49b7-8a0e-dc7ffd7cf04b.png">

<img width="815" alt="image" src="https://user-images.githubusercontent.com/98302106/196032822-fe644a3f-7fe1-4c58-bd4b-f10e3d6faff4.png">



#### indexed : 특정 데이터의 위치를 가리키는 서버가 존재

장점: 데이터의 분배를 원하는 형태로 하기 쉽다

단점: indexed 자체가 하나의 서비스가 되어야함 (index자체가 SPOF될수있다: index서버 장애났을때)

대안:클라이언트가 index서버에 질의 한 내용을 기억(저장)하고 직접 서버에 질의 한다. 만약 서버와 직접 질의 했을때 에러(데이터가 서버안에 없음)일때 index에 질의 한다. [ index의 read를 완화 시켜주고 index가 장애에도 (데이터의 변경사항이 없다면) 문제없다.]

<img width="976" alt="image" src="https://user-images.githubusercontent.com/98302106/196032994-caee00fe-1baf-4aa3-a26f-d39c176f73bd.png">
<img width="350" alt="image" src="https://user-images.githubusercontent.com/98302106/196033021-26ac5298-cab8-48ef-9bdf-f6aa10513964.png">

> index라는 새로운 서버 서비스 존재함. 인덱스가 유저정보가 어느 서버에 있는지 알고있음.
>
> 1. 클라이언트가 유저 정보 index에게 요청
> 2. index가 해당 정보가 있는 서버 알려줌
> 3. 클라이언트는 해당 서버에게 정보 요청후 읽기함



불균형이 생길 경우

- **indexd에서의 Migration **



>  서버1 데이터 :2개 , 서버2 데이터:0개 , 서버3 데이터:1 => 불균형
> <img width="731" alt="image" src="https://user-images.githubusercontent.com/98302106/196038409-70e2fea8-90a6-4d28-a903-80277a56fd3f.png">
>
> 1. admin tool (관리자 도구)에서 데이터 옮기는 요청(마이그레이션)을 index에게 함
> 2. 옮기는 데이터를 이동중으로 바꿈 (read만 가능하게 혹은 서비스 에러를 낸다)
> 3. 서버1에서 서버2로 데이터 복사
> 4. 데이터 복사 종료후 유저상태 정상으로 변경 





- **Indexed 장애의 완화 **(대안과 같은 내용) 

클라이언트는 index요청후에 user 위치 정보 (서버 정보) 저장, 이후에는 가지고 있는 정보로 요청 (조건: 클라이언트가 해당정보 처리할수 있도록 되어야한다)



#### complexed

장점: 여러가지 형태를 다 섞어서 여러 장점을 모두 취할 수 있다.

단점: 구현이 복잡하다



<img width="743" alt="image" src="https://user-images.githubusercontent.com/98302106/196040748-4b02ceb3-951e-45f9-8630-c65ca0381301.png">

> index에서 range 사용
>
> 서버에서 primary : write , replica : read // active, stand by =>  등등 DB 확장할수있다
>
> DB뿐만 아니라 서비스환경에서도 적용할수있다 : cell architecture





## 실습



```
# zookeeper 실행
cd zookeeper/bin
ll
# 이미 실행중이여서 종료해줬다
./zkServer.sh stop
#재실행
./zkServer.sh start
# cli 접속
./zkCli.sh
ls /
#출력 [hadoop-ha, zookeeper]

# 지워보기 
deleteall /hadoop-ha
ls
# [zookeeper] // 처음처럼 주키퍼만 남아있음

# 다른 터미널에서 shard 라는 디렉토리 만들고 (아마 /usr/local 같음)
cd shard #예시
vi app.ini #예시


```

> <img width="467" alt="image" src="https://user-images.githubusercontent.com/98302106/196043395-98d5f5a4-3825-4429-bf9a-1d38dd851679.png">
>
> host: 주키퍼의 주소
>
> Path: 어떤키를 쓸껀지 등록



```
# 4대의 서버로 range shard 한다
vi zoo_setup.py

```

>  <img width="440" alt="image" src="https://user-images.githubusercontent.com/98302106/196046338-89a50df4-4b52-474b-bd21-6875244fe179.png">
>
> **End 값을 -1** 로 해서 2000이 초과되는 data는 서버2에 저장된다. 중요!!!!!!!!!!!!!!!!



```
python zoo_setup.py # 값을 저장

# cil 터미널로 교체
ls /

# /를 노드 생성할땐 붙이면 안되고 앞에 붙여서 위치를 바꾼다
ls /the_red # 예시
#[storages]
ls /the_red/strages/redis/shards/ranges
#[]
get /the_red/strages/redis/shards/ranges
# {"0"}:{"host":redis0127.0.0.1",....}  # data 내용 보임
```



```
# main.py에서
```

<img width="395" alt="image" src="https://user-images.githubusercontent.com/98302106/196048361-a64bcd47-7493-447c-904b-b46167b03370.png">

> 샤드가 바뀔때마다 샤드매니저를 다시만들어서 샤드 정보를 저장하고있다

