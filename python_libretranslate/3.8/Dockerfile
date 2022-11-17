FROM        --platform=$TARGETOS/$TARGETARCH python:3.8-slim-bullseye

LABEL       author="Phinner" maintainer="62483793+Phinner@users.noreply.github.com"

RUN         apt update \
            && apt -y install git gcc g++ ca-certificates dnsutils curl iproute2 ffmpeg procps libicu-dev python3-icu pkg-config libxslt1-dev libxml2-dev \
            && useradd -m -d /home/container container

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
