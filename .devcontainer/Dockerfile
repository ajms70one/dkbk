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
    unzip \
    ca-certificates \
    tzdata \
    gnupg \
    lsb-release \
    openjdk-17-jdk \
    && mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
      | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && apt-get install -y \
      docker-ce \
      docker-ce-cli \
      containerd.io \
      docker-buildx-plugin \
      docker-compose-plugin \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# -----------------------------
# 1️⃣ Set JAVA_HOME
# -----------------------------
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH

# -----------------------------
# 2️⃣ Install Android SDK
# -----------------------------
ENV ANDROID_SDK_ROOT=/opt/android-sdk
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    cd $ANDROID_SDK_ROOT/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-1092546_latest.zip -O cmdline-tools.zip && \
    unzip cmdline-tools.zip && rm cmdline-tools.zip && \
    mv cmdline-tools latest

# Add SDK tools to PATH
ENV PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH

# Accept licenses and install essential packages
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# -----------------------------
# 3️⃣ Install Gradle (global)
# -----------------------------
ENV GRADLE_VERSION=8.3
RUN wget https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -O /tmp/gradle.zip && \
    unzip /tmp/gradle.zip -d /opt/gradle && rm /tmp/gradle.zip
ENV PATH=/opt/gradle/gradle-${GRADLE_VERSION}/bin:$PATH

# -----------------------------
# 4️⃣ Workspace
# -----------------------------
WORKDIR /workspace
USER root

# Default entrypoint
CMD ["/bin/bash"]
