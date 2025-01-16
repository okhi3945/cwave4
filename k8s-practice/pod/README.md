# 쿠버네티스 Pod 리소스
- Pod는 쿠버네티스에서 컨테이너를 관리하는 가장 기본적인 배포 단위입니다.

## Pod 기본 명령어
``` bash
# Pod 상태 실시간 모니터링
kubectl get po -w
```

# Pod 상세 정보 확인 (IP, Node 정보 등)
```bash
kubectl get po -o wide
```

# Pod 포트포워딩 (로컬:Pod)
```bash
kubectl port-forward goapp-pod 8082:8080
```
# Pod 리소스 정의 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: goapp-pod    # Pod의 이름
  
spec:    # Pod 리소스의 스펙
  containers:    # 하나 이상의 컨테이너 설정 가능
  - image: dangtong/goapp
    name: goapp-container    # 컨테이너 이름
    ports:
    - containerPort: 8080    # 컨테이너 포트
      protocol: TCP
```

# 주요 네트워크 구성
쿠버네티스는 다음과 같은 3가지 네트워크 계층을 가짐:

- Host/Node 네트워크
- Pod 네트워크
- Service 네트워크

- 위 예시에서 **containerPort**는 Pod 네트워크 상에서의 포트를 정의합니다.

# 리소스 생성 및 배포
```bash
# YAML 파일로 Pod 생성
kubectl apply -f pod.yaml

# Pod 상태 확인
kubectl get pods

# Pod 상세 정보 확인
kubectl describe pod goapp-pod
```