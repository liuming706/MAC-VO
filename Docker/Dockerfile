FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
#FROM nvidia/cuda:12.5.1-cudnn-devel-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=graphics,utility,compute

# 使用国内镜像加速 apt
RUN sed -i 's|http://.*.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list && \
    apt-get update && apt-get install -y \
    unzip sudo git wget python3-pip \
    ffmpeg libsm6 libxext6 libgtk-3-dev libxkbcommon-x11-0 vulkan-tools && \
    rm -rf /var/lib/apt/lists/*

# 创建非 root 用户
ARG USERNAME=macvo
RUN useradd -ms /bin/bash ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 使用非 root 用户
USER ${USERNAME}
WORKDIR /home/${USERNAME}

# 设置 PATH 和 pip 源
ENV PATH="/home/${USERNAME}/.local/bin:$PATH"
RUN pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip3 install --upgrade pip && \
    pip3 install --no-cache-dir \
        'pypose>=0.6.8' opencv-python-headless evo \
        matplotlib tabulate tqdm rich cupy-cuda12x einops \
        timm==0.9.12 rerun-sdk==0.21.0 yacs \
        numpy pyyaml wandb pillow scipy flow_vis h5py \
        xformers==0.0.27.post2 onnx torchvision

CMD ["/bin/bash"]

