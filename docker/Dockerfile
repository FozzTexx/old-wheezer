# -*- mode: Fundamental; indent-tabs-mode: nil -*-

ARG BASE=yamamuteki/debian-etch-i386
#ARG BASE=i386/debian:7

FROM ${BASE} as wget

RUN set -e \
    ; sed -i -r -e s,http://[^.]+,http://archive, /etc/apt/sources.list \
    ; apt-get update || true \
    ; apt-get install -y --force-yes \
        build-essential \
        dpkg-dev \
        libz-dev \
        pkg-config \
    ;

WORKDIR /build

# Install a newer version of Perl, needed for openssl
ADD https://www.cpan.org/src/5.0/perl-5.38.0.tar.gz /build/
RUN <<EOF
  set -e
  tar -xf perl-*.tar.gz
  cd perl-5.38.0
  ./Configure -des -Dprefix=/usr/local
  make -j$(nproc)
  make install
  perl -v # Verify installation
EOF

# Build and package OpenSSL
ADD https://www.openssl.org/source/openssl-3.1.2.tar.gz /build/
RUN <<EOF
  set -e
  PKG=openssl
  tar -xf ${PKG}-*.tar.gz
  VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
  cd ${PKG}-*
  ./Configure linux-x86 no-threads --prefix=/usr/local --openssldir=/usr/local/ssl shared zlib
  make -j$(nproc)
  make install DESTDIR=/tmp/${PKG}
  mkdir -p /tmp/${PKG}/DEBIAN
  sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control << CTLEND
Package: --PACKAGE--
Version: --VERSION--
Architecture: i386
Maintainer: fozztexx@fozztexx.com
Description: --PKG-- --VERSION--
CTLEND
  dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
  dpkg -i /build/${PKG}_${VERS}.deb
EOF

# # Build and package xz
# ADD https://github.com/tukaani-project/xz/releases/download/v5.6.3/xz-5.6.3.tar.gz /build/
# RUN <<EOF
#   set -ex
#   PKG=xz
#   tar -xf ${PKG}-*.tar.gz
#   VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
#   cd ${PKG}-*
#   ./configure
#   make
#   make install DESTDIR=/tmp/${PKG}
#   mkdir -p /tmp/${PKG}/DEBIAN
#   sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control <<CTLEND
# Package: --PACKAGE--
# Version: --VERSION--
# Architecture: i386
# Maintainer: fozztexx@fozztexx.com
# Description: --PKG-- --VERSION--
# CTLEND
#   dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
#   dpkg -i /build/${PKG}_${VERS}.deb
# EOF

# # # Build and package GnuTLS
# # ADD https://www.gnupg.org/ftp/gcrypt/gnutls/v3.8/gnutls-3.8.0.tar.xz /build/
# # RUN <<EOF
# #   set -e
# #   PKG=gnutls
# #   cat ${PKG}-*.tar.xz | xzcat | tar -xf -
# #   VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
# #   cd ${PKG}-*
# #   ./configure --prefix=/usr/local --with-openssl --with-included-libtasn1 --with-included-unistring
# #   make -j$(nproc)
# #   make install DESTDIR=/tmp/${PKG}
# #   mkdir -p /tmp/${PKG}/DEBIAN
# #   sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control << CTLEND
# # Package: --PACKAGE--
# # Version: --VERSION--
# # Architecture: i386
# # Maintainer: fozztexx@fozztexx.com
# # Description: --PKG-- --VERSION--
# # CTLEND
# #   dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
# # EOF

# # Build and package wget
# ADD https://ftp.gnu.org/gnu/wget/wget-latest.tar.gz /build/
# RUN <<EOF
#   set -e
#   PKG=wget
#   tar -xf ${PKG}-*.tar.gz
#   VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
#   cd ${PKG}-*
#   ./configure --prefix=/usr/local --with-ssl=openssl
#   make
#   make install DESTDIR=/tmp/${PKG}
#   mkdir -p /tmp/${PKG}/DEBIAN
#   sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control <<CTLEND
# Package: --PACKAGE--
# Version: --VERSION--
# Architecture: i386
# Maintainer: fozztexx@fozztexx.com
# Description: --PKG-- --VERSION--
# CTLEND
#   dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
#   ls -Fla
#   dpkg -i /build/${PKG}_${VERS}.deb
# EOF

ADD https://curl.se/download/curl-7.80.0.tar.gz /build/
RUN <<EOF
  set -e
  PKG=curl
  tar -xf ${PKG}-*.tar.gz
  VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
  cd ${PKG}-*
  ./configure --prefix=/usr/local --with-openssl
  make
  make install DESTDIR=/tmp/${PKG}
  mkdir -p /tmp/${PKG}/DEBIAN
  sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control <<CTLEND
Package: --PACKAGE--
Version: --VERSION--
Architecture: i386
Maintainer: fozztexx@fozztexx.com
Description: --PKG-- --VERSION--
CTLEND
  dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
  dpkg -i /build/${PKG}_${VERS}.deb
EOF

# Build and package git
ADD https://github.com/git/git/archive/refs/tags/v2.42.0.tar.gz /build/
RUN <<EOF
  set -ex
  apt-get install -y autoconf automake gettext
  PKG=git
  tar -xf v2.42.0.tar.gz
  VERS=$(echo ${PKG}-*/ | sed -e 's/.*-// ; s,/,,')
  cd ${PKG}-*
  autoconf
  ./configure --prefix=/usr/local CFLAGS=-std=c99
  make
  make install DESTDIR=/tmp/${PKG}
  mkdir -p /tmp/${PKG}/DEBIAN
  sed -e "s/--VERSION--/${VERS}/ ; s/--PACKAGE--/${PKG}/" >/tmp/${PKG}/DEBIAN/control <<CTLEND
Package: --PACKAGE--
Version: --VERSION--
Architecture: i386
Maintainer: fozztexx@fozztexx.com
Description: --PKG-- --VERSION--
CTLEND
  dpkg-deb --build /tmp/${PKG} /build/${PKG}_${VERS}.deb
  dpkg -i /build/${PKG}_${VERS}.deb
EOF

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
        file \
        g++ \
        less \
        libboulder-perl \
        libssl-dev \
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

RUN mkdir -p /tmp/packages
COPY --from=wget /build/*.deb /tmp/packages/
RUN set -e \
    ; ls -Fla /tmp/packages \
    ; dpkg -i /tmp/packages/*.deb \
    ; echo /usr/local/lib > /etc/ld.so.conf.d/local.conf \
    ; ldconfig \
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
