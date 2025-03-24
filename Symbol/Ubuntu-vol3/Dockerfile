FROM ubuntu:20.04

WORKDIR /volatility

# 安装必要的工具
RUN apt-get update && apt-get install -y wget dpkg python3 zip unzip
    #rm -rf /var/lib/apt/lists/*

# 下载dwarf2json工具
RUN wget https://github.com/volatilityfoundation/dwarf2json/releases/download/v0.9.0/dwarf2json-linux-amd64 \
    && chmod +x dwarf2json-linux-amd64

# 下载Ubuntu 20.04内核调试符号
RUN wget https://launchpadlibrarian.net/769817990/linux-image-unsigned-5.4.0-205-generic-dbgsym_5.4.0-205.225_amd64.ddeb

# 安装内核调试符号
RUN dpkg -i linux-image-unsigned-5.4.0-205-generic-dbgsym_5.4.0-205.225_amd64.ddeb

# 生成符号表JSON文件
RUN ./dwarf2json-linux-amd64 linux --elf /usr/lib/debug/boot/vmlinux-5.4.0-205-generic > linux-image-5.4.0-205-generic.json

RUN zip linux-image-5.4.0-205-generic.zip linux-image-5.4.0-205-generic.json

# 设置容器启动时的默认命令
CMD ["/bin/bash"]
