1021



# airflow

## airflow : 워크플로우 작성, 스케줄링, 모니터링 작업을 프로그래밍 할수있게 해주는 플랫폼

장점 : 파이썬, UI제공,확장성(분산환경),커스터마이징쉬움

구성 : 웹서버(UI) , 스케줄러(언제 실행 관리), 메타스토어, excutor(어떻게 실행 정의), worker(실행 프로세스)

사용: 주기적인 관리, AB테스트 학습 등에 유용하게 사용할수있다.

특징: Flask 기반, 



## 워크플로우 관리 

워크플로우 : 의존성으로 연결된 작업(task)의 집합 -> **DAG** directed acyclic graph 방향 순환하지않는 그래프 : <u>**의존성 관리**</u> 

<img width="783" alt="image" src="https://user-images.githubusercontent.com/98302106/197172063-150145f3-3efa-4a29-abf1-80d54e3e6f80.png">

> 기존의 데이터 파이프라인



## 기존의 데이터 파이프라인 방식의 문제점:

- 실패 복구 : 언제 어떻게 <u>**다시 실행**</u>? 
- 모니터링 : 돌아가고 있는지  <u>**실행 확인 힘듦**</u>
- 의존성 : 파이프 라인 간 의존성 있을때 <u>**상위 과정에서 잘 실행되는지 파악힘듦**</u>
- 확장성: 중앙화된 관리 툴이 없기 때문에 <u>**분산관리 힘듦**</u>
- 배포 : 새로운 워크플로우 <u>**배포 힘듦**</u>





## Operator : 작업 정의에 사용

operator 종류

- action: 실제 연산수행

- transfer :데이터 옮김

- sensor : (테스크 언제 실행시킬 )트리거를 기다림





airflow의 one node archiecture

<img width="500" alt="image" src="https://user-images.githubusercontent.com/98302106/197206269-06daf4b4-a328-4705-aeaf-8207ce50daf5.png">

> 4가지 노드가 존재, 
> 메타스토어 (DAG의 메타정보 담고있음) 를 웹서버와 스케줄러가 정보읽어보고 executor로 실행시킨다
> DAG가 다시 메타스토어에 저장된다
> 웹서버와 스케줄러가 메타스토어 안의 DAG를 보고 태스크가 잘 완료되었는지 확인한다.
>
> queue가 executor 바깥에 존재하고있다.



airflow의 multi node architecture



<img width="600" alt="image" src="https://user-images.githubusercontent.com/98302106/197207313-0c280dfb-e7a8-483f-a1b5-926fbe480765.png">

> Sql store > scheduler > celery broker(task순서대로 들어감)  > worker> DAG to celery executor & megastore > UI와 scheduler가 실행 완료 확인함



## 동작방식

1. DAG 작성해 workflow만든다 DAG는 task로 구성되어있음

2. task는 operator가 인스턴스화 된것

3. DAG를 실행 시킬때 scheduler는 dagrun 오브젝트를 만든다

4. Dagrun 오브젝트는 task instance를 만든다

5. Worker 가 task를 수행 후 dagrun의 상태를 <u>완료</u>로 바꿔놓으면서 워크플로우 완성



<img width="609" alt="image" src="https://user-images.githubusercontent.com/98302106/197323169-ddc179bb-a8ce-4d6a-aabe-07132e7cf92c.png">

> 유저가 새로운 DAG를 작성 floder DAGs 안에 배치
>
> 웹서버와 스케쥴러가 DAG파싱한다
>
> 스케줄러가 메타스토어를 통해 DAGrun오브젝트 생성 DAGrun:DAG의 인스턴스 / DAGrun status :running
>
> 메타스토어 안 DAGrun의 Task instance 오브젝트를 (스케줄러가) 스케줄링 task instance : DAGrun의 instance
>
> 이때 트리거가 맞으면 스케줄러가 executor에게 task instace를 보낸다
>
> 실행 완료후 executor는 metastore에 완료했다 보고함
>
> 완료된 task instance는 DAGrun을 업데이트 하게된다 스케줄러는 DAGrun(DAG실행)이 완료되었나 확인 DAGRun status : completed
>
> 메타스토어에 대한 정보를 가지고 웹서버 업데이트



## Airflow 설치

```
pip --version
 
pip install apache-airflow

airflow db init #database initialization

airflow webserver -p 8080 # 8080port UI에 띄워보기
#localhost:8080
#http://43.201.25.38:8080

```

> id : team06_test
>
> Pw: team06



## Airflow cli

```bash
airflow -h # help

airflow users list
#id | username | email              | first_name | last_name | roles
#===+==========+====================+============+===========+======
#1  | team06   | syoh5188@gmail.com | admin      | team06    | Admin

airflow users create -h # user 정보help

airflow users create -u team06_test -p team06 -f jioni -l team06 -r Admin -e rnrmfwldwlddl@gmail.com 

airflow scheduler
```

