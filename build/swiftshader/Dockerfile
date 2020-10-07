# Build the image:
#   docker build --no-cache --tag ssfilament -f build/swiftshader/Dockerfile .
#   docker tag ssfilament ghcr.io/filament-assets/swiftshader
#
# Publish the image:
#   docker login ghcr.io --username <user> --password <token>
#   docker push ghcr.io/filament-assets/swiftshader
#
# Run the image and mount the current directory:
#   docker run -it -v `pwd`:/trees/branch -t ssfilament

FROM ubuntu:focal
WORKDIR /trees
ARG DEBIAN_FRONTEND=noninteractive
ENV SWIFTSHADER_LD_LIBRARY_PATH=/trees/swiftshader/build
ENV CXXFLAGS='-fno-builtin -Wno-pass-failed'

RUN apt-get update && \
    apt-get --no-install-recommends install -y \
	apt-transport-https \
	apt-utils \
	build-essential \
	cmake \
	ca-certificates \
	git \
	ninja-build \
	python \
	python3 \
	xorg-dev \
	clang-7 \
	libc++-7-dev \
	libc++abi-7-dev \
	imagemagick \
	perceptualdiff

# Ensure that clang is used instead of gcc.
RUN set -eux ;\
    update-alternatives --install /usr/bin/clang clang /usr/bin/clang-7 100 ;\
    update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-7 100 ;\
    update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100 ;\
    update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++ 100

# Clone and build SwiftShader.
RUN set -eux ;\
	git clone https://swiftshader.googlesource.com/SwiftShader swiftshader ;\
    cd swiftshader ;\
    cd build ;\
    cmake .. -GNinja -DCMAKE_BUILD_TYPE=Release ;\
    ninja
