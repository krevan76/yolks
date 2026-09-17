FROM        --platform=$TARGETOS/$TARGETARCH ghcr.io/graalvm/graalvm-community:22-ol9

LABEL       author="Silly Development" maintainer="gamer@sillydev.co.uk"

ARG         TARGETARCH

RUN         microdnf install -y \
                curl \
                lsof \
                shadow-utils \
                ca-certificates \
                openssl \
                git \
                tar \
                gzip \
                zip \
                unzip \
                sqlite \
                fontconfig \
                freetype \
                tzdata \
                iproute \
                which \
                findutils \
                procps-ng \
            && microdnf clean all

# jq and tini (static builds, not available in the Oracle Linux base repos)
RUN         curl -fsSL "https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-linux-${TARGETARCH}" -o /usr/bin/jq \
            && chmod +x /usr/bin/jq \
            && curl -fsSL "https://github.com/krallin/tini/releases/download/v0.19.0/tini-static-${TARGETARCH}" -o /usr/bin/tini \
            && chmod +x /usr/bin/tini

RUN rm -rf /usr/bin/dd \
 && rm -rf /usr/bin/fallocate \
 && rm -rf /usr/bin/truncate \
 && rm -rf /usr/bin/xfs_mkfile

## Setup user and working directory
RUN         useradd -m -d /home/container -s /bin/bash container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