```html
airflow scheduler
  ____________       _____________
 ____    |__( )_________  __/__  /________      __
____  /| |_  /__  ___/_  /_ __  /_  __ \_ | /| / /
___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /
 _/_/  |_/_/  /_/    /_/    /_/  \____/____/|__/
[2022-10-22 06:59:29 +0000] [93456] [INFO] Starting gunicorn 20.1.0
[2022-10-22 06:59:29 +0000] [93456] [ERROR] Connection in use: ('::', 8793)
[2022-10-22 06:59:29 +0000] [93456] [ERROR] Retrying in 1 second.
[2022-10-22 06:59:29,674] {scheduler_job.py:701} INFO - Starting the scheduler
[2022-10-22 06:59:29,675] {scheduler_job.py:706} INFO - Processing each file at most -1 times
[2022-10-22 06:59:29,677] {executor_loader.py:107} INFO - Loaded executor: SequentialExecutor
[2022-10-22 06:59:29,681] {manager.py:163} INFO - Launched DagFileProcessorManager with pid: 93457
[2022-10-22 06:59:29,683] {scheduler_job.py:1374} INFO - Resetting orphaned tasks for active dag runs
[2022-10-22 06:59:29,685] {settings.py:58} INFO - Configured default timezone Timezone('UTC')
[2022-10-22T06:59:29.695+0000] {manager.py:409} WARNING - Because we cannot use more than 1 thread (parsing_processes = 2) when using sqlite. So we set parallelism to 1.
[2022-10-22 06:59:30 +0000] [93456] [ERROR] Connection in use: ('::', 8793)
[2022-10-22 06:59:30 +0000] [93456] [ERROR] Retrying in 1 second.
[2022-10-22 06:59:31 +0000] [93456] [ERROR] Connection in use: ('::', 8793)
[2022-10-22 06:59:31 +0000] [93456] [ERROR] Retrying in 1 second.
[2022-10-22 06:59:32 +0000] [93456] [ERROR] Connection in use: ('::', 8793)
[2022-10-22 06:59:32 +0000] [93456] [ERROR] Retrying in 1 second.
[2022-10-22 06:59:33 +0000] [93456] [ERROR] Connection in use: ('::', 8793)
[2022-10-22 06:59:33 +0000] [93456] [ERROR] Retrying in 1 second.
[2022-10-22 06:59:34 +0000] [93456] [ERROR] Can't connect to ('::', 8793)
^C[2022-10-22 06:59:54,384] {scheduler_job.py:173} INFO - Exiting gracefully upon receiving signal 2
[2022-10-22 06:59:55,390] {process_utils.py:129} INFO - Sending Signals.SIGTERM to group 93457. PIDs of all processes in the group: [93457]
[2022-10-22 06:59:55,390] {process_utils.py:84} INFO - Sending the signal Signals.SIGTERM to group 93457
[2022-10-22 06:59:55,563] {process_utils.py:79} INFO - Process psutil.Process(pid=93457, status='terminated', exitcode=0, started='06:59:28') (93457) terminated with exit code 0
[2022-10-22 06:59:55,576] {process_utils.py:129} INFO - Sending Signals.SIGTERM to group 93457. PIDs of all processes in the group: []
[2022-10-22 06:59:55,576] {process_utils.py:84} INFO - Sending the signal Signals.SIGTERM to group 93457
[2022-10-22 06:59:55,577] {process_utils.py:98} INFO - Sending the signal Signals.SIGTERM to process 93457 as process group is missing.
[2022-10-22 06:59:55,577] {scheduler_job.py:775} INFO - Exited execute loop
```

해결

```
vi airflow.cfg
```

<img width="803" alt="image" src="https://user-images.githubusercontent.com/98302106/197328483-33161d26-27e9-4c41-a4a0-d502629e6829.png">

> 사용중이였던 포트 번호였기때문에 8083으로 바꾼후 실행



```bash
airflow scheduler

airflow dags -h
#com: backfill: 망가졌을때 고친다음 데이터 되돌려서 다시실행시킴

airflow dags list
# example_xcom

airflow tasks -h  #task 도움말

airflow tasks list example_xcom # example_xcom 이라는 이름의 DAG의 task확인
#bash_pull
#bash_push
#pull_value_from_bash_push
#puller
#push
#push_by_returning


airflow dags -h
#trigger : dag를 실행시키는 트리거

airflow dags trigger -e 2022-10-22 example_xcom #-e exec_date # trigger 설정

```



<img width="700" alt="image" src="https://user-images.githubusercontent.com/98302106/197328387-b2e10d4d-7958-4a88-a770-aa3766dced50.png">

<img width="1000" alt="image" src="https://user-images.githubusercontent.com/98302106/197328202-e279edcd-ccb7-4c9a-b7ac-59b3dc4b70b1.png">

<img width="612" alt="image" src="https://user-images.githubusercontent.com/98302106/197328323-425776f1-8049-45c1-9f0a-ea80bb530f58.png">

> http://43.201.25.38:8080
> 트리거 적용





```
# provider 설치
$ pip install apache-airflow-providers-apache-spark
$ pip install apache-airflow-providers-apache-hive
```

ERROR: Failed building wheel for sasl



```
#root에 sasl설치
sudo -i
apt-get install -y libsasl2-dev gcc python-dev
pip install sasl==0.2.1

#Successfully installed sasl-0.2.1
#WARNING: Running pip as the 'root' user can result in broken permissions and conflicting #behaviour with the system package manager. It is recommended to use a virtual environment #instead: https://pip.pypa.io/warnings/venv

$ pip install apache-airflow-providers-apache-hive
```





airflow skeleton

```
cd /home/ubuntu/airflow/dags
vi test_pipe.py

```



----



```
from datetime import datetime
from airflow import DAG
from airflow.providers.apache.hive.oprators.
default_args={
	'start_date':datetime(2022,10,22),
}


# dag가 ui에서보여주는 id
with DAG(dag_id='test_pipe',
					schedule_interval='@daily',
					default_args=default_args,
					tags=['test'],
					catchup=False) as dag:
		pass
```



Hive hook을 이용한 hive 쿼리 날리는 예제: https://louisdev.tistory.com/4?category=894285



----









