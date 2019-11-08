

FROM        alpine:3.8 AS base

RUN     apk  add --no-cache --update libgcc libstdc++ ca-certificates libcrypto1.0 libssl1.0 libgomp expat git


FROM        base AS build

WORKDIR     /tmp/workdir

ARG        PKG_CONFIG_PATH=/opt/lame/lib/pkgconfig
ARG        LD_LIBRARY_PATH=/opt/lame/lib
ARG        PREFIX=/opt/lame
ARG        MAKEFLAGS="-j2"

ENV         LAME_VERSION=3.99.5

RUN     buildDeps="autoconf \
                   automake \
                   bash \
                   binutils \
                   bzip2 \
                   cmake \
                   curl \
                   coreutils \
                   diffutils \
                   file \
                   g++ \
                   gcc \
                   gperf \
                   libtool \
                   make \
                   python \
                   openssl-dev \
                   tar \
                   yasm \
                   nasm \
                   zlib-dev \
                   expat-dev" && \
        apk  add --no-cache --update ${buildDeps}

### libmp3lame http://lame.sourceforge.net/
RUN \
        DIR=/tmp/lame && \
        mkdir -p ${DIR} && \
        cd ${DIR} && \
        curl -sL https://versaweb.dl.sourceforge.net/project/lame/lame/$(echo ${LAME_VERSION} | sed -e 's/[^0-9]*\([0-9]*\)[.]\([0-9]*\)[.]\([0-9]*\)\([0-9A-Za-z-]*\)/\1.\2/')/lame-${LAME_VERSION}.tar.gz | \
        tar -zx --strip-components=1 && \
        ./configure --prefix="${PREFIX}" --bindir="${PREFIX}/bin" --enable-static --disable-shared --enable-nasm  && \
        make && \
        make install && \
        rm -rf ${DIR}

 RUN \
     cp ${PREFIX}/bin/lame /usr/local/bin

### Release Stage
FROM        base AS release

CMD         ["--help"]
ENTRYPOINT  ["lame"]

COPY --from=build /usr/local/bin/lame /usr/local/bin