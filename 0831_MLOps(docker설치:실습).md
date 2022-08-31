0831

Docker 실습



Aws ubuntu20.04 실행

~~~bash
$ ssh -i jioni-new.pem ubuntu@13.124.47.177 #public ip #pem file 저장소에서 실행하기
~~~



## 1. Docker 설치

- 공식 문서
  - https://docs.docker.com/engine/install/ubuntu/
  - 여러가지 install 방법 중, **Install using the repository** 방법으로 진행하겠습니다.
- 앞으로 어떤 오픈소스를 사용하더라도, 블로그가 아닌 공식 문서를 참고하시는 습관을 들이는 것을 추천드리겠습니다.

### 1) Set up the repository

- 먼저 apt 라는 패키지 매니저를 업데이트합니다.

  ```bash
  $ sudo apt-get update
  ```

- 그리고, docker 의 prerequisite package 들을 설치합니다.

  ```bash
  $ sudo apt-get install \\
      apt-transport-https \\
      ca-certificates \\
      curl \\
      gnupg \\
      lsb-release
  ```

- Docker 의 GPG key 를 추가합니다.

  ```bash
  $ curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  ```

- 그 다음 stable 버전의 repository 를 바라보도록 설정합니다.

  ```bash
  $ echo \\
    "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] <https://download.docker.com/linux/ubuntu> \\
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

  - 혹시 arm 기반의 cpu 라면 다음을 입력해주세요

    ```bash
    echo \\
      "deb [arch=arm64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] <https://download.docker.com/linux/ubuntu> \\
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

### 2) Install Docker Engine

- Docker 엔진의 최신 버전을 설치합니다.

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

- 혹시 특정 버전의 Docker 를 설치하고 싶다면 매뉴얼을 따라해주세요

### 3) 정상 설치 확인

- docker container 를 실행시켜, 정상적으로 설치되었는지 확인합니다.

  ```bash
  $ sudo docker run hello-world
  ```

- 다음과 같이 출력된다면, 정상적으로 설치가 된 것을 확인할 수 있습니다

  ```bash
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  b8dfde127a29: Pull complete 
  Digest: sha256:0fe98d7debd9049c50b597ef1f85b7c1e8cc81f59c8d623fcb2250e8bec85b38
  Status: Downloaded newer image for hello-world:latest
  
  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  
  To generate this message, Docker took the following steps:
   1. The Docker client contacted the Docker daemon.
   2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
      (amd64)
   3. The Docker daemon created a new container from that image which runs the
      executable that produces the output you are currently reading.
   4. The Docker daemon streamed that output to the Docker client, which sent it
      to your terminal.
  
  To try something more ambitious, you can run an Ubuntu container with:
   $ docker run -it ubuntu bash
  
  Share images, automate workflows, and more with a free Docker ID:
   <https://hub.docker.com/>
  
  For more examples and ideas, visit:
   <https://docs.docker.com/get-started/>
  ```

## 2. Docker 권한 설정

- 현재는 모든 docker 관련 작업이 root 유저에게만 권한이 있기 때문에, docker 관련 명령을 수행하려면 `sudo` 를 앞에 붙여주어야만 가능합니다.

- 예를 들면,

  - `docker ps` 를 수행하면 다음과 같이 Permission denied 라는 메시지가 출력됩니다.

  ```
  Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json?all=1: dial unix /var/run/docker.sock: connect: permission denied
  ```

- 따라서, root 유저가 아닌 host 의 기본 유저에게도 권한을 주기 위해 다음과 같은 명령을 **새로 띄운 터미널에서 수행해주셔야 합니다.**

  ```bash
  $ sudo usermod -a -G docker $USER
  $ sudo service docker restart
  ```

- 그 다음, VM 을 로그아웃 한 다음에 다시 로그인하면 다음과 같이 정상적으로 출력되는 것을 확인하실 수 있습니다.

  ```bash
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```

------

------

## 3. Docker 의 기본적인 명령

### 1) Docker pull

- docker image repository 부터 Docker image 를 가져오는 커맨드입니다.

```bash
$ docker pull --help
```

