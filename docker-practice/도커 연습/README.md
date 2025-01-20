# Go 웹 서버 Docker ARG 실습

- Go 웹 서버를 Docker ARG를 활용하여 다양한 베이스 이미지로 빌드하는 방법을 설명합니다.

## ARG 사용 설명

### Dockerfile에서의 ARG 범위

```dockerfile
# Global scope ARG - FROM 지시문에서만 사용 가능
ARG OS
FROM golang:${OS} AS build

# 빌드 스테이지 내 ARG 재선언
ARG OS
ENV BASE=${OS}

WORKDIR /app
COPY src ./
RUN CGO_ENABLED=0 go build -o main

FROM scratch AS release
COPY --from=build /app/main /app/
WORKDIR /app
CMD ["/app/main"]
```

### ARG의 특징
1. **범위(Scope)**
   - Global scope: FROM 지시문 이전에 선언된 ARG는 FROM 지시문에서만 사용 가능
   - Build stage scope: 각 빌드 스테이지에서 ARG를 재선언해야 사용 가능

2. **ENV와의 차이점**
   - ARG: 빌드 시에만 사용되는 임시 변수
   - ENV: 빌드 및 컨테이너 실행 시에도 유지되는 환경 변수

## 빌드 방법

다양한 베이스 이미지로 빌드하는 예시:

```bash
# Ubuntu Bookworm 기반 빌드
docker build --build-arg OS=bookworm -t go:ubuntu .

# Alpine 기반 빌드
docker build --build-arg OS=alpine -t go:alpine .

# Debian 기반 빌드
docker build --build-arg OS=bullseye -t go:debian .
```

## 프로젝트 구조

```
.
├── src/
│   ├── go.mod
│   └── main.go
└── Dockerfile
```

## ARG 활용 팁

1. **기본값 설정**
   ```dockerfile
   ARG OS=alpine  # 기본값을 alpine으로 설정
   ```

2. **여러 ARG 조합**
   ```dockerfile
   ARG OS
   ARG GO_VERSION
   FROM golang:${GO_VERSION}-${OS}
   ```

3. **조건부 빌드**
   ```bash
   # 개발 환경
   docker build --build-arg OS=bookworm --build-arg ENV=dev -t go:dev .
   
   # 프로덕션 환경
   docker build --build-arg OS=alpine --build-arg ENV=prod -t go:prod .
   ```

## 실행 방법

```bash
# 기본 실행
docker run -d -p 80:80 go:ubuntu

# 포트 매핑 변경
docker run -d -p 8080:80 go:alpine
```