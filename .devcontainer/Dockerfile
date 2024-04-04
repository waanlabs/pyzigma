# Use Ubuntu latest as the base image
FROM ubuntu:22.04

# Dockblock
LABEL maintainer="waan<admin@waan.email>"
LABEL description="Ubuntu Dev Container"
LABEL website="https://waan.io"
LABEL upstream="https://github.com/waanlabs/pyzigma"
LABEL version="1.0.0"
LABEL license="MIT"

# Custom settings
ARG USER=waan
ARG UID=1000
ARG GID=1000
ARG PROJECT=pyzigma

# Set non-interactive to avoid prompts during build
ENV DEBIAN_FRONTEND=noninteractive

# Set the timezone to EST (Eastern Standard Time)
ENV TZ=EST

# Update package lists and install sudo with minimal additional packagessou
# Clean up in the RUN statement to keep the image size down
RUN apt-get update && \
    apt-get install -y sudo \
    software-properties-common \
    git \
    python3-pip \
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

# Switch to new user in the docker image
USER ${USER}

# Set path for Python and pip
ENV PATH="/home/${USER}/.local/bin:${PATH}"

# Set the working directory
WORKDIR /home/${USER}/${PROJECT}

ENV PIPENV_VERBOSITY=-1

RUN sudo apt-get autoremove -y \
    && sudo apt-get clean \
    && sudo rm -rf /var/lib/apt/lists/*

# Define the default command or entry point
ENTRYPOINT ["/bin/bash"]
