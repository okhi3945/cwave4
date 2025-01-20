# 실습용 Ubuntu Docker 환경

- 실습용 ubuntu 이미지 제작하기 코드베이스입니다.
- 도커를 우분투 컨테이너 안에 설치하는 셸 스크립트와 도커파일을 포함합니다.

## 이미지 구성

- 베이스 이미지: `ubuntu:22.04`
- Docker Engine 사전 설치
- 필수 시스템 패키지 포함

## 프로젝트 구조

```
.
├── Dockerfile
├── install_docker_engine.sh
└── README.md
```

### install_docker_engine.sh
Docker 엔진 설치를 위한 스크립트로, 다음 작업을 수행합니다:
- 시스템 패키지 업데이트
- Docker 저장소 설정
- GPG 키 설정
- Docker 엔진 설치

## 이미지 빌드 방법

```bash
# 이미지 빌드
docker build -t cloudwave:base.v1 . # 를 실행하여 이미지 생성
# -t : 도커 이미지의 이름과 태그를 설정

# 컨테이너 실행
docker run -it cloudwave:base.v1
```

## 주요 설치 과정

1. 시스템 업데이트 및 기본 패키지 설치
```bash
apt-get update && apt-get upgrade
apt-get install -y ca-certificates curl gnupg
```

2. Docker 저장소 설정
```bash
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
```

3. Docker 저장소 추가
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4. Docker 엔진 설치
```bash
apt-get update && apt-get install -y docker-ce docker-ce-cli
```