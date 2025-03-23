FROM ubuntu:20.04

ENV TZ=Asia/Shanghai
ENV DEBIAN_FRONTEND=noninteractive
ENV UBUNTU_FRONTEND=noninteractive

COPY ./service/docker-entrypoint.sh /docker-entrypoint.sh
COPY ./src/ /src/

RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
    && sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
	&& apt update --no-install-recommends\
    && apt install -y openssh-server gcc-10 dwarfdump build-essential unzip wget\
    && chmod +x /docker-entrypoint.sh \
    && mkdir /app \
    && sed -i 's/\#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config \
    && sed -i 's/\#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config \
    && echo 'root:root' | chpasswd \
    && systemctl enable ssh \
    && service ssh start


# https://mirrors.ustc.edu.cn/ubuntu/pool/main/l/linux/
RUN wget https://mirrors.ustc.edu.cn/ubuntu/pool/main/l/linux/linux-image-unsigned-5.4.0-205-generic_5.4.0-205.225_amd64.deb -P /src/ \
    && wget https://mirrors.ustc.edu.cn/ubuntu/pool/main/l/linux/linux-headers-5.4.0-205-generic_5.4.0-205.225_amd64.deb -P /src/ \
    && wget https://mirrors.ustc.edu.cn/ubuntu/pool/main/l/linux/linux-headers-5.4.0-205_5.4.0-205.225_all.deb -P /src/ \
    && wget https://mirrors.ustc.edu.cn/ubuntu/pool/main/l/linux/linux-modules-5.4.0-205-generic_5.4.0-205.225_amd64.deb -P /src/

WORKDIR /src

RUN unzip tool.zip && apt-get update && apt-get install -y kmod linux-base initramfs-tools
RUN dpkg -i linux-headers-5.4.0-205_5.4.0-205.225_all.deb \
    && dpkg -i linux-headers-5.4.0-205-generic_5.4.0-205.225_amd64.deb \
    && dpkg -i linux-modules-5.4.0-205-generic_5.4.0-205.225_amd64.deb \
    && dpkg -i linux-image-unsigned-5.4.0-205-generic_5.4.0-205.225_amd64.deb \
    && apt --fix-broken install -y

RUN ls -la /lib/modules/5.4.0-205-generic/ \
    && mkdir -p /lib/modules/5.4.0-205-amd64/ \
    && ln -sf /usr/src/linux-headers-5.4.0-205-generic /lib/modules/5.4.0-205-amd64/build


WORKDIR /src/linux

RUN echo 'MODULE_LICENSE("GPL");' >> module.c && \
    sed -i 's/$(shell uname -r)/5.4.0-205-generic/g' Makefile && \
    make && \
    mv module.dwarf /app && \
    cp /boot/System.map* /app

WORKDIR /app

RUN zip ubuntu-5.4.0-205-generic.zip *

CMD ["/bin/bash"]
