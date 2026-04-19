# badprog.com
ARG ROS2_HUB=ros
ARG ROS2_DISTRO=humble
ARG ROS2_VARIANT=ros-base
ARG DOCKER_IMAGE=$ROS2_HUB:$ROS2_DISTRO-$ROS2_VARIANT

# Image 
FROM $DOCKER_IMAGE

# Import the previous ARG because the FROM instruction erased them
ARG DOCKER_IMAGE

# Env to get from the container
ENV FROM_IMAGE=$DOCKER_IMAGE

# Variables only available for this Dockerfile
ARG USERNAME=badprog
ARG WORKDIR_NAME=/workspaces/ros2_ws
ARG FILE_INSTALL_SETUP_BASH=$WORKDIR_NAME/install/setup.bash
ARG FILE_OPT_SETUP_BASH=/opt/ros/humble/setup.bash
ARG FILE_USER_BASHRC=/home/$USERNAME/.bashrc

# Update the tool list then delete it once toos installed (save space)
RUN apt-get update && apt-get install -y \
    tree \
    && rm -rf /var/lib/apt/lists/*

# Creation of the user 'badprog'
RUN useradd -m $USERNAME && \
    # same rights as root but needs sudo to do it
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    # the file is protected
    chmod 0440 /etc/sudoers.d/$USERNAME

# Create the directory and add rights to the 'badprog' user
RUN mkdir -p $WORKDIR_NAME && chown $USERNAME:$USERNAME $WORKDIR_NAME

# Specify the user when the build is finished
USER $USERNAME

# From where the following commands will be launched
WORKDIR $WORKDIR_NAME

# Automatically source ROS 2 environment for every new shell session
RUN echo "source $FILE_OPT_SETUP_BASH" >> $FILE_USER_BASHRC
RUN echo "if [ -f $FILE_INSTALL_SETUP_BASH ]; then source $FILE_INSTALL_SETUP_BASH; fi" >> $FILE_USER_BASHRC 
