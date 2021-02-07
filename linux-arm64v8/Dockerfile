FROM --platform=linux/arm64/v8 ubuntu:bionic
LABEL maintainer="Lovell Fuller <npm@lovell.info>"

# Create Debian-based container suitable for cross-compiling Linux ARM64v8-A binaries

# Path settings
ENV \
  RUSTUP_HOME="/usr/local/rustup" \
  CARGO_HOME="/usr/local/cargo" \
  PATH="/usr/local/cargo/bin:$PATH"

COPY sources.list /etc/apt/sources.list

ARG CMAKE_VERISON=3.19.4

# Build dependencies
RUN \
  apt-get update && \
  apt-get install -y \
    curl \
    advancecomp \
    autoconf \
    autopoint \
    brotli \
    cmake \
    dpkg-cross \
    g++-8 \
    gettext \
    git \
    gobject-introspection \
    gperf \
    gtk-doc-tools \
    intltool \
    jq \
    libglib2.0-dev \
    libssl-dev \
    libtool \
    nasm \
    ninja-build \
    python3-pip \
    texinfo \
    && \
  update-alternatives \
    --install /usr/bin/gcc gcc /usr/bin/gcc-8 100 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-8 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-8 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-8 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-8 \
    --slave /usr/bin/gcov gcov /usr/bin/gcov-8 \
    --slave /usr/bin/gcov-dump gcov-dump /usr/bin/gcov-dump-8 \
    --slave /usr/bin/gcov-tool gcov-tool /usr/bin/gcov-tool-8 \
    && \
  update-alternatives \
    --install /usr/bin/cpp cpp /usr/bin/cpp-8 100 \
    && \
  curl https://sh.rustup.rs -sSf | sh -s -- -y \
    --no-modify-path \
    --profile minimal \
    && \
  rustup target add aarch64-unknown-linux-gnu && \
  pip3 install meson && \
  curl -L https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERISON}/cmake-${CMAKE_VERISON}.tar.gz | tar zx -C / && \
    cd /cmake-${CMAKE_VERISON} && \
    ./bootstrap && \
    make && \
    make install && \
    cd - && \
    rm -rf /cmake-${CMAKE_VERISON}

# Compiler settings
ENV \
  # PKG_CONFIG="/usr/bin/aarch64-linux-gnu-pkg-config" \
  PLATFORM="linux-arm64v8" \
  # CHOST="aarch64-linux-gnu" \
  # RUST_TARGET="aarch64-unknown-linux-gnu" \
  FLAGS="-march=armv8-a -O3 -fPIC -D_GLIBCXX_USE_CXX11_ABI=0" \
  MESON="--cross-file=/root/meson.ini"

COPY Toolchain.cmake /root/
COPY meson.ini /root/
