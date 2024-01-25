FROM debian:bullseye-slim

ENV ANDROID_BUILD_TOOLS_VERSION=34.0.0

RUN apt-get update && apt-get install -y \
            wget \
            unzip \
            apt-transport-https \ 
            gnupg \
      && rm -rf /var/lib/apt/lists/*

# Install Temurin JDK 17
RUN wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | apt-key add - \
      && echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list \
      && apt-get update \
      && apt-get install -y temurin-17-jdk \
      && rm -rf /var/lib/apt/lists/*

# Install Android SDK-tools (version taken from https://developer.android.com/studio/index.html#command-line-tools-only)
ENV ANDROID_COMMAND_LINE_TOOLS_VERSION="11076708_latest" \
    ANDROID_COMMAND_LINE_TOOLS_SHA256_SUM="2d2d50857e4eb553af5a6dc3ad507a17adf43d115264b1afc116f95c92e5e258" \
    ANDROID_HOME=/opt/android_sdk

RUN cd /tmp \
      && wget "https://dl.google.com/android/repository/commandlinetools-linux-$ANDROID_COMMAND_LINE_TOOLS_VERSION.zip" \
      && echo "$ANDROID_COMMAND_LINE_TOOLS_SHA256_SUM commandlinetools-linux-$ANDROID_COMMAND_LINE_TOOLS_VERSION.zip" | sha256sum -c - \
      && mkdir -p $ANDROID_HOME \
      && unzip "commandlinetools-linux-$ANDROID_COMMAND_LINE_TOOLS_VERSION.zip" -d "$ANDROID_HOME/cmdline-tools" \
      && mv "$ANDROID_HOME/cmdline-tools/cmdline-tools" "$ANDROID_HOME/cmdline-tools/latest" \
      && rm "commandlinetools-linux-$ANDROID_COMMAND_LINE_TOOLS_VERSION.zip"

ENV PATH="$PATH:$ANDROID_HOME/cmdline-tools/latest/bin"

# install build-tools
RUN yes | sdkmanager \
      "build-tools;$ANDROID_BUILD_TOOLS_VERSION" \
      "platform-tools" \
      "extras;android;m2repository" \
      "extras;google;m2repository"

ARG VERSION
ARG BUILD_DATE
ARG VCS_REF

LABEL org.label-schema.version=$VERSION
LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.vcs-ref=$VCS_REF
LABEL org.label-schema.vcs-url="https://github.com/inserteffect/docker-android-build-tools.git"
LABEL org.label-schema.name="Android Build Tools"
LABEL org.label-schema.vendor="InsertEFFECT"
LABEL org.label-schema.schema-version="1.0"
LABEL org.opencontainers.image.authors="insertEFFECT <tobias.langen@insfx.com>"
