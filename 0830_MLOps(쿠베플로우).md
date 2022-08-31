0830

쿠버네티스

### 쿠버네티스 필요한이유

서버 폭주

서비스 개수에 따른 인프라 관리의 어려움

수직적 오토스케일링

수평적 오토스케일링



ML 파이프라인 소개: https://cloud.google.com/vertex-ai/docs/pipelines/introduction?hl=ko



쿠베플로우

쿠베플로우는 다음 3가지 기능으로 정의할 수 있습니다. 
**조합가능성****Composability**쿠베플로우의 핵심 구성 요소는 이미 머신러닝 실무자들에게 익숙한 데이터 과학 도구를 사용합니다. 이들은 기계 학습의 특정 단계를 용이하게 하기 위해 독립적으로 사용되거나, 엔드 투 엔드 파이프라인을 형성하기 위해 함께 구성될 수 있다.

**이식성****Portability**컨테이너 기반 설계를 갖추고 Kubernetes 및 클라우드 네이티브 아키텍처를 활용함으로써 Kubeflow는 특정 개발환경에 종속될 필요가 없습니다. 랩톱에서 실험 및 프로토타입 작업을 수행할 수 있으며, 프로덕션 환경에 손쉽게 배포할 수 있습니다.

**확장성****Scalability**
Kubernetes를 사용하면 기본 컨테이너와 기계의 수와 크기를 변경하여 클러스터의 요구에 따라 동적으로 확장할 수 있습니다.

쿠베플로우 필요한 이유:

Kubeflow는 Kubernetes 용 ML 툴킷입니다. 다음 다이어그램은 Kubernetes를 기반으로 ML 시스템의 구성 요소를 배열하기위한 플랫폼으로서 Kubeflow를 보여줍니다.



쿠베플로우 기본 개념 

쿠베플로우의 주요 인터페이스는 대시보드입니다.

머신러닝 프로젝트의 첫 시작점은 바로 프로토타이핑과 실험입니다. 
쿠베플로우의 경우에 이 단계를 담당하는 모듈이 바로 **쿠베플로우 노트북(JupyterHub)**입니다. 

쿠베플로우의 핵심적인 구성요소는 바로 **쿠베플로우 파이프라인**입니다. 
파이프라인 오케스트레이션을 수행해주는 툴입니다. (Airflow와 비슷합니다.)

<img width="535" alt="image" src="https://user-images.githubusercontent.com/98302106/187334066-f42797fb-3877-44be-9eb5-1fcbf9940705.png">

쿠베플로우 파이프라인





도커 설치

~~~
# 우분투 접속 
chmod 600 jioni-new.pem
ssh -i jioni-new.pem ubuntu@15.164.95.210 # public ip

#도커 설치
sudo apt install docker.io

#도커 실행
docker run -i -t ubuntu/bin/bash
error!
=>docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
~~~

