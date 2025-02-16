# 使用 Ubuntu 20.04 ROS 
FROM docker.1ms.run/osrf/ros:noetic-desktop-full

# 设置时区和更新镜像源
RUN apt-get update && apt-get install -y tzdata \
    && ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata



# 安装一些基础库
RUN apt-get update && apt-get install -y \
    git \
    vim \
    cmake \
    wget \
    zip \ 
    libgflags-dev \
    libgoogle-glog-dev \
    libssl-dev\
    build-essential \
    libgl1-mesa-dev \
    libglew-dev \
    libepoxy-dev \
    libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev \
    libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev \ 
    libboost-all-dev \
    && apt-get clean

# 创建目录 WORKDIR/code 并在目录下面git clone 代码
RUN mkdir -p /home/catkin_ws/src
RUN cd /home/catkin_ws/src && git clone https://github.com/ov2slam/ov2slam.git


RUN cd /home/ && git clone https://github.com/laurentkneip/opengv && cd opengv && mkdir build && cd build && cmake .. && make -j$(nproc) && make install

# Eigen3 > 3.1.0
RUN apt-get update && apt-get install -y libeigen3-dev

# OpenCV 3.4.0
# 下载 OpenCV 3.4.16
RUN cd /home/ && \
    git clone https://github.com/opencv/opencv.git -b 3.4 --depth 1 

# 下载 opencv_contrib 3.4.16
RUN cd /home/ && \
    git clone https://github.com/opencv/opencv_contrib.git -b 3.4 --depth 1 


RUN cd /home/opencv && mkdir build && cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D OPENCV_EXTRA_MODULES_PATH=/home/opencv_contrib/modules \
          -D BUILD_EXAMPLES=ON \
          -D BUILD_opencv_world=ON \
          -D WITH_CUDA=OFF \
          -D WITH_GTK=ON \
          -D WITH_OPENGL=ON \
          -D OPENCV_ENABLE_NONFREE=ON .. && \
    make -j$(nproc) && \
    make install

RUN ldconfig

RUN cd /home/catkin_ws/src/ov2slam && ./build_thirdparty.sh