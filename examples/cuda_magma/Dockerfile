FROM nvcr.io/nvidia/cuda:12.6.1-devel-ubuntu22.04

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
    cp make.inc-examples/make.inc.openblas make.inc && \
    MAKE_OPTS='CC=gcc CXX=g++ FORT=gfortran GPU_TARGET=Ampere CUDADIR=/usr/local/cuda OPENBLASDIR=/usr/lib/x86_64-linux-gnu/openblas-openmp' && \
    make -j8 $MAKE_OPTS && \
    make install prefix=/opt/magma $MAKE_OPTS && \
    rm -rf /opt/magma-build

ENV CPATH=/opt/magma/include:/usr/local/cuda/include \
    LIBRARY_PATH=/opt/magma/lib:/usr/local/cuda/lib64/stubs \
    LD_LIBRARY_PATH=/opt/magma/lib:/usr/local/cuda/lib64 \
    LC_ALL=C.UTF-8
