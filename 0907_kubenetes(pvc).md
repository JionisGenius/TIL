0907

Kubernetes - service , pvc



# 1. Service 란?

- Service 는 쿠버네티스에 배포한 애플리케이션(Pod)을 외부에서 접근하기 쉽게 추상화한 리소스입니다.
  - https://kubernetes.io/ko/docs/concepts/services-networking/service/
- Pod 은 IP 를 할당받고 생성되지만, 언제든지 죽었다가 다시 살아날 수 있으며, 그 과정에서 IP 는 항상 재할당받기에 고정된 IP 로 원하는 Pod 에 접근할 수는 없습니다.
- 따라서 클러스터 외부 혹은 내부에서 Pod 에 접근할 때는, Pod 의 IP 가 아닌 Service 를 통해서 접근하는 방식을 거칩니다.
- Service 는 고정된 IP 를 가지며, Service 는 하나 혹은 여러 개의 Pod 과 매칭됩니다.
- 따라서 클라이언트가 Service 의 주소로 접근하면, 실제로는 Service 에 매칭된 Pod 에 접속할 수 있게 됩니다.

------



# 2. Service 생성

- 우선 지난 시간에 생성한 Deployment 를 다시 생성합니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

- `kubectl apply -f deployment.yaml`
- 생성된 Pod 의 IP 를 확인하고 접속을 시도해봅니다.

```bash
kubectl get pod -o wide
# Pod 의 IP 를 확인합니다.

curl -X GET <POD-IP> -vvv
ping <POD-IP>
ping 172.17.0.4
# 통신 불가능
```

<img width="900" alt="image" src="https://user-images.githubusercontent.com/98302106/188811325-81a4cde3-7ff2-46b1-a253-c08eeb865c09.png">

- 할당된 <POD-IP> 는 클러스터 내부에서만 접근할 수 있는 IP 이기 때문에 외부에서는 Pod 에 접속할 수 없습니다.
- minikube 내부로 접속하면 통신이 되는지 확인해보겠습니다.

```bash
minikube ssh
# minikube 내부로 접속합니다.

curl -X GET <POD-IP> -vvv #172.17.0.4
ping <POD-IP> #172.17.0.4
# 통신 가능

# 통신중단 ctrl c
```

- 그럼 이제 위의 Deployment 를 매칭시킨 Service 를 생성해보겠습니다.

```yaml
# vi service.yaml 하고 복붙
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  type: NodePort 
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx 


#   type: NodePort # Service 의 Type 을 명시하는 부분입니다.
#  selector: # 아래 label 을 가진 Pod 을 매핑하는 부분입니다.
```

- Service 를 생성합니다.

```bash
vi service.yaml
# 파일을 열어 위의 내용을 복사 붙여넣기 합니다.

kubectl apply -f service.yaml

kubectl get service
# PORT 80:<PORT> 숫자 확인

curl -X GET $(minikube ip):<PORT>
# 이렇게 서비스를 통해서 클러스터 외부에서도 정상적으로 pod 에 접속할 수 있는 것을 확인합니다.
```

<img width="683" alt="image" src="https://user-images.githubusercontent.com/98302106/188819099-778cb82a-7bf5-4153-831c-07bb5fcf7a07.png">

> 



- *** Service 의 Type 이란?**

  - NodePort

     라는 type 을 사용했기 때문에, minikube 라는 kubernetes cluster 내부에 배포된 서비스에 클러스터 외부에서 접근할 수 있었습니다.

    - 접근하는 IP 는 pod 이 떠있는 노드(머신)의 IP 를 사용하고, Port 는 할당받은 Port 를 사용합니다.

  - **LoadBalancer** 라는 type 을 사용해도, 마찬가지로 클러스터 외부에서 접근할 수 있지만, LoadBalancer 를 사용하기 위해서는 LoadBalancing 역할을 하는 모듈이 추가적으로 필요합니다.

  - **ClusterIP** 라는 type 은 고정된 IP, PORT 를 제공하지만, 클러스터 내부에서만 접근할 수 있는 대역의 주소가 할당됩니다.

  - 실무

    에서는 주로 kubernetes cluster 에 MetalLB 와 같은 LoadBalancing 역할을 하는 모듈을 설치한 후, 

    <u>LoadBalancer</u> type 으로 서비스를 expose 하는 방식을 사용합니다.

    - NodePort 는 Pod 이 어떤 Node 에 스케줄링될 지 모르는 상황에서, Pod 이 할당된 후 해당 Node 의 IP 를 알아야 한다는 단점이 존재합니다.

------





# 1. PVC 란?

- Persistent Volume (**PV**), Persistent Volume Claim (**PVC**) 는 stateless 한 Pod 이 **영구적**으로(persistent) **데이터를 보존**하고 싶은 경우 사용하는 리소스입니다.

