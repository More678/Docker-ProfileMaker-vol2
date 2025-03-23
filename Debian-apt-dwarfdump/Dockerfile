FROM debian:11.8

ENV TZ=Asia/Shanghai
ENV DEBIAN_FRONTEND=noninteractive
ENV UBUNTU_FRONTEND=noninteractive

COPY ./service/docker-entrypoint.sh /docker-entrypoint.sh
COPY ./src/ /src/

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && apt update --no-install-recommends \
    && apt install -y openssh-server linux-kbuild-5.10 gcc-10 dwarfdump build-essential unzip zip wget linux-compiler-gcc-10-x86 \
    && apt --fix-broken install -y \
    && chmod +x /docker-entrypoint.sh \
    && mkdir /app \
    && sed -i 's/\#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config \
    && sed -i 's/\#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config \
    && echo 'root:root' | chpasswd \
    && systemctl enable ssh \
    && service ssh start

WORKDIR /src

# https://debian.sipwise.com/debian-security/pool/main/l/linux/
RUN wget https://debian.sipwise.com/debian-security/pool/main/l/linux/linux-headers-5.10.0-21-amd64_5.10.162-1_amd64.deb -P /src/ \
    && wget https://debian.sipwise.com/debian-security/pool/main/l/linux/linux-headers-5.10.0-21-common_5.10.162-1_all.deb -P /src/ \
    && wget https://debian.sipwise.com/debian-security/pool/main/l/linux/linux-image-5.10.0-21-amd64-dbg_5.10.162-1_amd64.deb -P /src/ \
    && wget https://debian.sipwise.com/debian-security/pool/main/l/linux/linux-image-5.10.0-21-amd64-unsigned_5.10.162-1_amd64.deb -P /src/

RUN unzip tool.zip \
    && dpkg -i linux-headers-5.10.0-21-common_5.10.162-1_all.deb \
    && dpkg -i linux-image-5.10.0-21-amd64-dbg_5.10.162-1_amd64.deb \
	&& dpkg -i linux-headers-5.10.0-21-amd64_5.10.162-1_amd64.deb
	
RUN apt --fix-broken install \
    && apt install -y kmod linux-base initramfs-tools \
	&& dpkg -i linux-image-5.10.0-21-amd64-unsigned_5.10.162-1_amd64.deb \
	&& apt --fix-broken install -y \
	&& dpkg -i linux-image-5.10.0-21-amd64-unsigned_5.10.162-1_amd64.deb

WORKDIR /src/linux

RUN echo 'MODULE_LICENSE("GPL");' >> module.c && \
    sed -i 's/$(shell uname -r)/5.10.0-21-amd64/g' Makefile && \
    make && \
    mv module.dwarf /app && \
    cp /boot/System.map* /app

WORKDIR /app

RUN zip debian-5.10.0-21-amd64.zip *

CMD ["/bin/bash"]