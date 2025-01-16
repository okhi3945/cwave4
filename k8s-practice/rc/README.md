## Replication Controller (RC) 개요

Replication Controller는 지정된 수의 Pod 복제본이 항상 실행되도록 보장하는 Kubernetes의 객체이다.

### 주요 특징
- Pod의 개수를 지정된 수로 유지(Replicas)
- Pod 장애 발생 시 원래는 다른 노드에서 다시 재생성되지만, RC가 설정된 Pod는 자동으로 복구됨
- Selector와 Replicas를 통해 Pod가 지정된 개수로 잘 떠있는지 감시
- **Label**에 해당하는 Pod가 Replicas 개수만큼 잘 떠있는지 감시하고, 종료되면 살리고, Pod의 라벨이 변경되면, 새로운 Pod를 띄운다. 

### 라벨 관련 명령어
- 라벨 추가 : kubectl label pod goapp-pod2 app="application" tier="frontEnd”
- 전체 라벨 확인 : kubectl get po --show-labels
- 라벨 삭제 : kubectl label pod <pod-name> <label-name>- 뒤에 - 붙여주면 삭제

### Pod 수정
```bash
# Pod 이미지 수정
kubectl edit po goapp-pod

# Pod 라벨 수정
kubectl label pod <pod-name> app=goapp-exit --overwrite
```

### 주의사항
- Pod 이미지 변경은 재시작 시에만 적용됨
- RC 관리 하의 Pod 라벨 변경 시 새로운 Pod 생성됨

## 클러스터 컨텍스트 관리

### kubectx 사용법
```bash

# 컨텍스트 이름 변경
kubectx <new-name>=<name>
```
