FROM crpi-hdoi26uk0jhh8oxv.cn-shenzhen.personal.cr.aliyuncs.com/fund_tlx/python:3.13-slim

WORKDIR /app

# 使用阿里云镜像源
RUN sed -i 's|deb.debian.org|mirrors.aliyun.com|g' /etc/apt/sources.list.d/debian.sources

# 安装系统依赖
RUN apt-get update && apt-get install -y \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# 复制依赖文件
COPY requirements.txt .

# 安装 Python 依赖（使用阿里云 PyPI 镜像）
RUN pip install --no-cache-dir -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com -r requirements.txt

# 复制项目文件
COPY . .

# 复制并设置 entrypoint
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# 创建配置目录（用于挂载 volume）
RUN mkdir -p /app/config

# 暴露端口
EXPOSE 8000

# 入口脚本
ENTRYPOINT ["/entrypoint.sh"]

# 默认启动命令
CMD ["gunicorn", "fundval.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "4", "--access-logfile", "-", "--error-logfile", "-"]
