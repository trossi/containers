FROM docker.io/ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qy && \
    apt-get install -qy \
        build-essential \
        cmake \
        git \
        libhdf5-serial-dev \
        libgtk-3-dev \
        libx11-dev \
        libxcursor-dev \
        libxinerama-dev \
        libxrandr-dev \
        pkgconf \
        && \
    apt-get clean

RUN git clone -b v0.1.38 --recurse-submodules https://github.com/scanberg/viamd.git /opt/viamd && \
    cd /opt/viamd && \
    mkdir build && \
    cd build && \
    cmake .. && \
    cmake --build .

ENV PATH=/opt/viamd/build/bin:$PATH

ENTRYPOINT ["viamd"]
