FROM docker.io/lyuma/gocd-agent-fedora-32:v20.5.0
USER root
RUN cd /usr/local/bin && curl -L -o butler.zip https://broth.itch.ovh/butler/linux-amd64/LATEST/archive/default && unzip butler.zip && rm butler.zip && butler -V && butler -V && cd && butler -V
RUN yum upgrade -y && yum group install -y "Development Tools" && yum group install -y "Development Libraries" && yum install -y git-lfs automake autoconf libtool yasm wine mono-devel cmake python3-scons clang glibc-devel.i686 libgcc.i686 libstdc++.i686 mingw64-gcc-c++ mingw32-gcc mingw32-gcc-c++ python3-pip mingw64-winpthreads mingw32-winpthreads mingw64-winpthreads-static mingw32-winpthreads-static libstdc++-static mingw64-filesystem mingw32-filesystem bash libX11-devel libXcursor-devel libXrandr-devel libXinerama-devel libXi-devel mesa-libGL-devel alsa-lib-devel pulseaudio-libs-devel freetype-devel openssl-devel libudev-devel mesa-libGLU-devel libpng-devel xar-devel llvm-devel clang llvm-devel libxml2-devel libuuid-devel openssl-devel bash patch libstdc++-static make git bzip2 xz java-openjdk yasm xorg-x11-server-Xvfb pkgconfig mesa-dri-drivers java-1.8.0-openjdk-devel ncurses-compat-libs unzip which gcc gcc-c++
RUN alternatives --set ld /usr/bin/ld.gold && git lfs install
RUN curl -L https://github.com/mstorsjo/llvm-mingw/releases/download/20200325/llvm-mingw-20200325-ubuntu-18.04.tar.xz | tar -Jxf - --strip 1 -C /opt/llvm-mingw
RUN git clone https://github.com/emscripten-core/emsdk /opt/emsdk && /opt/emsdk/emsdk install latest
# https://dl.google.com/android/repository/commandlinetools-linux-6514223_latest.zip
RUN curl -LO https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip && unzip sdk-tools-linux-4333796.zip -d /opt/android && rm sdk-tools-linux-4333796.zip && yes | /opt/android/tools/bin/sdkmanager --licenses && /opt/android/tools/bin/sdkmanager ndk-bundle 'platforms;android-23' 'build-tools;19.1.0' 'build-tools;28.0.3' 'platforms;android-28'
USER go
RUN git config --global url.git@gitlab.com:.insteadOf https://gitlab.com/
