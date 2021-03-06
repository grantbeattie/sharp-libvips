FROM debian:buster
LABEL maintainer="Lovell Fuller <npm@lovell.info>"

# Create Debian-based container suitable for cross-compiling Linux ARMv6 binaries

# Build dependencies
RUN \
  apt-get update && \
  apt-get install -y curl && \
  dpkg --add-architecture armhf && \
  apt-get update && \
  apt-get install -y \
    advancecomp \
    autoconf \
    autopoint \
    cmake \
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
  mkdir /root/tools && \
  curl -Ls https://github.com/rvagg/rpi-newer-crosstools/archive/master.tar.gz | tar xzC /root/tools --strip-components=1 && \
  curl https://sh.rustup.rs -sSf | sh -s -- -y && \
  ~/.cargo/bin/rustup target add arm-unknown-linux-gnueabihf && \
  pip3 install meson==0.53.2

# Compiler settings
ENV \
  PATH="/root/.cargo/bin:/root/tools/x64-gcc-6.5.0/arm-rpi-linux-gnueabihf/bin:$PATH" \
  PKG_CONFIG="/usr/bin/arm-linux-gnueabihf-pkg-config" \
  PLATFORM="linux-armv6" \
  CHOST="arm-rpi-linux-gnueabihf" \
  RUST_TARGET="arm-unknown-linux-gnueabihf" \
  FLAGS="-marm -mcpu=arm1176jzf-s -mfpu=vfp -mfloat-abi=hard -s -O3 -fPIC -D_GLIBCXX_USE_CXX11_ABI=0" \
  MESON="--cross-file=/root/meson.ini"

COPY Toolchain.cmake /root/
COPY meson.ini /root/