- 예시)

  ```bash
  $ docker pull ubuntu:18.04
  ```

  - [docker.io/library](http://docker.io/library/) 라는 이름의 repository 에서 ubuntu:18.04 라는 image 를 여러분의 노트북에 다운로드 받게됩니다.

- 참고사항

  - 추후 

    docker.io

     나 public 한 docker hub 와 같은 repository 대신에, 특정 private 한 repository 에서 docker image 를 가져와야 하는 경우:

    - docker login 을 통해서 특정 repository 를 바라보도록 한 뒤, docker pull 을 수행하는 형태로 사용합니다.

### 2) Docker images

- 로컬에 존재하는 docker image 리스트를 출력하는 커맨드입니다.

```bash
$ docker images --help
```

- 예시)

  ```bash
  $ docker images
  ```

### 3) Docker ps

- 현재 실행중인 도커 컨테이너 리스트를 출력하는 커맨드입니다.

```bash
$ docker ps --help
```

- 예시)

  ```bash
  $ docker ps
  $ docker ps -a
  ```

### 4) Docker run

- 도커 컨테이너를 실행시키는 커맨드입니다.

```bash
$ docker run --help
```

- 옵션에 대한 자세한 내용은 사용하게 될 때마다 하나씩 설명 예정

- 예시)

  ```bash
  $ docker run -it --name jioni  ubuntu:20.04 /bin/bash
  ```

  -  `-it`  :  `-i` +`-t` 옵션

  - container 를 실행시킴과 동시에 interactive 한 terminal 로 접속시켜주는 옵션

  - `--name` : name

    : 컨테이너 id 대신, 구분하기 쉽도록 지정해주는 이름

  - `/bin/bash`

    : dj컨테이너를 실행시킴과 동시에 실행할 커맨드로, `/bin/bash` 는 bash 터미널을 사용하는 것을 의미합니다.

### 5) Docker exec

- Docker 컨테이너 내부에서 명령을 내리거나, 내부로 접속하는 커맨드

```bash
$ docker exec --help
```

- 예시)

  ```bash
  $ docker run -it -d --name jioni2 ubuntu:20.04
  $ docker ps
  ```

  - `-d` : 백그라운드에서 실행시켜서, 컨테이너에 접속 종료를 하더라도, 계속 실행 중이 되도록 하는 커맨드

  ```bash
  $ docker exec -it jioni2 /bin/bash
  ```

  - 아까와 동일하게 container 내부에 접속할 수 있는 것을 확인 가능

### 6) Docker logs

- 도커 컨테이너의 log 를 확인하는 커맨드

```bash
$ docker logs --help
```

- 예시)

  ```bash
  $ docker run --name demo3 -d busybox sh -c "while true; do $(echo date); sleep 1; done"
  ```

  - test 라는 이름의 busybox 이미지를 백그라운드에서 도커 컨테이너로 실행하여, 1초에 한 번씩 현재 시간을 출력하는 커맨드

  ```bash
  $ docker logs demo3
  $ docker logs demo3 -f
  ```

  - `-f` 옵션: 계속 watch 하며 출력 / 게속해서 로그 트래킹

### 7) Docker stop

- 실행 중인 도커 컨테이너를 중단시키는 커맨드

```bash
$ docker stop --help
```

- 예시)

  ```bash
  $ docker stop demo3
  $ docker stop demo2
  $ docker stop demo1
  ```

### 8) Docker rm

- 도커 컨테이너를 삭제하는 커맨드

```bash
$ docker rm --help
```

- 예시)

  ```bash
  $ docker rm demo3
  $ docker rm demo2
  $ docker rm demo1
  ```

  - docker ps, docker ps -a 에서 모두 출력되지 않는 것을 확인하실 수 있습니다.

### 9) Docker rmi

- 도커 이미지를 삭제하는 커맨드

```bash
$ docker rmi --help
```

- 예시)

  ```bash
  $ docker images
  # busybox, ubuntu 가 있는 것을 확인하실 수 있습니다.
  $ docker rmi ubuntu
  $ docker rmi ubuntu:20.04 # 이미지 삭제
  ```

<img width="815" alt="image" src="https://user-images.githubusercontent.com/98302106/187600308-cb860c6f-83df-48b1-ba50-12bf53fa8e45.png">



# docker image



## 0. Docker Image

- Docker image :
  - 어떤 애플리케이션에 대해서,
  - 단순히 애플리케이션 코드뿐만이 아니라,
  - 그 애플리케이션과 dependent 한 모든 것을 함께 패키징한 데이터
