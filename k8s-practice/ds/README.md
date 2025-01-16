# Kubernetes DaemonSet

## 개요
- DaemonSet은 쿠버네티스의 워크로드 리소스로, 클러스터의 모든 노드(또는 특정 노드)에 Pod를 하나씩 실행하도록 보장
- 주로 클러스터의 **인프라적인 요소**를 관리하는 데 사용

## 특징
- 각 노드당 하나의 Pod만 실행되도록 보장
- DaemonSet에 의해 생성된 Pod는 해당 노드가 살아있는 동안만 존재 (노드와 생명주기 공유)
  - 노드가 종료되면 해당 노드의 DaemonSet Pod도 함께 종료
  - 노드가 다시 시작되면 DaemonSet Pod도 자동으로 다시 생성
- nodeSelector를 통해 특정 노드에만 Pod가 생성되도록 제어 가능

## 주요 사용 사례
DaemonSet은 다음과 같은 인프라 구성요소에 주로 사용됩니다:

1. **kindnet**
    - CNI(Container Network Interface) 구현체
    - Docker 기반의 Kind 네트워크를 쿠버네티스가 사용할 수 있도록 지원
    - 각 벤더별로 다양한 CNI 구현체 존재
    - k8s 설치를 위한 kind를 설치 후 `kind create cluster  --config ./3-node-cluster.yaml` 명령어를 실행하면, kind network가 생성됨

2. **kube-proxy**
    - 쿠버네티스 서비스 네트워킹 담당
    - 각 노드에서 서비스 규칙을 관리
    - node가 많은데 pod가 다 흩어져있음, 서로 다른 노드에 있더라도 pod들이 같은 네트워크에 있는 것처럼 만들어줌

### 주요 설정 설명
- `nodeSelector`: 특정 라벨을 가진 노드에만 Pod를 배포
- `selector`: DaemonSet이 관리할 Pod를 선택하는 기준
- `template`: 생성할 Pod의 명세

## 노드 라벨 관리

### 라벨 추가
```bash
kubectl label node [노드이름] disk=ssd
```

### 라벨 제거
```bash
kubectl label node [노드이름] disk-
```
- pod의 라벨 제거시 데몬셋도 삭제됨

## Kind 클러스터에서의 네트워크 관리

### 네트워크 구성
- Kind 클러스터는 자체 네트워크를 생성
- Docker의 Software Defined Network 활용
- IDE와 클러스터가 동일한 Kind 네트워크 사용
- k8s 설치를 위한 kind를 설치 후 `kind create cluster  --config ./3-node-cluster.yaml` 명령어를 실행하면, kind network가 생성됨
- kind network는 네트워크의 종류 중 host,node network 담당힘(pod network, service network X)

### 트러블슈팅
시스템 재부팅 후 kubectl 연결 실패 시:
1. `docker inspect kind` 명령으로 control-plane IP 확인
2. `.kube/config` 파일의 API 서버 주소를 새로운 IP로 업데이트