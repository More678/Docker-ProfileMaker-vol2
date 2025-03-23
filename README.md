# Docker-ProfileMaker-vol2

借鉴以下项目

- [CTF-Archives/profile-builder](https://github.com/CTF-Archives/profile-builder)
- [goodlunatic/Docker-ProfileMaker-vol2](https://github.com/goodlunatic/Docker-ProfileMaker-vol2)

使用Docker制作Linux内存取证中vol2需要的profile

本项目默认的内核版本如下所示，版本可以根据需要自行更改

```plaintext
Linux version 5.4.0-205-generic (buildd@lcy02-amd64-055) (gcc version 9.4.0 (Ubuntu 9.4.0-1ubuntu1~20.04.2)) #225-Ubuntu SMP Fri Jan 10 22:23:35 UTC 2025 (Ubuntu 5.4.0-205.225-generic 5.4.284)
```

Tips:如需修改版本需要把以下四个文件修改，构建好后进入容器，打包好的Profile文件在/app目录下

​		可以到 [官方仓库](https://debian.sipwise.com/debian-security/pool/main/l/linux/)下载以下几个文件

​		可以到 [官方仓库](https://debian.sipwise.com/debian-security/pool/main/l/linux/)下载以下几个文件

```
linux-image-unsigned-5.4.0-205-generic_5.4.0-205.225_amd64.deb
linux-headers-5.4.0-205-generic_5.4.0-205.225_amd64.deb
linux-headers-5.4.0-205_5.4.0-205.225_all.deb
linux-modules-5.4.0-205-generic_5.4.0-205.225_amd64.deb
```

具体使用命令如下

```bash
docker build --tag profile:v1 . 
docker run -it -p 7080:8000 profile:v1 bash
cd /app
python3 -m http.server
```
