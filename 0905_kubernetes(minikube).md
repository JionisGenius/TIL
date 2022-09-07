0905

Kubernetes minikube



# 1. Prerequisite

- References
  - minikube
    - https://minikube.sigs.k8s.io/docs/start/
  - kubectl
    - https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/
- 최소 사양
  - CPU : 2 이상
    - 원활한 실습을 위해서는 6 이상을 추천
  - Memory : 2GB 이상
    - 원활한 실습을 위해서는 12 GB 이상을 추천
  - Disk : 20 GB 이상
    - 원활한 실습을 위해서는 100 GB 이상을 추천
  - 가상화 tool : **Docker**, Hyperkit, Hyper-V, ...

### VM 스펙 업그레이드 필요

- CPU : multicore

  - VM 생성 이후 **demo 용 머신 우클릭 → 설정 → 시스템 → 프로세서 → cpu 3개 이상으로 변경**

- Disk: 40 GB 이상

  - **VM 생성 단계**에서 Disk 크기 조절 후 **재생성** 필요

------

# 2. Let's Install Minikube

- minikube 의 최신 버전 (v1.22.0) 바이너리를 다운받고, 실행할 수 있도록 변경합니다.
  - 이하의 모든 커맨드는 amd 기반의 CPU 를 기준으로 합니다.
  - arm 기반의 CPU 는 공식 문서를 확인해주시기 바랍니다.

```bash
curl -LO <https://storage.googleapis.com/minikube/releases/v1.22.0/minikube-linux-amd64>

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

- 정상 다운로드 확인

```bash
minikube --help
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.

  ```bash
  minikube는 개발 워크플로우에 최적화된 로컬 쿠버네티스를 제공하고 관리합니다.
  
  Basic Commands:
    start          로컬 쿠버네티스 클러스터를 시작합니다
    status         로컬 쿠버네티스 클러스터의 상태를 가져옵니다
    stop           실행 중인 로컬 쿠버네티스 클러스터를 중지합니다
    delete         로컬 쿠버네티스 클러스터를 삭제합니다
  ```

- minikube version 을 확인합니다.

  ```bash
  minikube version
  ```

------

# 3. Let's Install Kubectl

- kubectl 은 kubernetes cluster (server) 에 요청을 간편하게 보내기 위해서 널리 사용되는 client 툴입니다.
- kubectl 은 v1.22.1 로 다운로드 받겠습니다.

```bash
curl -LO <https://dl.k8s.io/release/v1.22.1/bin/linux/amd64/kubectl>
```

- kubectl 바이너리를 사용할 수 있도록 권한과 위치를 변경합니다.

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

- 정상적으로 설치되었는지 확인합니다.

```bash
kubectl --help
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.

  ```bash
  kubectl controls the Kubernetes cluster manager.
  
   Find more information at:
  <https://kubernetes.io/docs/reference/kubectl/overview/>
  
  Basic Commands (Beginner):
    create        Create a resource from a file or from stdin
    expose        Take a replication controller, service, deployment or pod and
  expose it as a new Kubernetes service
    run           Run a particular image on the cluster
    set           Set specific features on objects
  ```

- kubectl version 을 확인합니다.

```bash
kubectl version
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.

  ```bash
  Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.1", GitCommit:"632ed300f2c34f6d6d15ca4cef3d3c7073412212", GitTreeState:"clean", BuildDate:"2021-08-19T15:45:37Z", GoVersion:"go1.16.7", Compiler:"gc", Platform:"linux/amd64"}
  The connection to the server localhost:8080 was refused - did you specify the right host or port?
  ```

  - `The connection to the server localhost:8080 was refused - did you specify the right host or port?` 메시지는 에러를 의미하는 것이 맞습니다.
  - 하지만 `kubectl version` 은 client 의 버전과 kubernetes server 의 버전을 모두 출력하는 명령어이며, 현재 저희는 kubernetes server 를 생성하지 않았기 때문에 client 의 버전만 정상적으로 출력됩니다.

------

# 4. Minikube 시작하기

### minikube start

- minikube 를 docker driver 를 기반으로 하여 시작합니다.

  ```bash
  minikube start --driver=docker
  ```

- 다음과 같은 화면이 출력되며, 필요한 docker image 들을 다운받게 되고, 다운로드가 완료되면 이를 기반으로 minikube 를 구동합니다.

  <img width="879" alt="image" src="https://user-images.githubusercontent.com/98302106/188769557-3142d01c-2308-45a8-abd3-5cd5664e7e26.png">

- 정상적으로 `minikube start` 가 완료되면 다음과 같은 메시지가 출력됩니다.

  

### minikube status

- 정상적으로 생성되었는지 minikube 의 상태를 확인해봅니다.

```bash
minikube status
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### kubectl get pod -n kube-system

- kubectl 을 사용하여 minikube 내부의 default pod 들이 정상적으로 생성되었는지 확인해봅니다.

```bash
kubectl get pod -n kube-system
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
NAME                               READY   STATUS    RESTARTS   AGE
coredns-558bd4d5db-bwkjv           1/1     Running   0          3m40s
etcd-minikube                      1/1     Running   0          3m46s
kube-apiserver-minikube            1/1     Running   0          3m46s
kube-controller-manager-minikube   1/1     Running   0          3m53s
kube-proxy-ppgbx                   1/1     Running   0          3m40s
kube-scheduler-minikube            1/1     Running   0          3m46s
storage-provisioner                1/1     Running   1          3m51s
```

------

# 5. Minikube 삭제하기

### minikube delete

- 다음 명령어로 간단하게 삭제할 수 있습니다.

```bash
minikube delete
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
🔥  docker 의 "minikube" 를 삭제하는 중 ...
🔥  Deleting container "minikube" ...
🔥  /home/kjy/.minikube/machines/minikube 제거 중 ...
💀  "minikube" 클러스터 관련 정보가 모두 삭제되었습니다
```

------



