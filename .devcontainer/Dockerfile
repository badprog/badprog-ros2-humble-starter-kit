# badprog.com
# Image ROS 2 Humble
FROM osrf/ros:humble-desktop-full

# Variables only available for this Dockerfile
ARG USERNAME=badprog
ARG WORKDIR_NAME=/workspaces

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
RUN echo "source /opt/ros/humble/setup.bash" >> /home/$USERNAME/.bashrc
