# Use Ubuntu as the base image
FROM ubuntu:22.04

# Set environment variables
ENV FLUTTER_HOME=/opt/flutter
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV PATH="${FLUTTER_HOME}/bin:${FLUTTER_HOME}/bin/cache/dart-sdk/bin:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools:${PATH}"

# Install necessary dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    git \
    unzip \
    xz-utils \
    libglu1-mesa \
    ca-certificates \
    openjdk-11-jdk && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Download and install Flutter
RUN curl -L https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.13.0-stable.tar.xz \
    | tar -xJf - -C /opt

# Mark /opt/flutter as a safe directory for Git
RUN git config --global --add safe.directory /opt/flutter

# Download and install Android SDK Command-line tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    curl -o sdk-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip && \
    unzip sdk-tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm sdk-tools.zip

# Accept licenses and install Android platform packages
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.0"

# Run Flutter doctor to set up Flutter and Android
RUN flutter config --no-analytics && \
    flutter doctor --android-licenses && \
    flutter doctor

# Set the working directory
WORKDIR /app

# Create a new Flutter project if it doesn't already exist in the /app directory
RUN if [ ! -f "/app/pubspec.yaml" ]; then flutter create .; fi

# Expose the ADB port to allow for device connection
EXPOSE 5037

# Default command to start ADB and run Flutter on an Android device
CMD ["bash", "-c", "adb start-server && flutter run --device-id=101182538M109189"]
