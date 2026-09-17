FROM        --platform=$TARGETOS/$TARGETARCH php:8.1-cli

LABEL       author="Silly Development" maintainer="gamer@sillydev.co.uk"

RUN         apt-get update \
            && apt-get -y install --no-install-recommends \
                ca-certificates \
                curl \
                git \
                iproute2 \
                iputils-ping \
                jq \
                procps \
                sqlite3 \
                libsqlite3-dev \
                tar \
                tini \
                tzdata \
                unzip \
                zip \
            && docker-php-ext-install pdo_sqlite \
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
