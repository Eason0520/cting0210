## Tengine简介

Tengine是由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。

### 为什么叫他进阶版？

1. Tengine完全兼容Nginx，因此可以参照Nginx的方式来配置Tengine。
2. Nginx的负载均衡在转发多个子网（192.168.0.x）的时候无法转发，Nginx的轮训算法是根据IP地址的前3位来计算hash。Tengine的一致性hash模块功能完美的解决了我的问题。
3. Tengine的健康检查模块功能自带前台查看页面，可以方便的看到后端tomcat的健康状态。

### 使用

编译&安装

```
$ ./configure
$ make
$ sudo make install
```

Tengine默认将安装在/usr/local/nginx目录。你可以用'--prefix'来指定你想要的安装目录



## Docker 运行 Tengine 服务

由于淘宝没有做docker 的Tengine 镜像，所以我可以搜索下

```
https://hub.docker.com/search?q=tengine&type=image
```

我们选择下载人多的镜像

```
https://hub.docker.com/r/axizdkr/tengine
```

### docker 拉取 Nginx 版本

```
# 拉取最新版本
docker pull axizdkr/tengine:latest

# docker 查看本地镜像
docker images|grep tengine
```

### docker 运行 tengine 服务

```
# 运行指令
docker run -it -d --name docker_tengine \
      -v /data/docker_tengine/nginx.conf:/etc/nginx/conf.d/example.com.conf \
      -p "80:80" -p 443:443 axizdkr/tengine

docker exec -it -d --name docker_tengine \
	  -v example.com.conf:/etc/nginx/conf.d/example.com.conf \
      -v nginx.conf:/etc/nginx/nginx.conf \
      -p "80:80" -p 443:443 axizdkr/tengine

参数说明：
-p 表示端口映射
-v 表示宿主机和容器之间文件映射，
/data/docker_tengine/nginx.conf 表示宿主机文件路径
/etc/nginx/conf.d/example.com.conf 表示容器内文件路径
```

#### nginx.conf配置文件：

```
upstream back.example.com  {

    # list of backend servers
    server backend1.local;
    server backend2.local;
    server backend3.local;

    # sticky session on
    session_sticky;

    #chek interval in ms
    check interval=3000 rise=1 fall=3 timeout=3000 type=http default_down=true;
    check_keepalive_requests 1;
    check_http_send "HEAD / HTTP/1.1\r\nhost: example.com\r\nConnection: close\r\n\r\n";
    check_http_expect_alive http_2xx;

}


server {
    listen 80;
    server_name     pangugle.com www.pangugle.com;
    location / {
	# redirect to https
	return 301 https://$host$request_uri;
    }
    location ~ ^/(.well-known/acme-challenge/.*)$ {
	# redirect to acme storage
	proxy_pass		http://acme.local/$1;
	proxy_set_header	X-Real-IP $remote_addr;
	proxy_set_header	Host $http_host;
	proxy_set_header	X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

server {
	#ssl settings
	### server port and name ###
	listen			443 ssl;
	server_name		example.com www.example.com;

	access_log		off;
	error_log		/var/log/nginx/example.com-error.log;

	### SSL cert files ###
	ssl_certificate		/cert/example.cer;
	ssl_certificate_key	/cert/example.key;

	#ssl proto only
	ssl_protocols		TLSv1.1 TLSv1.2 TLSv1.3;
	# stapling on
	ssl_stapling		on;
	ssl_stapling_verify	on;
	# cipher methods restrict
	ssl_ciphers		HIGH:!aNULL:!MD5:!CAMELLIA;
	ssl_prefer_server_ciphers on;
	keepalive_timeout       60;
	ssl_session_cache       shared:SSL:10m;
	ssl_session_timeout     10m;
	ssl_dhparam             /cert/dhparam.pem;
	# HSTS
	add_header Strict-Transport-Security "max-age=31536000; preload" always;


	location / {
		proxy_pass  http://back.example.com/;

		proxy_next_upstream	error timeout invalid_header http_500 http_502 http_503 http_504;
		proxy_set_header        Accept-Encoding   "";
		proxy_set_header        Host            $host;
		proxy_set_header        X-Real-IP       $remote_addr;
		proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header        X-Forwarded-Proto $scheme;
		add_header              Front-End-Https   on;
		proxy_redirect		off;
	}

}
```

### 查看容器

```
docker ps -a|grep docker_tengine
```

### 访问 tengine

```
http://1.116.191.34:8080
```

### 停止容器并移除

```
docker kill docker_tengine
docker rm docker_tengine
```
