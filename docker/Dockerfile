# -*- mode: Fundamental; indent-tabs-mode: nil -*-

ARG BASE=yamamuteki/debian-etch-i386
#ARG BASE=i386/debian:7

# Assemble all the pieces together for final image
FROM ${BASE}
LABEL maintainer fozztexx@fozztexx.com

ENV DEBIAN_FRONTEND noninteractive
ENV WSUSER wario

RUN set -e \
    ; sed -i -r -e s,http://[^.]+,http://archive, /etc/apt/sources.list \
    ; apt-get update || true \
    ; apt-get install -y --force-yes \
        bc \
        bzip2 \
        cpio \
        curl \
        file \
        g++ \
        git \
        less \
        libboulder-perl \
        locales \
        make \
        ncurses-dev \
        patch \
        python \
        rsync \
        sudo \
        texinfo \
        unzip \
        wget \
    #; rm -rf /var/lib/apt/lists/* \
     \
    ; echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    ; /usr/sbin/locale-gen en_US.UTF-8 \
    ; update-locale LANG=en_US.UTF-8 \
    ;

ADD https://github.com/tukaani-project/xz/releases/download/v5.6.3/xz-5.6.3.tar.gz /tmp/
RUN <<EOF
  set -e
  tar -xf /tmp/xz-5.6.3.tar.gz -C /tmp
  cd /tmp/xz-5.6.3
  ./configure
  make
  make install
  rm -rf /tmp/xz-5.6.3 /tmp/xz-5.6.3.tar.gz
EOF

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
