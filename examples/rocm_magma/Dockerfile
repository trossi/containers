FROM docker.io/rocm/dev-ubuntu-22.04:6.3.2-complete

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qy && \
    apt-get install -qy \
        build-essential \
        gfortran \
        libopenblas-openmp-dev \
        python3 \
        python-is-python3 \
        git \
        && \
    apt-get clean

RUN mkdir -p /opt/magma && \
    git clone --depth 1 --branch v2.9.0 https://github.com/icl-utk-edu/magma.git /opt/magma-build && \
    cd /opt/magma-build && \
    cp make.inc-examples/make.inc.hip-gcc-openblas make.inc && \
    sed -i 's/__HIP_PLATFORM_HCC__/__HIP_PLATFORM_AMD__/g' Makefile && \
    MAKE_OPTS='CC=gcc CXX=g++ FORT=gfortran GPU_TARGET=gfx90a OPENBLASDIR=/usr/lib/x86_64-linux-gnu/openblas-openmp' && \
    make -j8 $MAKE_OPTS && \
    make install prefix=/opt/magma $MAKE_OPTS && \
    rm -rf /opt/magma-build

ENV CPATH=/opt/magma/include \
    LIBRARY_PATH=/opt/magma/lib \
    LD_LIBRARY_PATH=/opt/magma/lib \
    LC_ALL=C.UTF-8
