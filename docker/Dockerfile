# -*- mode: Fundamental; indent-tabs-mode: nil -*-

ARG BASE=i386/debian:7

FROM ${BASE}
LABEL maintainer fozztexx@fozztexx.com

ENV DEBIAN_FRONTEND noninteractive
ENV WSUSER wario

RUN set -e \
    ; sed -i -r -e s,http://[^.]+,http://archive, /etc/apt/sources.list \
    ; apt-get update || true \
    ; apt-get install -y --force-yes --no-install-recommends \
        g++ \
        make \
        ncurses-dev \
        patch \
        python \
        realpath \
        sudo \
    ; rm -rf /var/lib/apt/lists/* \
    ;

RUN set -e \
    ; useradd -s /bin/bash ${WSUSER} \
    ; eval WSHOME=~${WSUSER} \
    ; mkdir -p ${WSHOME} \
    ; chown -R ${WSUSER}.${WSUSER} ${WSHOME} \
    ; echo "${WSUSER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    ;

# Copy init script
COPY cntnr-init /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/cntnr-init"]
