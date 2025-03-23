# Docker-ProfileMaker-vol2

借鉴以下项目

- [CTF-Archives/profile-builder](https://github.com/CTF-Archives/profile-builder)
- [goodlunatic/Docker-ProfileMaker-vol2](https://github.com/goodlunatic/Docker-ProfileMaker-vol2)

使用Docker制作Linux内存取证中vol2需要的profile

具体使用命令如下

```bash
docker build --tag profile:v1 . 
docker run -it -p 7080:8000 profile:v1 bash
cd /app
python3 -m http.server
```
