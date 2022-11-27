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

## 部署安装tengine

### 第一步：关闭系统默认防火墙( 暂未清楚原因 )

```
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config

# 临时关闭
systemctl stop firewalld
# 禁止开机启动
systemctl disable firewalld
systemctl stop iptables
systemctl disable iptables

systemctl status firewalld
systemctl status iptables
```

### 第二步：安装Tengine（在线yum安装和离线源码方式安装）

#### 安装方法1：离线安装

1. 配置本地yum并安装开发工具（视情况安装配置）

```csharp
mkdir /mnt/cdrom
# 将 /dev/cdrom 挂在 /mnt/cdrom 之下。
mount /dev/cdrom /mnt/cdrom

cat <<EOF >/etc/yum.repos.d/local.repo
[local]
name=local
baseurl=file:///mnt/cdrom
gpgcheck=0
enabled=1
EOF

# 清理本地缓存
yum clean all 
# 清理插件缓存
yum clean plugins 
# 构建缓存
yum makecache       

yum group install -y "Development Tools"
```

2. 下载安装PCRE

```go
curl -o /home/pcre-8.42.tar.gz https://ftp.pcre.org/pub/pcre/pcre-8.42.tar.gz
tar vxzf pcre-8.42.tar.gz
cd pcre-8.42
./configure
make
make install
```

3. 下载安装zlib

```go
wget http://www.zlib.net/zlib-1.2.11.tar.gz
tar vxzf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```

4. 下载安装OpenSSL

```bash
wget https://www.openssl.org/source/openssl-1.1.1a.tar.gz
tar vxzf openssl-1.1.1a.tar.gz
cd openssl-1.1.1a
./config
make
make install

ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/libssl.so.1.1
ln -s /usr/local/lib64/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1

openssl version
```

5.下载并安装Tengine

```shell
curl -o /data/package/tengine-2.3.3.tar.gz https://tengine.taobao.org/download/tengine-2.3.3.tar.gz
tar -vxf tengine-2.3.3.tar.gz
mv tengine-2.3.3 /data/tengine/
cd /data/tengine/tengine-2.3.3
./configure --with-http_ssl_module

# 编译
make
# 安装
make install
```

6.完成安装并启动

默认安装路径：/usr/local/nginx

```bash
# 测试配置文件是否正确
/usr/local/nginx/sbin/nginx -t
# 启动Nginx
/usr/local/nginx/sbin/nginx

# 优雅的重启（重载配置文件，如果配置文件有错误的话，会继续使用之前配置运行）
/usr/local/nginx/sbin/nginx -s reload
    -s stop     快速停止
    -s quit     优雅的退出
    -s reopen   重新打开日志文件
    -s reload   重新加载配置文
```

[Welcome to tengine!](http://1.116.191.34/)



#### 安装方法2：采用阿里云官方yum源方式安装

1. 设置Tengine官方YUM源：CentOS7（视情况设置，本次不需要）

```
curl -o /etc/yum.repos.d/opsx-centos7.repo https://mirrors.aliyun.com/opensource/149994924900000037/opsx/centos7/opsx-centos7.repo
# 清理本地缓存
yum clean all  
# 清理插件缓存
yum clean plugins   
# 构建缓存
yum makecache       
```

2. 安装Tengine并启动

```undefined
yum install -y tengine
/opt/tengine/sbin/nginx
```

3. 配置文件路径

默认安装路径在：/opt/tengine

```bash
# 测试配置文件是否正确
/opt/tengine/sbin/nginx -t

# 优雅的重启（重载配置文件，如果配置文件有错误的话，会继续使用之前配置运行）
/opt/tengine/sbin/nginx -s reload
    -s stop     快速停止
    -s quit     优雅的退出
    -s reopen   重新打开日志文件
    -s reload   重新加载配置文
```

注意：在CentOS7下只配置Tengine官方yum源可以安装成功，REHL7下提示缺少依赖，所以这里同时配置了163的yum源。

```ruby
cat <<EOF >/etc/yum.repos.d/163.repo
[163]
name=163
baseurl=http://mirrors.163.com/centos/7/os/x86_64/
gpgcheck=0
enabled=1
EOF
```



[Welcome to tengine!](http://1.116.191.34/)



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







## 附录：Tengine & Nginx性能测试



```csharp
http://tengine.taobao.org/document_cn/benchmark_cn.html

# 结论:
    Tengine相比Nginx默认配置，提升200%的处理能力。
    Tengine相比Nginx优化配置，提升60%的处理能力。
```

## 附录：Tengine建立虚拟目录



```bash
# 访问：http://10.0.1.131的时候访问的是html目录
# 访问：http://10.0.1.131/111的时候访问的是/opt/tengine/111目录
location / {
    root   html;
    index  index.html index.htm;
}

location /111 {
    alias   /opt/tengine/111;
    index  index.html index.htm;
}
```

## 附录：Tengine一台服务器建立多个虚拟主机（通过域名区分）

1./opt/tengine/的根目录下建立111和222两个文件夹并分别建立index.html主页文件



```ruby
mkdir -p /opt/tengine/111
mkdir -p /opt/tengine/222
echo "111" > /opt/tengine/111/index.html
echo "222" > /opt/tengine/222/index.html
```

2.http下增加如下内容



```undefined
server {
    listen       80;
    server_name  111.com;

    location / {
        root   111;
        index  index.html index.htm;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }

}


server {
    listen       80;
    server_name  222.com;

    location / {
        root   222;
        index  index.html index.htm;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }

}
```

3.重启Tengine服务生效



```undefined
/opt/tengine/sbin/nginx -s reload
```

## 附录：Tengine一台服务器建立多个虚拟主机（通过端口区分）



```undefined
只需要将以上listen       80;修改为自己想要的端口即可。
```

## 附录：Tengine一台服务器建立多个虚拟主机（绑定IP）



```css
server_name  222.com;   将其中的222.com换成想要绑定的IP即可
```

## 附录：Tengine配置文件详解

/opt/tengine/sbin/nginx
 /opt/tengine/sbin/nginx -s reload
 /opt/tengine/sbin/nginx -t
 vim /opt/tengine/conf/nginx.conf



```objectivec
[root@redhat73 conf]# cat nginx.conf

#user  nobody;          # 指定 Nginx Worker 进程运行用户以及用户组。
worker_processes  1;

# 在这里，很多人会误解worker_connections这个参数的意思，认为这个值就是nginx所能建立连接的最大值。
# 其实不然，这个值是表示每个worker进程所能建立连接的最大值
# 所以，一个nginx能建立的最大连接数，应该是worker_connections * worker_processes。
# 当然，这里说的是最大连接数，对于HTTP请求本地资源来说
# 能够支持的最大并发数量是worker_connections * worker_processes
# 而如果是HTTP作为反向代理来说，最大并发数量应该是worker_connections * worker_processes/2。
# 因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服务的连接，会占用两个连接。

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#error_log  "pipe:rollback logs/error_log interval=1d baknum=7 maxsize=2G";

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}

# load modules compiled as Dynamic Shared Object (DSO)
#
#dso {
#    load ngx_http_fastcgi_module.so;
#    load ngx_http_rewrite_module.so;
#}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;
    #access_log  "pipe:rollback logs/access_log interval=1d baknum=7 maxsize=2G"  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #access_log  "pipe:rollback logs/host.access_log interval=1d baknum=7 maxsize=2G"  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```



作者：小六的昵称已被使用
链接：https://www.jianshu.com/p/ff72d5e7d0f7
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
