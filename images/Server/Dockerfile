FROM ubuntu:focal

MAINTAINER Ian Torres <iantorres@outlook.com>

LABEL maintainer="Ian Torres <iantorres@outlook.com>" \
        version="1.0"

ENV DEBIAN_FRONTEND=noninteractive \
    # BUILD
    BUILD_DEPENDENCIES="build-essential ninja-build cmake libmysqlcppconn-dev uuid-dev git p7zip-full wget zlib1g-dev libssl-dev libcurl4-gnutls-dev curl zip unzip tar pkg-config libxinerama-dev libxcursor-dev xorg-dev libglu1-mesa-dev" \
    # BOOST
    BOOST_VERSION_MAJOR=1 \
    BOOST_VERSION_MINOR=79 \
    BOOST_VERSION_PATCH=0

RUN apt-get -qq update \
    && apt-get -qq install -y ${BUILD_DEPENDENCIES} \
    && echo "Build Dependencies Installed Successfully."

RUN cd / \
    && git clone https://github.com/google/googletest.git \
    && cd /googletest \
    && cmake . \
    && make \
    && make install \
    && cd / \
    && rm -rf /googletest \
    && echo "Google Test Installed Successfully."

RUN cd / \
    && wget -q https://boostorg.jfrog.io/artifactory/main/release/${BOOST_VERSION_MAJOR}.${BOOST_VERSION_MINOR}.${BOOST_VERSION_PATCH}/source/boost_${BOOST_VERSION_MAJOR}_${BOOST_VERSION_MINOR}_${BOOST_VERSION_PATCH}.tar.gz \
    && tar -xzf boost_${BOOST_VERSION_MAJOR}_${BOOST_VERSION_MINOR}_${BOOST_VERSION_PATCH}.tar.gz \
    && cd boost_${BOOST_VERSION_MAJOR}_${BOOST_VERSION_MINOR}_${BOOST_VERSION_PATCH} \
    && ./bootstrap.sh \
    && ./b2 install -j 8 \
    && cd / \
    && rm -rf boost_${BOOST_VERSION_MAJOR}_${BOOST_VERSION_MINOR}_${BOOST_VERSION_PATCH} \
    && rm boost_${BOOST_VERSION_MAJOR}_${BOOST_VERSION_MINOR}_${BOOST_VERSION_PATCH}.tar.gz \
    && echo "Boost Library Installed Successfully."

RUN cd / \
    && git clone https://github.com/microsoft/vcpkg \
    && cd vcpkg \
    && ./bootstrap-vcpkg.sh \
    && ./vcpkg install "aws-sdk-cpp[sqs, lambda]" sentry-native imgui cpp-redis glfw3 --recurse --disable-metrics

RUN echo "Server Built Successfully."
