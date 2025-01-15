# Kubernetes ReplicaSets

## Replication Controller vs ReplicaSet

### Replication Controller
- 기본적인 파드 복제 컨트롤러
- 단순한 라벨 선택자만 지원 (equality-based)
- 동일한 라벨을 가진 파드만 관리 가능

### ReplicaSet
- Replication Controller의 새로운 버전
- 더 풍부한 라벨 선택자 지원 (set-based)
- `matchExpressions`를 통한 복잡한 라벨 조건 설정 가능
- 예: `In`, `NotIn`, `Exists`, `DoesNotExist` 연산자 지원

### ReplicaSet의 replicas 개수 변경 명령어
```bash
kubectl scale rs nginx --replicas=6
```

## 주의사항
- ReplicaSet은 `apps/v1` API 버전을 사용
- Replication Controller는 `v1` API 버전을 사용
- ReplicaSet의 `matchExpressions`를 사용할 때는 연산자와 값의 조합이 올바른지 확인 필요
- 파드 템플릿의 라벨은 반드시 selector와 일치해야 함

## 참고
- Kubernetes 공식 문서
- 실제 운영 환경에서는 Deployment를 사용하는 것을 권장