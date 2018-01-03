FROM java:8
MAINTAINER c4software [at] gmail [dot] com

ENV DEBIAN_FRONTEND=noninteractive \
    ANDROID_HOME=/opt/android-sdk-linux \
    NPM_VERSION=5.5.1 \
    CORDOVA_VERSION=7.1.0 \
    YARN_VERSION=0.24.6 \
    # Fix for the issue with Selenium, as described here:
    # https://github.com/SeleniumHQ/docker-selenium/issues/87
    DBUS_SESSION_BUS_ADDRESS=/dev/null

# Install basics
RUN apt-get update &&  \
    apt-get install -y git wget curl unzip ruby ruby-dev build-essential && \
    curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
    apt-get update &&  \
    apt-get install -y nodejs && \
    npm install -g npm@"$NPM_VERSION" cordova@"$CORDOVA_VERSION" yarn@"$YARN_VERSION" && \
    mkdir -p /root/.cache/yarn/ && \

# Install sass scss_lint
    gem install sass scss_lint && \

# System libs for android enviroment
    echo ANDROID_HOME="${ANDROID_HOME}" >> /etc/environment && \
    dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --force-yes expect ant wget libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1 qemu-kvm kmod && \
    apt-get clean && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \

# Install Android Tools
    mkdir  /opt/android-sdk-linux && cd /opt/android-sdk-linux && \
    wget --output-document=android-tools-sdk.zip --quiet https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip && \
    unzip -q android-tools-sdk.zip && \
    rm -f android-tools-sdk.zip && \
    chown -R root. /opt

# Setup environment
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install Android SDK
RUN yes Y | ${ANDROID_HOME}/tools/bin/sdkmanager --licenses
RUN cordova telemetry off

RUN ${ANDROID_HOME}/tools/bin/sdkmanager "platform-tools"

# Installation de Graddle
RUN wget https://services.gradle.org/distributions/gradle-4.1-bin.zip && \
    mkdir /opt/gradle && \
    unzip -d /opt/gradle gradle-4.1-bin.zip && \
    ln -s /opt/gradle/gradle-4.1/bin/gradle /usr/bin/gradle && \
    rm gradle-4.1-bin.zip

RUN yes Y | ${ANDROID_HOME}/tools/bin/sdkmanager "platforms;android-26" \
    "platforms;android-25" \
    "platforms;android-24" \
    "build-tools;26.0.2" \
    "build-tools;26.0.1" \
    "build-tools;25.0.3" \
    "build-tools;24.0.3" \
    "extras;android;m2repository" \
    "extras;google;m2repository" \
    "extras;google;google_play_services" \
    "add-ons;addon-google_apis-google-23" \
    "add-ons;addon-google_apis-google-22" \
    "add-ons;addon-google_apis-google-21"

# Ajout SSH Client (en cas de submodule)
RUN apt-get install -y openssh-client

RUN mkdir Sources
WORKDIR Sources
CMD []
