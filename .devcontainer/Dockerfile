# Base: clean Ubuntu 24.04
FROM ubuntu:24.04

# Set timezone (optional)
ENV TZ=Etc/UTC

# Install base tools + Docker CLI + daemon
RUN apt-get update && apt-get install -y \
    sudo \
    curl \
    git \
    wget \
    ca-certificates \
    tzdata \
    gnupg \
    lsb-release && \
    # Install Docker (from official Docker repo)
    mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
      | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && apt-get install -y \
      docker-ce \
      docker-ce-cli \
      containerd.io \
      docker-buildx-plugin \
      docker-compose-plugin && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create workspace
WORKDIR /workspace

# Default user
USER root