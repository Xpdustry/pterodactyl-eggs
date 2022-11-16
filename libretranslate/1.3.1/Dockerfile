FROM        --platform=$TARGETOS/$TARGETARCH python:3.8.14-slim-bullseye as builder

LABEL       author="Phinner" maintainer="62483793+Phinner@users.noreply.github.com"

WORKDIR     /home/container

ARG         DEBIAN_FRONTEND=noninteractive
RUN         apt-get update -qq \
            && apt-get -qqq install --no-install-recommends -y libicu-dev pkg-config gcc g++ wget unzip \
            && apt-get clean \
            && rm -rf /var/lib/apt

RUN         apt-get update && apt-get upgrade --assume-yes

RUN         python -mvenv venv && ./venv/bin/pip install --upgrade pip

RUN         wget https://github.com/LibreTranslate/LibreTranslate/archive/refs/tags/v1.3.1.zip \
            && unzip v1.3.1.zip -d . \
            && mv ./LibreTranslate-1.3.1/** ./ \
            && rm -rf v1.3.1.zip ./LibreTranslate-1.3.1

# Install package from source code
RUN         ./venv/bin/pip install . \
            && ./venv/bin/pip cache purge

FROM        python:3.8.14-slim-bullseye

RUN         addgroup --system --gid 1032 container && adduser --system --uid 1032 container
RUN         apt-get update -qq && apt-get -qqq install --no-install-recommends -y libicu67 && apt-get clean && rm -rf /var/lib/apt
ENV         USER=container HOME=/home/container
USER        container

COPY        --from=builder --chown=1032:1032 /home/container /home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]