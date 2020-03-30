FROM ubuntu:18.04
ARG TENSORFLOW_TYPE=tensorflow
#FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
#ARG TENSORFLOW_TYPE=tensorflow-gpu

# ********SWITCH BETWEEN CPU AND GPU ABOVE********

MAINTAINER hao jiang (hao.jiang@corerain.com)
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NOWARNINGS yes
ENV TERM xterm-256color
ARG USER_NAME=corerain
ARG MAKE_THREADS=4

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    autoconf \
    automake \
    build-essential \
    curl \
    ctags \
    ca-certificates \
    dirmngr \
    freeglut3-dev \
    gnupg2 \
    gpg-agent \
    graphviz \
    gawk \
    git \
    id-utils \
    libssl-dev \
    libpq-dev \
    libtool \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    libgtk2.0-dev \
    libcairo2-dev \
    libmtdev-dev \
    libxkbcommon-x11-0 \
    libglu1-mesa-dev \
    libatlas-base-dev \
    libboost-all-dev \
    libhdf5-serial-dev \
    libleveldb-dev \
    liblmdb-dev \
    libsnappy-dev \
    mesa-common-dev \
    net-tools \
    openssh-server \
    pkg-config\
    python3 \
    python3-dev \
    python3-pip \
    sudo \
    software-properties-common \
    unzip \
    virtualenv \
    vim \
    wget \
    xauth \
    xclip \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN echo "deb http://downloads.skewed.de/apt bionic main" >> /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-key 612DEFB798507F25 \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    python3-graph-tool \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN useradd -ms /bin/bash ${USER_NAME} \
    && echo "${USER_NAME}:admin" | chpasswd \
    && adduser ${USER_NAME} sudo
USER ${USER_NAME}
RUN mkdir -p ~/tmp/

