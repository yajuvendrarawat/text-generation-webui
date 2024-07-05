# BUILDER
FROM registry.access.redhat.com/ubi9/python-311
# Base
USER 0
WORKDIR /builder
ARG TORCH_CUDA_ARCH_LIST="${TORCH_CUDA_ARCH_LIST:-3.5;5.0;6.0;6.1;7.0;7.5;8.0;8.6+PTX}"
ARG BUILD_EXTENSIONS="${BUILD_EXTENSIONS:-}"
ARG APP_UID="${APP_UID:-6972}"
ARG APP_GID="${APP_GID:-6972}"

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    yum upgrade -y && \
    yum install -y git vim gcc gcc-c++ kernel-devel make python3-dev pip bash curl && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /opt/app-root/src
RUN git clone https://github.com/yajuvendrarawat/text-generation-webui.git 
WORKDIR /opt/app-root/srctext-generation-webui
RUN GPU_CHOICE=A USE_CUDA118=FALSE LAUNCH_AFTER_INSTALL=FALSE INSTALL_EXTENSIONS=TRUE ./start_linux.sh --verbose
COPY CMD_FLAGS.txt /home/app/text-generation-webui/
EXPOSE ${CONTAINER_PORT:-7860} ${CONTAINER_API_PORT:-5000} ${CONTAINER_API_STREAM_PORT:-5005}
WORKDIR /opt/app-root/srctext-generation-webui
# set umask to ensure group read / write at runtime
CMD umask 0002 && export HOME=/opt/app-root/srctext-generation-webui && ./start_linux.sh --listen
