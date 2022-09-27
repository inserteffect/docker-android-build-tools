FROM openjdk:11-jdk-slim-bullseye

ENV ANDROID_BUILD_TOOLS_VERSION=31.0.0

RUN apt-get update && apt-get install -y --no-install-recommends \
            wget \
            unzip \
      && rm -rf /var/lib/apt/lists/*

# Install Android SDK-tools
ENV ANDROID_COMMAND_LINE_TOOLS_VERSION="7583922_latest" \
    ANDROID_COMMAND_LINE_TOOLS_SHA256_SUM="124f2d5115eee365df6cf3228ffbca6fc3911d16f8025bebd5b1c6e2fcfa7faf" \
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
RUN yes | sdkmanager "build-tools;$ANDROID_BUILD_TOOLS_VERSION"
RUN yes | sdkmanager "platform-tools"

RUN yes | sdkmanager "extras;android;m2repository"
RUN yes | sdkmanager "extras;google;m2repository"

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
LABEL org.opencontainers.image.authors="insertEFFECT <philip.graf@insfx.com>"
