# 쿠버네티스의 라벨과 어노테이션

## 개요
- 라벨과 어노테이션은 모두 주석의 형태로, 객체를 삭제하거나 조회하는 등의 운영 작업 시 대상을 식별하는 데 사용함.
- 라벨은 다차원적이며 여러 개를 지정할 수 있음.

# 라벨 명령어
bash 라벨 추가
```bash
kubectl label pod goapp-pod2 app="application" tier="frontEnd"
```

# 전체 라벨 확인
kubectl get po --show-labels

# 특정 라벨만 확인
kubectl get po -L env,app

# 라벨 삭제 (끝에 '-' 추가)
kubectl label pod <pod-name> <label-name>-

# describe로 라벨 확인
kubectl describe po goapp-pod2

# 라벨 조건에 맞는 pod 정보 출력 (AND 조건)
kubectl get po -l 'app in (application), tier in (frontEnd)'