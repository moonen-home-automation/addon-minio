ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base/amd64:10.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# hadolint ignore=DL3008
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install wget -y && \
    wget --progress=dot:giga https://dl.min.io/server/minio/release/linux-amd64/minio_20240710184149.0.0_amd64.deb && \
    dpkg -i minio_20240710184149.0.0_amd64.deb

ARG BUILD_ARCH=amd64
RUN \
    apt-get update \
    \
    && ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    \
    && curl -J -L -o /tmp/minio.deb \
    https://dl.min.io/server/minio/release/linux-amd64/minio_20240710184149.0.0_${BUILD_ARCH}.deb \
    \
    && dpkg --install /tmp/minio.deb \
    \
    && rm -fr \
    /tmp/* \
    /etc/nginx \
    /var/{cache,log}/* \
    /var/lib/apt/lists/* \
    \
    && mkdir -p /var/log/nginx \
    && touch /var/log/nginx/error.log

EXPOSE 9001
EXPOSE 9000

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

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