# Install cmake
RUN cd ~/tmp/ \
    && wget -O cmake-3.14.7-Linux-x86_64.tar.gz \
    https://cmake.org/files/v3.14/cmake-3.14.7-Linux-x86_64.tar.gz \
    && tar xzf cmake-3.14.7-Linux-x86_64.tar.gz \
    && echo admin | sudo -S mv cmake-3.14.7-Linux-x86_64 /opt/cmake-3.14.7 \
    && echo admin | sudo -S ln -sf /opt/cmake-3.14.7/bin/* /usr/bin/ \
    && rm -rf ~/tmp/*


# Install OpenCV for C and C++
RUN cd ~/tmp/ \
    && wget -O opencv-3.4.zip \
    https://codeload.github.com/opencv/opencv/zip/3.4 \
    && ls \
    && unzip opencv-3.4.zip \
    && cd opencv-3.4 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_PROTOBUF=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DWITH_CUBLAS=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_CUFFT=OFF \
    -DWITH_OPENCL=OFF \
    -DWITH_OPENCLAMDBLAS=OFF \
    -DWITH_OPENCLAMDFFT=OFF \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install protobuf
RUN cd ~/tmp/ \
    && wget -O protobuf-3.5.1.1.zip \
    https://codeload.github.com/protocolbuffers/protobuf/zip/3.5.1.1 \
    && ls \
    && unzip protobuf-3.5.1.1.zip \
    && cd protobuf-3.5.1.1 \
    && ./autogen.sh \
    && ./configure \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install gtest
RUN cd ~/tmp/ \
    && wget -O googletest-release-1.8.0.zip \
    https://codeload.github.com/google/googletest/zip/release-1.8.0 \
    && ls \
    && unzip googletest-release-1.8.0.zip \
    && cd googletest-release-1.8.0 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -G"Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install gflags
RUN cd ~/tmp/ \
    && wget -O gflags-2.2.2.zip \
    https://codeload.github.com/gflags/gflags/zip/v2.2.2 \
    && ls \
    && unzip gflags-2.2.2.zip \
    && cd gflags-2.2.2 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install eigen
RUN cd ~/tmp/ \
    && wget -O eigen-eigen-b3f3d4950030.tar.bz2 \
    https://bitbucket.org/eigen/eigen/get/3.3.5.tar.bz2 \
    && ls \
    && tar -xvf eigen-eigen-b3f3d4950030.tar.bz2 \
    && cd eigen-eigen-b3f3d4950030 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install glog
RUN cd ~/tmp/ \
    && wget -O glog-0.3.4.zip \
    https://codeload.github.com/google/glog/zip/v0.3.4 \
    && ls \
    && unzip glog-0.3.4.zip \
    && cd glog-0.3.4 \
    && ./configure \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

# Install exvim
RUN cd ~/tmp/ \
    && git clone https://github.com/exvim/main \
    && cd main \
    && sh unix/install.sh \
    && sh unix/replace-my-vim.sh \
    && sed -i "45s/.*/        silent exec 'language C.UTF-8'/" ~/.vimrc \
    && sed -i "50s/call/\" call/" ~/.vimrc.plugins \
    && sed -i "51s/call/\" call/" ~/.vimrc.plugins \
    && rm -rf ~/tmp/*

USER root
RUN python3 -m pip install --upgrade pip \
    && python3 -m pip install --no-cache-dir "setuptools==42.0.2" \
    && python3 -m pip install --no-cache-dir \
    "numpy==1.16.4" \
    "matplotlib==3.1.2" \
    "opencv-python==4.1.2.30" \
    "click==6.7" \
    "h5py==2.10.0" \
    "Jinja2==2.10.3" \
    "pandas==0.25.3" \
    "Pillow==6.2.1" \
    "scipy==1.4.1" \
    "pydot==1.4.1" \
    "antlr4-python3-runtime==4.5" \
    "Cython==0.29.14" \
    "decorator==4.4.0" \
    "typing==3.7.4" \
    "typing_extensions==3.7.4" \
    "networkx==2.3" \
    "onnx==1.5.0" \
    "tqdm==4.32.2" \
    "pycairo==1.16.2" \
    "pyparsing==2.4.5" \
    "flask==1.1.1" \
    "flask-classful==0.14.2" \
    "graphviz==0.13.2" \
    "SgGo==0.5.3" \
    "kivy==1.11.1" \
    "pycryptodome==3.9.7" \
    "psutil==5.7.0" \
    "pyinstaller==3.6" \
    "scikit-image>=0.9.3" \
    "ipython>=3.0.0" \
    "leveldb>=0.191" \
    "nose>=1.3.0" \
    "python-dateutil>=1.4,<2" \
    "python-gflags>=2.0" \
    "pyyaml>=3.10" \
    $TENSORFLOW_TYPE"==1.14.0"

# Change download sources
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bk \
    && sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN mkdir -p /root/.pip/ \
    && echo "[global]" >> /root/.pip/pip.conf \
    && echo "index-url = https://pypi.tuna.tsinghua.edu.cn/simple" >> /root/.pip/pip.conf \
    && echo "extra-index-url = https://pypi.python.org/pypi" >> /root/.pip/pip.conf

USER ${USER_NAME}

# Install Caffe
RUN cd ~/tmp/ \
    && git clone -b 1.0 --depth 1 https://github.com/BVLC/caffe.git . \
    && mkdir0 -p build \
    && cd build \
    && cmake -DCPU_ONLY=1 .. \
    && make -j$MAKE_THREADS \
    && echo admin | sudo -S make install \
    && echo admin | sudo -S ldconfig \
    && rm -rf ~/tmp/*

RUN PYTHON_V=$(python3 -c 'import sys; print(str(sys.version_info[0])+"."+ \
                                             str(sys.version_info[1]))') \
    && echo "export LC_ALL=C.UTF-8" >> ~/.bashrc \
    && echo "export LANG=C.UTF-8" >> ~/.bashrc \
    && echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/python${PYTHON_V}/dist-packages/" >> ~/.bashrc \
    && echo admin | sudo -S ln -s /usr/bin/python3 /usr/bin/python \
    && echo admin | sudo -S ln -s /usr/bin/pip3 /usr/bin/pip

WORKDIR /home/${USER_NAME}
# Finished
# To build an image:
# docker build -t <image_name>:<image_tag> -f <Dockerfile_name> .
