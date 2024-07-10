ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base/amd64:10.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# hadolint ignore=DL3008
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install wget -y && \
    wget --progress=dot:giga https://dl.min.io/server/minio/release/linux-amd64/minio_20240704142545.0.0_amd64.deb && \
    dpkg -i minio_20240704142545.0.0_amd64.deb && \
    groupadd -r minio-user && \
    useradd -M -r -g minio-user minio-user && \
    mkdir /data && \
    chown minio-user:minio-user /data

ENV MINIO_ROOT_USER=admin
ENV MINIO_ROOT_PASSWORD=password

EXPOSE 9001
EXPOSE 9000

ENTRYPOINT [ "minio", "server", "/data", "--console-address=:9001" ]

LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Rick Moonen <rick@moonen.network>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Moonen Home Assistant Services" \
    org.opencontainers.image.authors="Rick Moonen <rick@moonen.network>" \
    org.opencontainers.image.licenses="Apache2" \
    org.opencontainers.image.url="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}