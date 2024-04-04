# Use Ubuntu latest as the base image.
FROM ubuntu:22.04

# Dockblock.
LABEL maintainer="waan<admin@waan.email>"
LABEL description="Ubuntu Dev Container"
LABEL website="https://waan.io"
LABEL upstream="https://github.com/waanlabs/pyzigma"
LABEL version="1.0.0"
LABEL license="MIT"

# Custom settings.
ARG USER=waan
ARG UID=1000
ARG GID=1000
ARG PROJECT=pyzigma

# Set non-interactive to avoid prompts during build.
ENV DEBIAN_FRONTEND=noninteractive

# Set the timezone to EST (Eastern Standard Time)
ENV TZ=EST

# Update package lists and install sudo.
RUN apt-get update && \
    apt-get install -y sudo

# Install common tools.
RUN apt-get install -y software-properties-common \
    git \
    curl

# Set default pipenv verbosity to -1 to reduce noise in logs 
ENV PIPENV_VERBOSITY=-1

# Install Python packages.
RUN apt-get install -y python3-pip \
    python3-venv && \
    pip install --upgrade pip && \
    pip install pipenv \
    poetry

# Create a non-root user, add to sudo group, and configure sudoers
# Using useradd with explicit user/group IDs for consistency
RUN groupadd -g ${GID} ${USER} && \
    useradd -m -s /bin/bash -u ${UID} -g ${GID} ${USER} && \
    echo "${USER} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USER} && \
    chmod 0440 /etc/sudoers.d/${USER}

# Switch to new sudo user
USER ${USER}

# Set path for Python and pip
ENV PATH="/home/${USER}/.local/bin:${PATH}"

# Set the working directory
WORKDIR /home/${USER}/${PROJECT}

# Set the default shell to bash rather than sh
SHELL ["/bin/bash", "-c"]

# Clean up in the RUN statements to keep the image size down.
RUN sudo apt-get autoremove -y \
    && sudo apt-get clean \
    && sudo rm -rf /var/lib/apt/lists/*

# Define the default entry point.dd
ENTRYPOINT ["/bin/bash"]
