FROM ros:noetic-ros-base-focal

SHELL ["/bin/bash", "-c"]

# set the ros version
ENV ROS_DISTRO noetic
ENV PROJ_HUB project

# install some useful packages and upgrade existing ones
RUN apt update && apt upgrade -y \
    && apt install -y \
    apt-utils \
    git \
    curl \
    nano \
    vim \
    tmux \
    wget \
    x11-apps \
    python3-pip \
    python3-catkin-tools \
    python3-vcstool \
    python-is-python3 \
    ros-${ROS_DISTRO}-cv-bridge \
    libqt5websockets5-dev \
    libasound2-dev \
    libsndfile-dev \
    pkg-config \
    qt5-default


# install ros packages
# see ../docs/installing_ros_packages.md for alternatives
RUN DEBIAN_FRONTEND=noninteractive apt install -y \
    ros-${ROS_DISTRO}-rviz \
    && rm -rf /var/lib/apt/lists/*

RUN rosdep update

# Install pip packages
WORKDIR /usr/src/app
COPY ./requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

# Install torch packages
RUN pip3 install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu117

WORKDIR /${PROJ_HUB}

# setup .bashrc to correctly source workspace and setup user aliases
SHELL ["/bin/bash", "-l", "-c"]
ENV TERM=xterm-256color
RUN echo -e "PS1='\e[1m\e[93m\u\e[97m@\e[93m\h:\e[35m\w\e[0m\e[97m#\e[0m '" >> ~/.bashrc

# copy the entrypoint into the image
COPY ./entrypoint.sh /entrypoint.sh

# run this script on startup
ENTRYPOINT /entrypoint.sh