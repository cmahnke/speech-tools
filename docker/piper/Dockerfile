FROM debian:12-slim

ARG PIPER_GIT_TAG=2023.11.14-2

LABEL maintainer="cmahnke@gmail.com"
LABEL org.opencontainers.image.source https://github.com/cmahnke/speech-tools

ENV BUILD_DEPS="git wget cmake build-essential " \
    RUN_DEPS="lame " \
    PIPER_GIT_REPO="https://github.com/rhasspy/piper.git" \
    PIPER_PHONEMIZE_GIT_REPO="https://github.com/rhasspy/piper-phonemize.git" \
    BUILD_DIR=/tmp/build \
    PATH="${PATH}:/opt/piper"

# alsa-utils alsa-lib icu icu-data-full hwloc jack espeak-ng websocket++ openfst

RUN --mount=target=/mnt/build-context \
    apt-get update && \
    apt-get upgrade && \
    apt install -y $BUILD_DEPS $RUN_DEPS && \
    mkdir -p $BUILD_DIR && \
    cd $BUILD_DIR && \
## Piper phonemize
#    git clone --depth 1 --branch $PIPER_GIT_TAG $PIPER_PHONEMIZE_GIT_REPO && \
#    cd piper-phonemize && \
#    mkdir -p build && cd build && \
#    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_LIBRARY_PATH_FLAG=/usr/glibc-compat/lib -DCMAKE_INSTALL_PREFIX=/opt/piper .. && \
#    make && \
#    cd ../../ && \
# Piper
    git clone --depth 1 --branch $PIPER_GIT_TAG $PIPER_GIT_REPO && \
    #cp -dR piper-phonemize/build/install/* ./piper/
    cd piper && \
    mkdir -p build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr .. && \
    make && make install && \
    cd ../.. && \
# Cleanup
    rm -rf $BUILD_DIR  && \
    apt-get remove -y $BUILD_DEPS && \
    rm -rf /var/cache/apk/* /root/.cache

ENTRYPOINT /opt/piper/piper
