ARG BUILD_FROM=alpine:3.6
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        gcc=6.4.0-r8 \
        linux-pam-dev=1.3.0-r0 \
        make=4.2.1-r2 \
        musl-dev=1.1.19-r10 \
    \
    && cd /tmp \
    && curl -sSL https://github.com/tiwe-de/libpam-pwdfile/archive/v1.0.tar.gz \
        | tar xz --strip 1 \
    && make \
    && make install \
    && cd - \
    \
    && apk add --no-cache \
        openssl=1.0.2p-r0 \
        vsftpd=3.0.3-r5 \
    \
    && apk del --purge .build-dependencies \
    \
    && mkdir -p \
        /ftproot/empty \
        /ftproot/users \
        /etc/vsftpd/users \
    \
    && rm -f -r \
        /tmp/*

 # Copy root filesystem
COPY rootfs /

# Build arugments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.csm.name="FTP" \
    io.csm.description="A secure and fast FTP server for CSM" \
    io.csm.arch="${BUILD_ARCH}" \
    io.csm.type="addon" \
    io.csm.version=${BUILD_VERSION} \
    maintainer="Klaudiusz Staniek <klstanie@cisco.com>" \
    org.label-schema.description="A secure and fast FTP server for CSM" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="FTP" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://github.com/csmcloud/csm-addons/tree/master/ftp" \
    org.label-schema.usage="https://github.com/csmcloud/csm-addons/tree/master/ftp/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/csmcloud/csm-addons/tree/master/ftp" \
    org.label-schema.vendor="Cisco"