- 도커에 익숙하신 분이라면 `docker run` 의 `-v` 옵션인 도커 볼륨과 유사한 역할을 한다고 이해할 수 있습니다.

- PV

   는 관리자가 생성한 실제 저장 공간의 정보를 담고 있고, 

  PVC

   는 사용자가 요청한 저장 공간의 스펙에 대한 정보를 담고 있는 리소스입니다.

  - PV 와 PVC 의 차이에 대해서는 헷갈리실 수 있지만, 저희는 지금 당장 이해하지 않아도 괜찮습니다.
  - Pod 내부에서 작성한 데이터는 기본적으로 **언제든지 사라질 수 있기에**, <u>**보존**하고 싶은 데이터가 있다면 Pod</u> <u>에 **PVC 를 mount 해서** 사용</u>해야 한다는 것만 기억하시면 됩니다.

- PVC 를 사용하면 여러 pod 간의 data 공유도 쉽게 가능합니다. 

------



# 2. PVC 생성

- minikube 를 생성하면, 기본적으로 minikube 와 함께 설치되는 storageclass 가 존재합니다.
  - `kubectl get storageclass`를 통해 이미 설치된 storageclass 를 확인할 수 있습니다.
  - storageclass 에 대한 자세한 설명은 생략하겠습니다.
    - PVC 를 생성하면 해당 PVC 의 스펙에 맞는 PV 를 그 즉시 자동으로 생성해준 뒤, PVC 와 매칭시켜준다고만 이해하시면 됩니다. (dynamic provisioning 지원하는 storageclass)
- PVC 를 생성합니다.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec: # pvc 의 정보를 입력하는 파트입니다.
  accessModes:
    - ReadWriteMany # ReadWriteOnce, ReadWriteMany 옵션을 선택할 수 있습니다.
  volumeMode: Filesystem
  resources:
    requests:
      storage: 10Mi # storage 용량을 설정합니다.
  storageClassName: standard # 방금 전에 확인한 storageclass 의 name 을 입력합니다.
vi pvc.yaml

kubectl apply -f pvc.yaml

kubectl get pvc,pv
# pvc 와 동시에 pv 까지 방금 함께 생성된 것을 확인할 수 있습니다.
```

<img width="1116" alt="image" src="https://user-images.githubusercontent.com/98302106/188829695-9e2293f7-e4d0-441e-aa9c-fd3f0812191a.png">

--------------



# 3. Pod 에서 PVC 사용

- Pod 을 생성합니다.
  - volumeMounts, volumes 부분이 추가되었습니다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html" # mount 할 pvc 를 mount 할 pod 의 경로를 적습니다.
        name: mypd # 어떤 이름이든 상관없으나, 아래 volumes[0].name 과 일치해야 합니다.
  volumes:
    - name: mypd # 어떤 이름이든 상관없으나, 위의 volumeMounts[0].name 과 일치해야 합니다.
      persistentVolumeClaim:
        claimName: myclaim # mount 할 pvc 의 name 을 적습니다.


vi pod-pvc.yaml

kubectl apply -f pod-pvc.yaml
```

- pod 에 접속하여 mount 한 경로와 그 외의 경로에 파일을 생성합니다.

```bash
kubectl exec -it mypod -- bash
#mypod 접속

touch hi-jioni
# mypod에서 hi-jioni생성

cd /var/www/html 


touch hi-jioni
# mount경로에 파일 생성
```

<img width="1126" alt="image" src="https://user-images.githubusercontent.com/98302106/188831034-863b8c3a-83d8-4bb4-82df-61bf4d573180.png">

- pod 을 삭제합니다.

```bash
kubectl delete pod mypod
```

- pvc 는 그대로 남아있는지 확인합니다.

```bash
kubectl get pvc,pv
```

- 해당 pvc 를 mount 하는 pod 을 다시 생성합니다.

```bash
kubectl apply -f pod-pvc.yaml
```

- pod 에 접속하여 아까 작성한 파일들이 그대로 있는지 확인합니다.

```bash
kubectl exec -it mypod -- bash

ls
# hi-fast-campus 파일이 사라진 것을 확인할 수 있습니다.

cd /var/www/html
# mountPath 

ls
# hi-fast-campus 파일이 그대로 보존되는 것을 확인할 수 있습니다.
```

------

> kubernetes [공식 문서](https://kubernetes.io/ko/docs/home/) 혹은 해당 페이지에서 제공하는 [interactive tutorial](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/) 을 따라해보시면 많은 도움이 될 것입니다.

<img width="1113" alt="image" src="https://user-images.githubusercontent.com/98302106/188844038-3d922033-d7e1-470f-9aee-bc2d3d07b824.png">