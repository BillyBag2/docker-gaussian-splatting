ARG UBUNTU_VERSION=22.04

ARG CUDA_VERSION=11.8.0

FROM nvidia/cuda:${CUDA_VERSION}-devel-ubuntu${UBUNTU_VERSION}

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update \
    && apt-get install -y --no-install-recommends \
    git \
    nano \
    wget

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda clean -t -i -p -y && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

SHELL ["/bin/bash", "-c"]

ENV PATH /opt/conda/bin:$PATH
ENV CONDA_DEFAULT_ENV myenv   

RUN git clone https://github.com/graphdeco-inria/gaussian-splatting --recursive

RUN . /opt/conda/etc/profile.d/conda.sh \
    && conda activate base \
    && conda install -y pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

ENV TORCH_CUDA_ARCH_LIST "6.1"

# CUDA_VISIBLE_DEVICES=1

RUN cd gaussian-splatting \
   && conda env create --file environment.yml



