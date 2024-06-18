ARG UBUNTU_VERSION=22.04
FROM ghcr.io/matt9ucci/vscode/devcontainers/ubuntu:$UBUNTU_VERSION

ARG DEBIAN_FRONTEND=noninteractive

RUN set -x \
 && apt-get -qq update \
 && apt-get -qq -o Dpkg::Use-Pty=0 install --no-install-recommends \
 gcc \	
 bzip2 \  
	cmake \
	make \
	python3 \
	python-is-python3\
	xz-utils \
&& rm -rf /var/lib/apt/lists/*

ARG USER_NAME=vscode
USER $USER_NAME

# Install emsdk: https://emscripten.org/docs/getting_started/downloads.html
ARG EMSDK=/home/$USER_NAME/emsdk
ARG EMSDK_VERSION=latest
RUN set -ux \
 && git clone --depth 1 https://github.com/emscripten-core/emsdk.git $EMSDK \
 && $EMSDK/emsdk install $EMSDK_VERSION \
 && $EMSDK/emsdk activate $EMSDK_VERSION \
 && echo "source $(realpath $EMSDK/emsdk_env.sh) > /dev/null 2>&1" | sudo tee -a /etc/bash.bashrc > /dev/null

# OCI labels: https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL \
org.opencontainers.image.authors='Matt Gucci' \
org.opencontainers.image.description='Emsdk and tools for Emscripten development inside VS Code devcontainer.' \
org.opencontainers.image.licenses=MIT \
org.opencontainers.image.title='Emscripten with VSCode'
# FROM mcr.microsoft.com/devcontainers/cpp:0-debian-11

# ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="none"

# # Optionally install the cmake for vcpkg
# COPY ./reinstall-cmake.sh /tmp/

# RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
#         chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
#     fi \
#     && rm -f /tmp/reinstall-cmake.sh

# # [Optional] Uncomment this section to install additional vcpkg ports.
# # RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# # [Optional] Uncomment this section to install additional packages.
# # RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
# #     && apt-get -y install --no-install-recommends <your-package-list-here>