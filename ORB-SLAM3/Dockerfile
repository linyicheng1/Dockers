# 使用 Ubuntu 20.04 作为基础镜像
FROM docker.1ms.run/ubuntu:20.04

# 设置时区和更新镜像源
RUN apt-get update && apt-get install -y tzdata \
    && ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata

# 安装 
RUN apt-get update && apt-get install -y \
    git \
    vim \
    cmake \
    wget \
    zip \ 
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
RUN mkdir -p /home/code
RUN cd /home/code && git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git
# 使用C++14 编译
RUN cd /home/code/ORB_SLAM3 && sed -i 's/++11/++14/g' CMakeLists.txt 

# 1. Eigen3 > 3.1.0
RUN apt-get update && apt-get install -y libeigen3-dev

# 2. python2.7  python3 
RUN apt-get update && apt-get install -y python2.7-dev python3-dev python3-numpy

# 安装环境
# 3. Pangolin
RUN  cd /home/code/ORB_SLAM3/Thirdparty && git clone https://github.com/stevenlovegrove/Pangolin.git
RUN cd /home/code/ORB_SLAM3/Thirdparty/Pangolin && mkdir build && cd build && cmake .. && make -j4 && make install

# 4. OpenCV 4.4.0
RUN cd /home/code/ORB_SLAM3/Thirdparty && wget -O opencv.zip https://github.com/opencv/opencv/archive/4.4.0.zip && unzip opencv.zip
RUN cd /home/code/ORB_SLAM3/Thirdparty/opencv-4.4.0 && mkdir build && cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local D BUILD_EXAMPLES=ON .. && make -j4 && make install

RUN ldconfig




