FROM        --platform=$TARGETOS/$TARGETARCH alpine:3.24

LABEL       author="Silly Development" maintainer="gamer@sillydev.co.uk"

RUN         apk add --update --no-cache \
                bash \
                ca-certificates \
                curl \
                iproute2 \
                tini \
                tzdata

# download the latest Pumpkin (nightly) binary for the target architecture
ARG         TARGETARCH
ARG         PUMPKIN_TAG=nightly
ARG         PUMPKIN_VERSION=unknown
LABEL       org.opencontainers.image.version=$PUMPKIN_VERSION
RUN         case "${TARGETARCH}" in \
                "amd64") BIN_ARCH="X64" ;; \
                "arm64") BIN_ARCH="ARM64" ;; \
                *) echo "Unsupported architecture: ${TARGETARCH}" && exit 1 ;; \
            esac \
            && curl -fsSL "https://github.com/Pumpkin-MC/Pumpkin/releases/download/${PUMPKIN_TAG}/pumpkin-${BIN_ARCH}-Linux-musl" -o /usr/local/bin/pumpkin \
            && chmod +x /usr/local/bin/pumpkin \
            && ln -sf /usr/local/bin/pumpkin /usr/bin/pumpkin

# add container user and set stop signal
RUN         adduser -D -h /home/container -s /bin/bash container
STOPSIGNAL  SIGINT

USER        container
ENV         USER=container HOME=/home/container RUST_BACKTRACE=1
WORKDIR     /home/container

EXPOSE      25565/tcp 19132/udp

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
    CMD nc -z 127.0.0.1 25565 || exit 1

COPY        --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/sbin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
