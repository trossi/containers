ARG CUDA_VERSION
FROM nvcr.io/nvidia/cuda:$CUDA_VERSION-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -qy && \
    apt-get install -qy --no-install-recommends \
        build-essential \
        cmake \
        gfortran \
        git \
        gnupg \
        equivs \
        wget ca-certificates \
        && \
    apt-get clean

# Install a dummy cuda package so that it shows up in apt
# -- required for hip installation using apt
ARG CUDA_VERSION
RUN equivs-control cuda.control && \
    sed -i "s/^Package:.*/Package: cuda/g" cuda.control && \
    sed -i "s/^# Version:.*/Version: $CUDA_VERSION/g" cuda.control && \
    equivs-build cuda.control && \
    dpkg -i /cuda_${CUDA_VERSION}_all.deb

# Install HIP
ARG HIP_VERSION
RUN wget https://repo.radeon.com/rocm/rocm.gpg.key -P /usr/share/keyrings/ && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/rocm.gpg.key] https://repo.radeon.com/rocm/apt/$HIP_VERSION focal main" | tee /etc/apt/sources.list.d/rocm.list && \
    apt-get update -qy && \
    apt-get install -qy --no-install-recommends \
        hip-runtime-nvidia \
        hip-dev \
        hipify-clang \
        && \
    apt-get clean

# Install hipblas etc from sources as the rocm repo has
# only the one for hip-runtime-amd
RUN wget https://github.com/ROCm/hipBLAS/archive/refs/tags/rocm-$HIP_VERSION.tar.gz -O - | tar xz -C /tmp && \
    cd /tmp/hipBLAS-rocm-$HIP_VERSION && \
    ROCM_PATH=/opt/rocm ./install.sh --cuda -i && \
    rm -rf /tmp/hipBLAS-rocm-$HIP_VERSION

RUN wget https://github.com/ROCm/hipRAND/archive/refs/tags/rocm-$HIP_VERSION.tar.gz -O - | tar xz -C /tmp && \
    cd /tmp/hipRAND-rocm-$HIP_VERSION && \
    ROCM_PATH=/opt/rocm ./install -i && \
    rm -rf /tmp/hipRAND-rocm-$HIP_VERSION

RUN wget https://github.com/ROCm/hipFFT/archive/refs/tags/rocm-$HIP_VERSION.tar.gz -O - | tar xz -C /tmp && \
    cd /tmp/hipFFT-rocm-$HIP_VERSION && \
    mkdir build && \
    cd build && \
    ROCM_PATH=/opt/rocm HIP_PLATFORM=nvidia cmake -DCMAKE_CXX_COMPILER=hipcc -DCMAKE_BUILD_TYPE=Release -L .. && \
    make install && \
    rm -rf /tmp/hipFFT-rocm-$HIP_VERSION

RUN wget https://github.com/ROCm/hipfort/archive/refs/tags/rocm-$HIP_VERSION.tar.gz -O - | tar xz -C /tmp && \
    cd /tmp/hipfort-rocm-$HIP_VERSION && \
    mkdir build && \
    cd build && \
    cmake -DHIPFORT_INSTALL_DIR=/opt/hipfort .. && \
    make install && \
    rm -rf /tmp/hipfort-rocm-$HIP_VERSION

ENV ROCM_HOME=/opt/rocm \
    HIP_COMPILER=nvcc \
    HIP_PLATFORM=nvidia \
    HIP_RUNTIME=cuda \
    CPATH=/opt/rocm/include \
    LIBRARY_PATH=/opt/rocm/lib:/usr/local/cuda/lib64/stubs \
    LD_LIBRARY_PATH=/opt/rocm/lib:/usr/local/nvidia/lib:/usr/local/nvidia/lib64 \
    PATH=/opt/rocm/bin:/opt/hipfort/bin:$PATH \
    LC_ALL=C.UTF-8