- Dockerfile
  - 사용자가 도커 이미지를 쉽게 만들 수 있도록, 제공하는 템플릿

------

## 1. Dockerfile

### 1) Dockerfile 만들기

- `Dockerfile` 이라는 이름으로 빈 file 을 하나 만들어봅니다.

```bash
# home 디렉토리로 이동합니다.
$ cd $HOME

# docker-practice 라는 이름의 폴더를 생성합니다.
$ mkdir docker-practice

# docker-practice 폴더로 이동합니다.
$ cd docker-practice

# Dockerfile 이라는 빈 파일을 생성합니다.
$ touch Dockerfile

# 정상적으로 생성되었는지 확인합니다.
$ ls
```

### 2) 기본 명령어

- Dockerfile 에서 사용할 수 있는 기본적인 명령어에 대해서 하나씩 알아보겠습니다.

  - 지금 모든 사용법을 자세히 알아야 할 필요는 없으며, 필요한 경우에 구글링할 수 있을 정도면 충분합니다.

- **FROM**

  - Dockerfile 이 base image 로 어떠한 이미지를 사용할 것인지를 명시하는 명령어입니다.

  ```docker
  FROM <image>[:<tag>] [AS <name>]
  
  # 예시
  FROM ubuntu
  FROM ubuntu:18.04
  FROM nginx:latest AS ngx
  ```

- **COPY**

  - <src> 의 파일 혹은 디렉토리를 <dest> 경로에 복사하는 명령어입니다.

  ```docker
  COPY <src>... <dest>
  
  # 예시
  COPY a.txt /some-directory/b.txt
  COPY my-directory /some-directory-2
  ```

- **RUN**

  - 명시한 커맨드를 도커 컨테이너에서 실행하는 것을 명시하는 명령어입니다.

  ```docker
  RUN <command>
  RUN ["executable-command", "parameter1", "parameter2"]
  
  # 예시
  RUN pip install torch
  RUN pip install -r requirements.txt
  ```

- **CMD**

  - 명시한 커맨드를 도커 컨테이너가 

    시작될 때

    , 실행하는 것을 명시하는 명령어입니다.

    - 비슷한 역할을 하는 명령어로 **ENTRYPOINT** 가 있지만, 아직은 그 차이를 구분하기 어려울 수 있으므로 이번 강의에서는 **ENTRYPOINT** 에 대한 설명은 생략하겠습니다.

  - 하나의 Docker Image 에서는 하나의 **CMD** 만 실행할 수 있다는 점에서 **RUN** 명령어와 다릅니다.

  ```docker
  CMD <command>
  CMD ["executable-command", "parameter1", "parameter2"]
  CMD ["parameter1", "parameter2"] # ENTRYPOINT 와 함께 사용될 때
  
  # 예시
  CMD python main.py
  CMD 
  ```

- **WORKDIR**

  - 이후 작성될 명령어를 컨테이너 내의 어떤 디렉토리에서 수행할 것인지를 명시하는 명령어입니다.
  - 해당 디렉토리가 없다면 생성합니다.

  ```docker
  WORKDIR /path/to/workdir
  
  # 예시
  WORKDIR /home/demo
  ```

- **ENV**

  - 컨테이너 내부에서 지속적으로 사용될 environment variable 의 값을 설정하는 명령어입니다.

  ```docker
  ENV <key> <value>
  ENV <key>=<value>
  
  # 예시
  # default 언어 설정
  RUN locale-gen ko_KR.UTF-8
  ENV LANG ko_KR.UTF-8
  ENV LANGUAGE ko_KR.UTF-8
  ENV LC_ALL ko_KR.UTF-8
  ```

- **EXPOSE**

  - 컨테이너에서 뚫어줄 포트/프로토콜을 지정할 수 있습니다.
  - protocol 을 지정하지 않으면 TCP 가 디폴트로 설정됩니다.

  ```docker
  EXPOSE <port>
  EXPOSE <port>/<protocol>
  
  # 예시
  EXPOSE 8080
  ```

### 3) 간단한 Dockerfile 작성해보기

- `vi Dockerfile` 혹은 vscode 등 본인이 사용하는 편집기로 `Dockerfile` 을 열어 다음과 같이 작성해줍니다.

