FROM docker.m.daocloud.io/library/node:slim
 
# 更新源
# RUN sed -i "s|deb.debian.org|mirrors.ustc.edu.cn|g" /etc/apt/sources.list.d/debian.sources \
# && sed -i "s|http://mirrors.ustc.edu.cn|https://mirrors.ustc.edu.cn|g" /etc/apt/sources.list.d/debian.sources \
# && apt update \
RUN apt update \
 && apt full-upgrade -y

# 安装依赖
RUN apt install -y \
    ca-certificates \
    curl \
    git \
    redis-server \
    chromium \
    fonts-lxgw-wenkai \
    fonts-noto-color-emoji \
 && apt autoremove --purge \
 && apt clean


# 配置 pnpm 源并安装 pnpm
RUN npm install -g pnpm 
#RUN npm install -g pnpm --registry "https://registry.npmmirror.com"

# 设置 Git 安全目录
RUN git config --global --add safe.directory "*"

# 克隆仓库（如果 .git 不存在或为空）
WORKDIR /root/Yunzai
RUN if [ ! -s .git ]; then \
      git clone --depth 1 --single-branch "https://gitee.com/TimeRainStarSky/Yunzai" .; \
    fi

# 安装依赖
RUN pnpm install --force

# 创建启动脚本
RUN echo 'exec node . start' > /start && chmod +x /start

# 健康检查
HEALTHCHECK CMD curl -s http://localhost:2536/status || exit 1

# 设置工作目录和暴露端口
WORKDIR /root/Yunzai
EXPOSE 2536

# 默认启动命令
CMD ["sh", "/start"]
