FROM alpine:3.20

ARG SHERPA_ONNX_GIT_TAG=v1.10.1

LABEL maintainer="cmahnke@gmail.com"
LABEL org.opencontainers.image.source https://github.com/cmahnke/speech-tools

ENV BUILD_DEPS="git wget cmake make automake autoconf libtool clang g++ musl-dev zlib-dev icu-dev alsa-lib-dev hwloc-dev jack-dev espeak-ng-dev openfst-dev" \
    RUN_DEPS="gcompat zlib alsa-utils alsa-lib icu icu-data-full hwloc jack lame espeak-ng websocket++ openfst" \
    SHERPA_ONNX_GIT_REPO="https://github.com/k2-fsa/sherpa-onnx" \
    BUILD_DIR=/tmp/build

RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    apk --update upgrade && \
    apk add --no-cache $RUN_DEPS $BUILD_DEPS && \
    mkdir -p $BUILD_DIR && \
    cd $BUILD_DIR && \
    export CC=clang CXX=clang++ && \
# Sherpa ONNX
    git clone --depth 1 --branch $SHERPA_ONNX_GIT_TAG $SHERPA_ONNX_GIT_REPO && \
    cd sherpa-onnx && \
    mkdir -p build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr .. && \
    make && make install && \
    cd ../.. && \
#RUN     export CC=clang CXX=clang++ && \
# Cleanup
    rm -rf $BUILD_DIR  && \
    apk del $BUILD_DEPS && \
    rm -rf /var/cache/apk/* /root/.cache
