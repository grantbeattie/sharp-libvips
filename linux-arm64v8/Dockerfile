FROM debian:bullseye
LABEL maintainer="Lovell Fuller <npm@lovell.info>"

# Create Debian-based container suitable for cross-compiling Linux ARM64v8-A binaries

# Build dependencies
RUN \
  apt-get update && \
  apt-get install -y curl && \
  dpkg --add-architecture arm64 && \
  apt-get update && \
  apt-get install -y \
    advancecomp \
    autoconf \
    autopoint \
    cmake \
    crossbuild-essential-arm64 \
    gettext \
    git \
    gobject-introspection \
    gperf \
    gtk-doc-tools \
    intltool \
    jq \
    libglib2.0-dev \
    libtool \
    nasm \
    ninja-build \
    patchelf \
    python3-pip \
    texinfo \
    && \
  curl https://sh.rustup.rs -sSf | sh -s -- -y && \
  ~/.cargo/bin/rustup target add aarch64-unknown-linux-gnu && \
  pip3 install meson==0.53.2

# Compiler settings
ENV \
  PATH="/root/.cargo/bin:$PATH" \
  PKG_CONFIG="/usr/bin/aarch64-linux-gnu-pkg-config" \
  PLATFORM="linux-arm64v8" \
  CHOST="aarch64-linux-gnu" \
  FLAGS="-march=armv8-a -s -O3 -fPIC -D_GLIBCXX_USE_CXX11_ABI=0" \
  MESON="--cross-file=/root/meson.ini"

COPY Toolchain.cmake /root/
COPY meson.ini /root/
