FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
LABEL maintainer="yucao cyuleo@126.com"
ENV LANG C.UTF-8

COPY pip.conf /etc/pip.conf

RUN rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list &&\
     apt-get clean &&\
     apt-get update \
     &&\

    apt-get install -y \
    python3 \
    python3-pip \
    git \
    vim \
    lib32z1 \
    libglib2.0-dev \
    libsm6 \
    libxrender1 \
    libxext6 \
    libice6 \
    libxt6 \
    libfontconfig1 \
    libcups2 \
    && ln -s /usr/bin/pip3 /usr/bin/pip \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && pip install cython torch==1.1.0 torchvision==0.3.0 \
    && pip install ipython jupyter notebook matplotlib seaborn scikit-learn scipy scikit-image pandas xlrd \
    mmcv>=0.2.6 numpy six terminaltables pycocotools \
    &&\

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* 
