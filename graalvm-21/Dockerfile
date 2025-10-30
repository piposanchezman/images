# ----------------------------------
# QuickLand Network Custom Docker Image
# Image: ghcr.io/piposanchezman/images:graalvm-21
# ----------------------------------

FROM debian:bookworm-slim

LABEL author="DevOps Team at QuickLand Network" maintainer="devops@quickland.net"

# Instalar dependencias necesarias y limpiar caché
RUN apt-get update -y \
 && apt-get install -y --no-install-recommends \
      fontconfig iproute2 curl ca-certificates unzip tar jq zlib1g-dev build-essential wget \
 && useradd -d /home/container -m container \
 && mkdir -p /opt/java \
 && rm -rf /var/lib/apt/lists/*

# Descargar y configurar GraalVM JDK 21 desde Oracle
RUN cd /home/container \
 && wget https://download.oracle.com/graalvm/21/latest/graalvm-jdk-21_linux-x64_bin.tar.gz -O graalvm.tar.gz \
 && tar -xzf graalvm.tar.gz \
 && mv graalvm-jdk-21* /opt/java/graalvm \
 && rm graalvm.tar.gz

# Variables de entorno y usuario no root
USER container
ENV USER=container \
    HOME=/home/container \
    JAVA_HOME=/opt/java/graalvm \
    PATH="/opt/java/graalvm/bin:$PATH" \
    LANG=C.UTF-8

WORKDIR /home/container

# Copiar entrypoint
COPY ./entrypoint.sh /entrypoint.sh

# Definir entrypoint (siempre ejecuta el script de arranque)
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
