## Docker 命令大全

### 容器生命周期管理

#### docker run

```
# 指定镜像运行一个容器
docker run -itd --name ubuntu -p 5000:5000 ubuntu:15.10 /bin/bash
或
docker run -itd --name test_centos \
           -e myname="hello_pangule" --restart=always \
           -v /opt/test:/opt/test  \
           -v /opt/test2/a.txt:/opt/test2/a.txt \
           -m 200m \
           centos:6.9

# 参数作用:
docker: Docker 的二进制执行文件。
run: 与前面的 docker 组合来运行一个容器。
ubuntu:15.10 指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，不存在Docker就会从镜像仓库Docker Hub下载公共镜像。
/bin/bash 在启动的容器里执行的命令
-t: 在新容器内指定一个伪终端或终端。
-i: 允许你对容器内的标准输入 (STDIN) 进行交互。
-d: 让容器在后台运行。
--name: 指定运行的容器名称，不指定会随机生成
-P: 将容器内部使用的网络端口随机映射到我们使用的主机上
-p: 是容器内部端口绑定到指定的主机端口。
-v 表示指定目录映射或文件映射
--add-host=[] 当宿主机与容器的网络是通过 docker0 网桥连接的时候起作用，本地dns解析，作用在 /etc/hosts
--restart=always 当容器由于外部因素停止运行时，在系统恢复时，会自动重启服务
-e 容器环境变量设置
```

### 容器操作

```
# docker ps 查看容器（查询最后一次创建的容器）
docker ps -l

# docker stop 停止容器
docker stop ubuntu # 可以是是名字，可以是ID
```



- docker exec 连接容器
- docker port 查看端口映射
- docker start 启动
- docker restart 重启
- docker rm 删除容器
- docker images 列出本地镜像列表
- docker pull 下载镜像
- docker search 查找镜像
- docker rmi 删除镜像
- docker inspect 查看容器状态