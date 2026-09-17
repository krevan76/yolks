FROM        --platform=$TARGETOS/$TARGETARCH ruby:3.2-slim-bookworm

LABEL       author="Silly Development" maintainer="gamer@sillydev.co.uk"

RUN         apt-get update \
            && apt-get -y install --no-install-recommends \
                build-essential \
                ca-certificates \
                curl \
                git \
                iproute2 \
                iputils-ping \
                jq \
                libsqlite3-dev \
                procps \
                sqlite3 \
                tar \
                tini \
                tzdata \
                unzip \
                zip \
                zlib1g-dev \
            && rm -rf /var/lib/apt/lists/*

# add container user and set stop signal
RUN         useradd -m -d /home/container container
STOPSIGNAL  SIGINT

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
