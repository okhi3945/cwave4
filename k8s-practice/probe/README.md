# Kubernetes Probes

## 개요
쿠버네티스에서는 파드의 상태를 모니터링하고 관리하기 위해 세 가지 종류의 probe를 제공함:
- Startup Probe
- Readiness Probe
- Liveness Probe

## 추가적인 내용
probe는 pod의 일종, probe 객체가 따로 있음

## Probe 종류별 특징

### Startup Probe
- **목적**: 어플리케이션이 컨테이너 내에서 **정상적으로 시작**되었는지 확인
- **동작 시점**: 컨테이너 초기화 시
- **특징**:
  - Startup probe가 성공적으로 완료되어야 다른 probe들이 실행됨
  - 실패 시 컨테이너 재시작
  - 각 서비스의 startup 시간이 다른 상황에 대응 가능
  - startup probe가 정상적으로 종료되면, pod가 정상적으로 실행이 된것임, 컨테이너를 초기화할 때 작동함


### Readiness Probe
- **목적**: 어플리케이션이 **트래픽을 처리**할 수 있는 상태인지 확인
- **동작**: 문제 감지 시 **트래픽 차단**
- **사용 사례**: 사용자가 준비되지 않은 노드에 접속하는 것을 방지

### Liveness Probe
- **목적**: 어플리케이션의 건강 상태 확인, 파드가 떴을 때 파드에 문제가 생기는 것을 감지
- **감지 대상**: 
  - 데드락
  - 메모리 leak
  - 무한루프
- **동작**: 문제 감지 시 컨테이너 재시작 및 트래픽 차단

## 주의사항
  - 서비스들의 startup 시간이 다름
  - 사용자가 안뜬 노드에 접속 → 트래픽 차단해줘야함
  - startup probe 통과시 다른 probe 실행하는 형식으로 프로그램을 구성
  - startup에서 계속 실패시 재가동(최대 시도 횟수 정함)

## Probe 공통 설정

| 설정 항목 | 설명 |
|-----------|------|
| `initialDelaySeconds` | probe 시작 전 대기 시간 |
| `periodSeconds` | probe 수행 간격 |
| `timeoutSeconds` | probe 최대 응답 대기 시간 |
| `successThreshold` | 성공으로 간주되기 위한 연속 성공 횟수 |
| `failureThreshold` | 재시작을 위한 연속 실패 횟수 |
