FROM docker.io/rocm/dev-ubuntu-24.04:6.2.4-complete

ENV DEBIAN_FRONTEND=noninteractive

ARG LLVM_VERSION

RUN apt-get update -qy && \
    apt-get install -qy \
        cmake \
        clang-$LLVM_VERSION \
        clang-tools-$LLVM_VERSION \
        libclang-$LLVM_VERSION-dev \
        libomp-$LLVM_VERSION-dev \
        llvm-$LLVM_VERSION-dev \
        lld-$LLVM_VERSION \
        libboost-dev \
        libboost-fiber-dev \
        libboost-context-dev \
        libtbb-dev \
        && \
    apt-get clean

ARG ACPP_VERSION

RUN mkdir -p /opt/build && \
    cd /opt/build && \
    curl -LOJ https://github.com/AdaptiveCpp/AdaptiveCpp/archive/refs/tags/v$ACPP_VERSION.tar.gz && \
    tar xf AdaptiveCpp-$ACPP_VERSION.tar.gz && \
    cd AdaptiveCpp-$ACPP_VERSION && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_C_COMPILER=clang-$LLVM_VERSION -DCMAKE_CXX_COMPILER=clang++-$LLVM_VERSION -DACPP_COMPILER_FEATURE_PROFILE=full -DROCM_PATH=/opt/rocm -DWITH_ROCM_BACKEND=ON .. && \
    make -j8 && \
    make install && \
    cd /opt && \
    rm -r /opt/build

