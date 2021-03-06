---
id: RF4oAlftyLgH4AE0ek9Rc
title: Docker File Example
desc: ''
updated: 1644575012291
created: 1643115036292
---
## No proxy
```
FROM nvidia/cuda:11.3.0-runtime-ubuntu20.04

# Prevent stop building ubuntu at time zone selection.  
ENV DEBIAN_FRONTEND=noninteractive

# Prepare and empty machine for building
RUN apt-get update && \
    apt-get install -y git \
    build-essential \
    cmake \
    python3 \
    python3-pip \
	libegl1-mesa-dev \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6

RUN pip3 install PySide2 PyOpenGL

RUN git clone https://github.com/PixarAnimationStudios/USD.git

RUN python3 USD/build_scripts/build_usd.py /usr/local/USD

ENV PYTHONPATH="/usr/local/USD/lib/python:${PYTHONPATH}"

ENV PATH="/usr/local/USD/bin:${PATH}"
```

## With proxy
```
FROM nvidia/cuda:11.3.0-devel-ubuntu20.04

# Prevent stop building ubuntu at time zone selection.  
ENV DEBIAN_FRONTEND=noninteractive

# Prepare and empty machine for building
RUN apt-get update
RUN apt-get install -y shared-mime-info
RUN apt-get install -y git
RUN apt-get install -y cmake
RUN apt-get install -y vim
RUN apt-get install -y build-essential
RUN apt-get install -y libboost-atomic-dev
RUN apt-get install -y libboost-date-time-dev
RUN apt-get install -y libboost-program-options-dev
RUN apt-get install -y libboost-filesystem-dev
RUN apt-get install -y libboost-graph-dev
RUN apt-get install -y libboost-system-dev
RUN apt-get install -y libboost-test-dev
RUN apt-get install -y libatlas3-base
RUN apt-get install -y libatlas-base-dev
RUN apt-get install -y libeigen3-dev
RUN apt-get install -y libsuitesparse-dev
RUN apt-get install -y libfreeimage-dev
RUN apt-get install -y libgoogle-glog-dev
RUN apt-get install -y libgflags-dev
RUN apt-get install -y libglew-dev
RUN apt-get install -y qtbase5-dev
RUN apt-get install -y libqt5opengl5-dev
RUN apt-get install -y libcgal-dev
RUN apt-get install -y libcgal-qt5-dev

# Build and install ceres solver
RUN apt-get -y install \
    libatlas-base-dev \
    libsuitesparse-dev
RUN git config --global http.proxy http://127.0.0.1:7890
RUN git clone https://github.com/ceres-solver/ceres-solver.git --branch 1.14.0
RUN cd ceres-solver && \
	mkdir build && \
	cd build && \
	cmake .. -DBUILD_TESTING=OFF -DBUILD_EXAMPLES=OFF && \
	make -j12 && \
	make install

# Build and install COLMAP

# Note: This Dockerfile has been tested using COLMAP pre-release 3.6.
# Later versions of COLMAP (which will be automatically cloned as default) may
# have problems using the environment described thus far. If you encounter
# problems and want to install the tested release, then uncomment the branch
# specification in the line below
RUN git clone https://github.com/colmap/colmap.git

RUN cd colmap && \
	git checkout dev && \
	mkdir build && \
	cd build && \
	cmake .. && \
	make -j12 && \
	make install

# Build and install Pixel-Perfect Structure-from-Motion
RUN apt-get -y install \
    libhdf5-dev
RUN git clone https://github.com/cvg/pixel-perfect-sfm --recursive
RUN apt install -y python3-pip

RUN pip3 install --proxy=http://127.0.0.1:7890 \ 
    torch==1.10.1+cu113 \
    torchvision==0.11.2+cu113 \
    torchaudio==0.10.1+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html

RUN cd pixel-perfect-sfm && \
    pip3 install --proxy=http://127.0.0.1:7890 -r requirements.txt

RUN git clone --recursive https://github.com/cvg/Hierarchical-Localization/
RUN pip3 install --proxy=http://127.0.0.1:7890 --upgrade pip
RUN cd Hierarchical-Localization && \
    pip3 --proxy=http://127.0.0.1:7890 install -e .

RUN cd pixel-perfect-sfm && \
    pip3 install --proxy=http://127.0.0.1:7890 -e .

RUN pip3 install --proxy=http://127.0.0.1:7890 jupyter notebook
RUN pip3 install --proxy=http://127.0.0.1:7890 ipython ipykernel

RUN apt-get install -y wget

RUN echo "use_proxy = on \nhttp_proxy =  http://127.0.0.1:7890/ \nhttps_proxy =  http://127.0.0.1:7890/" >> /etc/wgetrc
WORKDIR "/pixel-perfect-sfm"
CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root"]
```
