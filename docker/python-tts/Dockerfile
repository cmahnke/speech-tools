FROM python:3.11-slim-bullseye

LABEL maintainer="cmahnke@gmail.com"
LABEL org.opencontainers.image.source https://github.com/cmahnke/speech-tools

ENV BUILD_DEPS="build-essential curl git-lfs libsndfile1-dev" \
    RUN_DEPS="espeak-ng libsndfile1" \
    SHERPA_ONNX_GIT_REPO="https://github.com/k2-fsa/sherpa-onnx" \
    BUILD_DIR=/tmp/build

RUN --mount=target=/mnt/build-context \
    apt-get update && \
    apt-get upgrade && \
    apt install -y $BUILD_DEPS $RUN_DEPS && \
    pip install --upgrade pip && \
    mkdir -p $BUILD_DIR && \
    cd $BUILD_DIR && \
    curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | bash -s -- -y && \
    . $HOME/.cargo/env && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
# Python TTS
# Use `coqui-tts` instead of `TTS`, since it supports current Python
    pip install -r /mnt/build-context/docker/python-tts/requirements.txt && \
    cd / && \
# Cleanup
    rustup self uninstall -y && \
    rm -rf $BUILD_DIR  && \
    apt-get remove -y $BUILD_DEPS && \
    rm -rf /var/cache/apk/* /root/.cache

ENTRYPOINT /bin/bash