```docker
# base image 를 ubuntu 18.04 로 설정합니다.
FROM ubuntu:18.04

# apt-get update 명령을 실행합니다.
RUN apt-get update

# DOCKER CONTAINER 가 시작될 때, "Hello FastCampus" 를 출력합니다.
CMD ["echo", "Hello FastCampus"]
```

------

## 2. Docker build from Dockerfile

- `docker build` 명령어로 Dockerfile 로부터 Docker Image 를 만들어봅니다.

```bash
$ docker build --help
# 자세한 옵션들에 대한 설명은 생략

# Dockerfile 이 있는 경로에서 다음 명령을 실행합니다.
$ docker build -t my-image:v1.0.0 .
```

- 설명
  - `.` (**현재 경로**에 있는 Dockerfile 로부터)
  - my-image 라는 **이름**과 v1.0.0 이라는 **태그**로 **이미지**를
  - 빌드하겠다라는 명령어
- 정상적으로 이미지 빌드되었는지 확인해보겠습니다.

```bash
# grep : my-image 가 있는지를 잡아내는 (grep) 하는 명령어
$ docker images | grep my-image
```

- 그럼 이제 방금 빌드한 `my-image:v1.0.0` 이미지로 docker 컨테이너를 **run** 해보겠습니다.

```bash
$ docker run my-image:v1.0.0

# Hello FastCampus 가 출력되는 것 확인
```

------

------

## 3. Docker Image 저장소

### 1) Docker Registry

- 공식 문서

  - https://docs.docker.com/registry/

- 간단하게 도커 레지스트리를 직접 띄워본 뒤에, 방금 빌드한 my-image:v1.0.0 을 도커 레지스트리에 push 해보겠습니다.

- Docker Registry 는 이미 잘 준비된 도커 컨테이너가 존재하므로, 쉽게 사용할 수 있습니다.

- docker registry 를 띄워봅니다.

  ```bash
  $ docker run -d -p 5000:5000 --name registry registry
  
  $ docker ps
  # 정상적으로 registry 이미지가 registry 라는 이름으로 생성되었음을 확인할 수 있습니다.
  # localhost:5000 으로 해당 registry 와 통신할 수 있습니다.
  ```

- my-image 를 방금 생성한 registry 를 바라보도록 tag 합니다.

  ```bash
  $ docker tag my-image:v1.0.0 localhost:5000/my-image:v1.0.0
  
  $ docker images | grep my-image
  # localhost:5000/my-image:v1.0.0 로 새로 생성된 것을 확인할 수 있습니다.
  ```

- my-image 를 registry 에 push 합니다. (업로드합니다.)

  ```bash
  $ docker push localhost:5000/my-image:v1.0.0
  ```

- 정상적으로 push 되었는지 확인합니다.

  ```bash
  # localhost:5000 이라는 registry 에 어떤 이미지가 저장되어 있는지 리스트를 출력하는 명령
  $ curl -X GET <http://localhost:5000/v2/_catalog>
  
  # 출력 : {"repositories":["my-image"]}
  
  # my-image 라는 이미지 네임에 어떤 태그가 저장되어있는지 리스트를 출력하는 명령
  $ curl -X GET <http://localhost:5000/v2/my-image/tags/list>
  
  # 출려 : {"name":"my-image","tags":["v1.0.0"]}
  ```

### 2) Docker Hub

- 회원 가입

  - [hub.docker.com](http://hub.docker.com)

- Choose a Plan

  - Free

  <img width="675" alt="image" src="https://user-images.githubusercontent.com/98302106/187603264-d853749a-5176-4242-8fe0-f98a7c63b4b8.png">

- Email 인증

  <img width="684" alt="image" src="https://user-images.githubusercontent.com/98302106/187603284-abc13ba9-1d1b-441c-ab98-be3003dbc5f5.png">

- Docker login

  ```bash
  $ docker login
  
  # username, password 입력
  # Login Succeeded!
  ```

- Docker Hub 를 바라보도록 tag 생성

  ```bash
  $ docker tag my-image:v1.0.0 fastcampusdemo/my-image:v1.0.0
  
  # docker tag <image_name>:<tag_name> <user_name>/<image_name>:<tag>
  ```

- Docker image push to Docker Hub

  ```bash
  $ docker push fastcampusdemo/my-image:v1.0.0
  
  # docker push <user_name>/<image_name>:<tag>
  ```

- Docker hub 의 본인 계정에서 업로드한 이미지 확인

  - https://hub.docker.com/repositories