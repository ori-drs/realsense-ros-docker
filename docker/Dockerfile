ARG UBUNTU_DISTRO=20.04
ARG ROS_DISTRO=noetic
##############
# Base image #
##############
FROM ros:${ROS_DISTRO}-perception as base

ARG CATKIN_WORKSPACE_DIR="/catkin_ws"

WORKDIR ${CATKIN_WORKSPACE_DIR}
SHELL ["/bin/bash", "-c"]

ARG DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=${ROS_DISTRO}

RUN apt-get update \
   && apt-get install -y \
   build-essential \
   cmake \
   git-all \
   software-properties-common \
   && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
   && apt-get install -y \
   ros-${ROS_DISTRO}-librealsense2* \
   ros-${ROS_DISTRO}-realsense2-* \
   && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
   && apt-get install -y \
   ros-${ROS_DISTRO}-rviz \
   && rm -rf /var/lib/apt/lists/*

ARG DEBIAN_FRONTEND=dialog

###################
# Developer image #
###################
FROM base as dev

ARG USERNAME="developer"
ARG UID=1000
ARG GID=1000

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
   && apt-get install -y sudo \
   && rm -rf /var/lib/apt/lists/* \
   && addgroup --gid ${GID} ${USERNAME} \
   && adduser --disabled-password --gecos '' --uid ${GID} --gid ${GID} ${USERNAME} \
   && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} \
   && chown -R ${UID}:${GID} /home/${USERNAME} \
   && chown -R ${UID}:${GID} ${CATKIN_WORKSPACE_DIR}

RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /home/${USERNAME}/.bashrc

ENV DEBIAN_FRONTEND=dialog

USER ${USERNAME}