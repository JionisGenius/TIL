0829

kubernetes 개념

도커로 생성한 컨테이너를 관리해주는 컨테이너 오케스트레이션 오픈소스인 쿠버네티스

아마존 EKS =  google kubernetes service = azure AKS 



쿠버네티스의 컨셉 -선언형 인터페이스와 Desired State (원하는 상태)

명령형 인터페이스 : A 를 이렇게 저렇게 해서 하나 만들어줘 > 에어컨의 냉매는 어떤걸 쓰고 얼마나 압축한 다음 어떻게 순환시켜서...~

**선언형 인터페이스(desired state)** : A 가 하나 있었으면 좋겠어 > 내방 온도가 20도가 되었으면 좋겠어

> 최종결과만 선언 하는방식 > 쿠버네티스는 선언형 인터페이스 방식을 사용한다.



### <master node & worker node>

<img width="1077" alt="image" src="https://user-images.githubusercontent.com/98302106/187228726-d016f26c-c972-40a5-b923-cdbce778afe6.png"> 

> 사용자는 한번 가상화 시키는것 즉, hw 에 sw화 되어서,  사용자는 하나의 컴퓨터로 여러서버를 사용할수있다.



**Control plane node** : 여러개의 워커노드들을 관리 하고 모니터링  클라이언트 요청 전달 /스케줄링 
**API server**:  클라이언트가 보내는 요청을 받는 component
**etcd** :사용자가 보내는 요청의 desired state 를  key-value형식으로 저장 하는 컴포넌트
**Kubelet**: control plane이 정확히는 그 내부에 cude api server가 전달해주는 요청을 워커노드에서는 실제로 수행을 해야한다. 그 과정에서 명령을 받고 자신의 워커노드의 현재 상태를 다시 control plane 에게(api server) 전달하는 역할을 하는 컴포넌트

