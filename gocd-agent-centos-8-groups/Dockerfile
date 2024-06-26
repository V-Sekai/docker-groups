# Copyright 2024 Thoughtworks, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

###############################################################################################
# This file is autogenerated by the repository at https://github.com/gocd/gocd.
# Please file any issues or PRs at https://github.com/gocd/gocd
###############################################################################################

FROM curlimages/curl:latest as gocd-agent-unzip
USER root
ARG TARGETARCH
ARG UID=1000
RUN curl --fail --location --silent --show-error "https://download.gocd.org/binaries/24.1.0-18867/generic/go-agent-24.1.0-18867.zip" > /tmp/go-agent-24.1.0-18867.zip && \
    unzip -q /tmp/go-agent-24.1.0-18867.zip -d / && \
    mkdir -p /go-agent/wrapper /go-agent/bin && \
    mv -v /go-agent-24.1.0/LICENSE /go-agent/LICENSE && \
    mv -v /go-agent-24.1.0/*.md /go-agent && \
    mv -v /go-agent-24.1.0/bin/go-agent /go-agent/bin/go-agent && \
    mv -v /go-agent-24.1.0/lib /go-agent/lib && \
    mv -v /go-agent-24.1.0/logs /go-agent/logs && \
    mv -v /go-agent-24.1.0/run /go-agent/run && \
    mv -v /go-agent-24.1.0/wrapper-config /go-agent/wrapper-config && \
    export WRAPPERARCH=$(if [ "$TARGETARCH" == "arm64" ]; then echo arm-64; else echo x86-64; fi) && \
    mv -v /go-agent-24.1.0/wrapper/wrapper-linux-$WRAPPERARCH* /go-agent/wrapper/ && \
    mv -v /go-agent-24.1.0/wrapper/libwrapper-linux-$WRAPPERARCH* /go-agent/wrapper/ && \
    mv -v /go-agent-24.1.0/wrapper/wrapper.jar /go-agent/wrapper/ && \
    chown -R ${UID}:0 /go-agent && chmod -R g=u /go-agent

FROM rockylinux:8
ARG TARGETARCH

LABEL gocd.version="24.1.0" \
  description="GoCD agent based on rockylinux:8" \
  maintainer="V-Sekai Team - K. S. Ernest (iFire) Lee <ernest.lee@chibifire.com>" \
  url="https://www.gocd.org" \
  gocd.full.version="24.1.0-18867" \
  gocd.git.sha="a60529cdf34a96de310bc05e2041f2ac23014707"

RUN export WRAPPERARCH=$(if [ "$TARGETARCH" == "arm64" ]; then echo arm-64; else echo x86-64; fi) && \
    curl -L -o /usr/local/sbin/tini https://github.com/krallin/tini/releases/download/v0.19.0/tini-static-${WRAPPERARCH}

# force encoding
ENV LANG=en_US.UTF-8 LANGUAGE=en_US:en LC_ALL=en_US.UTF-8
ENV GO_JAVA_HOME="/gocd-jre"

ARG UID=1000
ARG GID=1000

RUN \
# add mode and permissions for files we added above
  chmod 0755 /usr/local/sbin/tini && \
  chown root:root /usr/local/sbin/tini && \
  echo 'fastestmirror=1' >> /etc/dnf/dnf.conf && \
  echo 'install_weak_deps=False' >> /etc/dnf/dnf.conf && \
  dnf upgrade -y && \
  dnf install -y shadow-utils && \
# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
  useradd -l -u ${UID} -g root -d /home/go -m go && \
  dnf install -y git-core openssh-clients bash unzip procps-ng coreutils-single glibc-langpack-en  curl && \
  dnf clean all && \
  rm -rf /var/cache/yum /var/cache/dnf && \
  curl --fail --location --silent --show-error "https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.11%2B9/OpenJDK17U-jre_$(uname -m | sed -e s/86_//g)_linux_hotspot_17.0.11_9.tar.gz" --output /tmp/jre.tar.gz && \
  mkdir -p /gocd-jre && \
  tar -xf /tmp/jre.tar.gz -C /gocd-jre --strip 1 && \
  rm -rf /tmp/jre.tar.gz && \
  mkdir -p /go-agent /docker-entrypoint.d /go /godata

ADD docker-entrypoint.sh /


COPY --from=gocd-agent-unzip /go-agent /go-agent
# ensure that logs are printed to console output
COPY --chown=go:root agent-bootstrapper-logback-include.xml agent-launcher-logback-include.xml agent-logback-include.xml /go-agent/config/

RUN chown -R go:root /docker-entrypoint.d /go /godata /docker-entrypoint.sh && \
    chmod -R g=u /docker-entrypoint.d /go /godata /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

USER root

RUN dnf install -y epel-release dnf-plugins-core && yum config-manager -y --set-enabled powertools && curl https://download.mono-project.com/repo/centos8-stable.repo | tee /etc/yum.repos.d/mono-stable.repo && yum update -y

RUN dnf upgrade -y 

RUN yum group install -y "Development Tools"

RUN yum group install -y "Additional Development"

RUN yum install -y \
    python3-pip \
    mingw64-winpthreads \
    mingw32-winpthreads \
    mingw64-winpthreads-static \
    mingw32-winpthreads-static \
    libstdc++-static \
    mingw64-filesystem \
    mingw32-filesystem \
    bash \
    libX11-devel \
    libXcursor-devel \
    libXrandr-devel \
    libXinerama-devel \
    libXi-devel \
    mesa-libGL-devel \
    alsa-lib-devel \
    pulseaudio-libs-devel \
    freetype-devel \
    openssl-devel \
    libudev-devel \
    mesa-libGLU-devel \
    libpng-devel \
    llvm-devel \
    clang \
    libxml2-devel \
    libuuid-devel \
    patch \
    make \
    git \
    bzip2 \
    xz \
    yasm \
    xorg-x11-server-Xvfb \
    pkgconfig \
    mesa-dri-drivers \
    java-11-openjdk \
    java-11-openjdk-devel \
    ncurses-compat-libs \
    unzip \
    which \
    gcc \
    gcc-c++ \
    libatomic-static \
    libatomic \
    ccache \
    ninja-build \
    speech-dispatcher-devel \
    git-lfs \
    cmake

RUN git clone https://github.com/tpoechtrager/osxcross.git /osxcross
COPY MacOSX14.2.sdk.tar.xz /osxcross/tarballs

RUN cd /osxcross && export UNATTENDED=1 && \
    CLANG_VERSION=16.0.0 ENABLE_CLANG_INSTALL=1 INSTALLPREFIX=target ./build_apple_clang.sh > /dev/null 2>&1

RUN cd /osxcross && export PATH=$PATH:target/bin && INSTALLPREFIX=target UNATTENDED=1 ./build.sh

RUN cd /osxcross && export PATH=$PATH:target/bin && DISABLE_PARALLEL_ARCH_BUILD=1 ./build_compiler_rt.sh

RUN alternatives --set ld /usr/bin/ld.gold && git lfs install && alternatives --set python /usr/bin/python3
RUN pip3 install -U pip && pip3 install -U scons
RUN mkdir /opt/llvm-mingw && curl -L https://github.com/mstorsjo/llvm-mingw/releases/download/20240619/llvm-mingw-20240619-ucrt-ubuntu-20.04-x86_64.tar.xz | tar -Jxf - --strip 1 -C /opt/llvm-mingw
RUN git clone https://github.com/emscripten-core/emsdk /opt/emsdk && /opt/emsdk/emsdk install latest && /opt/emsdk/emsdk activate latest && curl -L -o ispc.tgz 'https://github.com/ispc/ispc/releases/download/v1.15.0/ispc-v1.15.0-linux.tar.gz' && tar -zxf ispc.tgz ispc-v1.15.0-linux/bin/ispc && mv ispc-v1.15.0-linux/bin/ispc /usr/local/bin/ispc && rmdir -p ispc-v1.15.0-linux/bin

RUN cd /usr/local/bin && curl -L -o butler.zip https://broth.itch.zone/butler/linux-amd64/15.21.0/archive/default && unzip butler.zip && rm butler.zip && butler -V && butler -V && cd && butler -V

RUN curl -LO https://go.dev/dl/go1.22.4.linux-amd64.tar.gz && \
    rm -rf /usr/local/go && \
    tar -C /usr/local -xzf go1.22.4.linux-amd64.tar.gz && \
    rm go1.22.4.linux-amd64.tar.gz
    
ENV PATH=$PATH:/usr/local/go/bin

ENV RUSTUP_HOME=/opt/rust CARGO_HOME=/opt/cargo PATH=/opt/cargo/bin:/opt/rust/bin:$PATH
RUN mkdir -p /opt/cargo /opt/rust && chown go /opt/cargo && curl https://sh.rustup.rs -sSf | sh -s -- -y --default-toolchain nightly --no-modify-path && rustup default nightly && rustup target add aarch64-linux-android x86_64-linux-android x86_64-unknown-linux-gnu aarch64-apple-ios x86_64-apple-ios x86_64-apple-darwin aarch64-apple-darwin x86_64-pc-windows-gnu x86_64-pc-windows-msvc wasm32-wasi
RUN cp -p /usr/lib/gcc/x86_64*/*/libatomic.a /usr/lib64/libatomic.a
RUN curl -O https://download.blender.org/release/Blender4.1/blender-4.1.1-linux-x64.tar.xz && \
    tar -xf blender-4.1.1-linux-x64.tar.xz -C /opt/ && \
    rm blender-4.1.1-linux-x64.tar.xz && \
    ln -s /opt/blender-4.1.1-linux-x64/blender /usr/local/bin/blender
USER go
