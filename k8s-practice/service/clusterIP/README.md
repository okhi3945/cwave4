# Kubernetes Services

- 쿠버네티스의 Service는 Pod에 대한 단일 진입점을 제공하는 네트워크 로드 밸런서
- Pod에 장애가 발생하여 다른 노드로 이동하더라도 Service를 통해 안정적으로 접근할 수 있도록 도와줌
- 서비스는 쿠버네티스 etcd에 존재하는 객체 → kube-proxy가 서비스가 어디에 떠있는지 정보를 수집(iptable, ipvs에 정보를 업데이트)
-  linux에 있는 iptable, ipvs 서비스(방화벽 sw)에 설정값으로 들어가있음
- 노드별로 격리된 os를 가짐 → pod를 받치고 있는 iptable

## Service의 주요 특징
- 네트워크 로드 밸런싱 제공
- 요청 분배를 통한 **부하 분산**
- 동적 라우팅 지원
- 향상된 장애 내성

## Service 타입

### 1. ClusterIP
- 기본 Service 타입
- 클러스터 내부에서만 접근 가능
- kube-proxy 기반으로 동작
- iptables/ipvs를 통한 라우팅 설정

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP  # 생략 가능 (기본값)
  selector:
    app: my-app # 트래픽을 요청하면 라벨로 지정된 값이 일치하는 pod에 트래픽 보냄
  ports:
  - port: 80 # 80번 포트로 서비스 요청을 받고, 
    targetPort: 8080 # 각 서비스의 8080포트로 서비스해줌
```

### 2. NodePort
- 클러스터 외부에서 노드의 IP:Port로 접근 가능
- 자동으로 ClusterIP 서비스도 생성
- 포트 범위: 30000-32767

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30123  # 미지정시 자동 할당
```

### 3. LoadBalancer
- 클라우드 공급자의 로드 밸런서와 연동
- 외부(인터넷)에서 접근 가능
- NodePort와 ClusterIP 서비스도 자동 생성

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer
  annotations:  # 클라우드 공급자별 설정
    service.beta.kubernetes.io/aws-load-balancer-type: external
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-scheme: internet-facing
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
```

### 4. Ingress
- Layer 7 로드 밸런싱 제공
- URL 기반 라우팅
- 여러 서비스에 대한 단일 진입점
- SSL/TLS 종료 지원

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: "alb-ingress-class"
  rules:
  - host: app1.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
```

### 5. Headless Service
- 로드 밸런싱 없이 DNS 기반 서비스 디스커버리
- Pod IP 직접 반환
- StatefulSet과 함께 사용할 때 유용

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-headless
spec:
  clusterIP: None  # Headless 서비스 지정
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
```

## 서비스 선택 가이드
- **ClusterIP**: 내부 백엔드 서비스용
- **NodePort**: 개발/테스트 환경, 임시 외부 접근용
- **LoadBalancer**: 프로덕션 환경의 외부 서비스용
- **Ingress**: 여러 서비스를 단일 IP로 제공해야 할 때
- **Headless**: StatefulSet 워크로드, DNS 기반 서비스 디스커버리 필요시