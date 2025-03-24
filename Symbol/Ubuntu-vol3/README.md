本项目默认的内核版本如下所示，版本可以根据需要自行更改

```plaintext
Linux version 5.10.0-21-amd64 (debian-kernel@lists.debian.org) (gcc-10 (Debian 10.2.1-6) 10.2.1 20210110, GNU ld (GNU Binutils for Debian) 2.35.2) #1 SMP Debian 5.10.162-1 (2023-01-21)
```

Tips:如需修改版本需要把内核调试符号，构建好后进入容器，打包好的Profile文件在/volatility目录下

​		可以到 https://launchpadlibrarian.net/ 下载内核调试符号

```
linux-image-unsigned-5.4.0-205-generic-dbgsym_5.4.0-205.225_amd64.ddeb
```

具体使用命令如下

```bash
docker build --tag profile:v1 . 
docker run -it -p 7080:8000 profile:v1 bash
cd /volatility
python3 -m http.server
```

Tips:如无法从Github上直接wget获取dwarf2json，可使用COPY复制本目录下的dwarf2json，版本为0.9
