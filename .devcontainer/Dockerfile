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

# Set the timezone to EST (Eastern Standard Time).
ENV TZ=EST


# Update package lists and install sudo.
RUN apt-get update && \ 
    apt-get install -y sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install common tools.
RUN apt-get update && \
    apt-get install -y software-properties-common \
    build-essential \
    git \
    curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set default pipenv verbosity to -1 to reduce noise in logs.
ENV PIPENV_VERBOSITY=-1

# Install Python packages.
RUN apt-get update && \
    apt-get install -y python3-pip \
    python3-venv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create a non-root user, add to sudo group, and configure sudoers.
# Using useradd with explicit user/group IDs for consistency.
RUN groupadd -g ${GID} ${USER} && \
    useradd -m -s /bin/bash -u ${UID} -g ${GID} ${USER} && \
    echo "${USER} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USER} && \
    chmod 0440 /etc/sudoers.d/${USER}

# Switch to new sudo user.
USER ${USER}

# Install pipenv & poetry as non-root user.
RUN pip install --upgrade pip && \
    pip install pipenv poetry

# Install Rust.
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# Set path for Python and pip.
ENV PATH="/home/${USER}/.local/bin:${PATH}"

# Set the working directory.
WORKDIR /home/${USER}/${PROJECT}

# Set the default shell to bash.
SHELL ["/bin/bash", "-c"]

# Define the default entry point.
ENTRYPOINT ["/bin/bash"]
