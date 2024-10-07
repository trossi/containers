# https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch
FROM nvcr.io/nvidia/pytorch:24.09-py3

ENV DEBIAN_FRONTEND=noninteractive \
    PIP_NO_CACHE_DIR=1 \
    PIP_ROOT_USER_ACTION=ignore

# Install packages via apt-get
# RUN apt-get update -qy && \
#     apt-get install -qy \
#         vim \
#         && \
#     apt-get clean


# Install packages via pip
RUN python3 -m pip install \
        flash-attn \
        anemoi-inference[plugin] \
        anemoi-models

