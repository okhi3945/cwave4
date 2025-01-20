FROM ubuntu:22.04
RUN mkdir -p /scripts
COPY install_docker_engine.sh /scripts
WORKDIR /scripts
RUN chmod +x install_docker_engine.sh
RUN ./install_docker_engine.sh
RUN apt-get install -y docker-ce docker-ce-cli

# 코드 설명
# FROM ubuntu:22.04 -> 우분투 22.04버전을 베이스 이미지로 사용
# RUN mkdir -p /scripts -> 루트 디렉토리에 scripts 디렉토리 생성
# COPY install_docker_engine.sh /scripts -> 만든 scripts 디렉토리에 파일 복사
# WORKDIR /scripts -> 워킹 디렉토리를 /scripts로 설정
# RUN chmod +x install_docker_engine.sh -> 실행 권한 추가
# RUN ./install_docker_engine.sh -> 파일 실행
# RUN apt-get install -y docker-ce docker-ce-cli -> docker-ce, docker-ce-cli 설치
