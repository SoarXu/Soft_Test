# 安装Docker遇到问题的解决方案
## 1. 验证docker安装结果是否正确

```linux
sudo docker run hello-world
```
遇到提示：
```linux
Using default tag: latest
Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```
根据AI提示，换完各种镜像源也没有用，最后通过修改DNS解决
```linux
sudo nano /etc/docker/daemon.json
```
```bash
{
 
    "dns": ["8.8.8.8", "8.8.4.4"],
 
    "registry-mirrors": [
 
        "https://docker.m.daocloud.io/",
 
        "https://huecker.io/",
 
        "https://dockerhub.timeweb.cloud",
 
        "https://noohub.ru/",
 
        "https://dockerproxy.com",
 
        "https://docker.mirrors.ustc.edu.cn",
 
        "https://docker.nju.edu.cn",
 
        "https://xx4bwyg2.mirror.aliyuncs.com",
 
        "http://f1361db2.m.daocloud.io",
 
        "https://registry.docker-cn.com",
 
        "http://hub-mirror.c.163.com"
 
    ],
 
    "runtimes": {
 
        "nvidia": {
 
            "path": "nvidia-container-runtime",
 
            "runtimeArgs": []
 
        }
 
    }
 
}
```
- 8.8.8.8 和 8.8.4.4 是 Google 的公共 DNS 服务器
- 解决了国内 DNS 污染或解析 Docker Hub 域名的问题
- 既有 https:// 也有 http:// 协议

替换文件内容后，重启Docker服务
```linux
sudo systemctl daemon-reload
sudo systemctl restart docker
```
验证结果：
```linux
sudo docker run hello-world
```
出现：Hello from Docker!
