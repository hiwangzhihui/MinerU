# 基于 sglang 镜像，这是 MinerU 项目推荐的基础镜像
FROM swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/lmsysorg/sglang:v0.4.7-cu124

# 安装开发工具和系统依赖
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    vim \
    nano \
    htop \
    tree \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# 安装 uv 工具
RUN pip install --break-system-packages uv

# 创建非 root 用户
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update \
    && apt-get install -y sudo \
    && groupadd -g $USER_GID $USERNAME 2>/dev/null || groupmod -n $USERNAME $(getent group $USER_GID | cut -d: -f1) \
    && useradd -m -s /bin/bash -u $USER_UID -g $USER_GID $USERNAME 2>/dev/null || usermod -l $USERNAME -s /bin/bash $(getent passwd $USER_UID | cut -d: -f1) \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# 设置工作目录
WORKDIR /workspace

# 复制项目文件
COPY pyproject.toml ./
COPY mineru/ ./mineru/
COPY mineru.template.json ./

# 安装开发工具
RUN pip install --break-system-packages \
    black \
    flake8 \
    isort \
    pylint \
    pytest \
    pytest-cov \
    jupyter \
    ipykernel \
    matplotlib \
    seaborn \
    pandas \
    numpy

# 创建缓存目录并设置权限
RUN mkdir -p /home/$USERNAME/.cache/mineru && \
    chown -R $USERNAME:$USERNAME /home/$USERNAME/.cache && \
    chown -R $USERNAME:$USERNAME /workspace

# 设置用户
USER $USERNAME

# 设置环境变量
ENV MINERU_MODEL_SOURCE=local
ENV PYTHONPATH=/workspace
ENV CUDA_VISIBLE_DEVICES=0

# 创建模型缓存目录
RUN mkdir -p /home/$USERNAME/.cache/mineru

# 默认命令
CMD ["/bin/bash"] 
