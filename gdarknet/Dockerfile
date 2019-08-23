# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (apt)
# opencv        3.4.0  (git)
# g-darknet       latest (git)
# ==================================================================

FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
LABEL maintainer="yucao cyuleo@126.com"
ENV LANG C.UTF-8

COPY pip.conf /etc/pip.conf

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    GIT_CLONE="git clone --depth 10" && \
    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \
# ==================================================================
# change sources
# ------------------------------------------------------------------
    sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list &&\
     apt-get clean &&\
     apt-get update \
     &&\
# ==================================================================
# tools
# ------------------------------------------------------------------
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL\
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
    cmake \
    && ln -s /usr/bin/pip3 /usr/bin/pip \
    && ln -s /usr/bin/python3 /usr/bin/python \
    &&\
# ==================================================================
# python packages
# ------------------------------------------------------------------
    pip install numpy scipy pandas cloudpickle scikit-learn matplotlib Cython \
    && \
# ==================================================================
# opencv
# ------------------------------------------------------------------
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        && \

    $GIT_CLONE --branch 4.0.1 https://github.com/opencv/opencv ~/opencv && \
    mkdir -p ~/opencv/build && cd ~/opencv/build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D WITH_IPP=OFF \
          -D WITH_CUDA=OFF \
          -D WITH_OPENCL=OFF \
          -D BUILD_TESTS=OFF \
          -D BUILD_PERF_TESTS=OFF \
          .. && \
    make -j"$(nproc)" install && \
    ln -s /usr/local/include/opencv4/opencv2 /usr/local/include/opencv2 && \
# ==================================================================
# g-darknet
# ------------------------------------------------------------------
    $GIT_CLONE https://github.com/pjreddie/darknet.git ~/darknet && \
    cd ~/darknet && \
    sed -i 's/GPU=0/GPU=1/g' ~/darknet/Makefile && \
    sed -i 's/CUDNN=0/CUDNN=1/g' ~/darknet/Makefile && \
    make -j"$(nproc)" && \
    cp ~/darknet/include/* /usr/local/include && \
    cp ~/darknet/*.a /usr/local/lib && \
    cp ~/darknet/*.so /usr/local/lib && \
    cp ~/darknet/darknet /usr/local/bin && \
# ==================================================================
# config & cleanup
# ------------------------------------------------------------------
    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* 