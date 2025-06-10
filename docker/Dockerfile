ARG CUDA_VERSION=12.4.1

FROM vastai/vllm:v0.8.5-cuda-12.4-pytorch-2.6.0-py312

ARG BUILD_TYPE=all
ENV DEBIAN_FRONTEND=noninteractive

RUN echo 'tzdata tzdata/Areas select America' | debconf-set-selections \
    && echo 'tzdata tzdata/Zones/America select Los_Angeles' | debconf-set-selections \
    && apt update -y \
    && apt install software-properties-common -y \
    && apt install python3 python3-pip -y \
    && apt install curl git sudo libibverbs-dev -y \
    && apt install rdma-core infiniband-diags openssh-server perftest -y \
    && python3 --version \
    && python3 -m pip --version \
    && rm -rf /var/lib/apt/lists/* \
    && apt clean

# For openbmb/MiniCPM models
RUN pip3 install datamodel_code_generator

WORKDIR /sgl-workspace

ARG CUINDEX=124
RUN python3 -m pip install --upgrade pip setuptools wheel html5lib six --ignore-installed \
    && git clone --depth=1 -b fix/update_torch https://github.com/WayneLe17/sglang.git \
    && cd sglang \
    && python3 -m pip --no-cache-dir install -e "python[${BUILD_TYPE}]"

ENV DEBIAN_FRONTEND=interactive
