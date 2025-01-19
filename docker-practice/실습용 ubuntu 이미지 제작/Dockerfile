FROM ubuntu:22.04
RUN mkdir -p /scripts
COPY install_docker_engine.sh /scripts
WORKDIR /scripts
RUN chmod +x install_docker_engine.sh
RUN ./install_docker_engine.sh
RUN apt-get install -y docker-ce docker-ce-cli