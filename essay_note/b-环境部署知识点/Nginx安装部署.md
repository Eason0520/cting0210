

## 第一步：安装工具和库

## （http://www.pcre.org/）

```
yum -y install gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

## 第二部：下载并解压

## （http://nginx.org/en/download.html）

```
 cd /usr/local/nginx
 wget -c https://nginx.org/download/nginx-1.18.0.tar.gz
 tar -zxvf nginx-1.18.0.tar.gz
```

## 第三步：配置configure

```
cd /usr/local/nginx/nginx-1.18.0
./configure --prefix=/usr/local/nginx/nginx --lock-path=/var/nginx/lock/nginx.lock --error-log-path=/var/nginx/error.log --http-log-path=/var/nginx/access.log --http-client-body-temp-path=/var/nginx/temp/client --http-proxy-temp-path=/var/nginx/temp/proxy --http-fastcgi-temp-path=/var/nginx/temp/fastcgi --http-uwsgi-temp-path=/var/nginx/temp/uwsgi --http-scgi-temp-path=/var/nginx/temp/scgi --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_sub_module --with-http_gzip_static_module --with-pcre --with-stream --with-file-aio --with-http_realip_module
```

备注：
  --prefix                          						指定安装路径
  --with-http_stub_status_module    	   允许查看nginx状态的模块
  --with-http_ssl_module           			  支持https的模块
  --with-stream                    					支持stream的模块



## 第四步：编译并安装

```
make && make install
```



## 第五步：启动

```
cd /usr/local/nginx/nginx/sbin
./nginx
ps -ef|grep nginx
```

  【其他命令：
    ./nginx -s quit:（温和）此方式停止步骤是待nginx进程处理任务完毕进行停止
    ./nginx -s stop:（强硬）此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程
    ./nginx -s reload 重启nginx(不推荐此方法，推荐先停止在启动)】

## 第六步：测试

### 测试端口
```
netstat -na | grep 8081
```

### 测试网址
  http://139.196.29.243:8081



## 第七步：配置`systemctl`服务

1. 在`/usr/lib/systemd/system`创建`nginx.service`文件：

```undefined
vim /usr/lib/systemd/system/nginx.service
```

- 添加配置内容:

```bash
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
```

- 配置systemctl之后的启动方式

```bash
# 查询状态
systemctl status nginx
# 启动
systemctl start nginx
# 重启
systemctl restart nginx
# 关闭
systemctl stop nginx
# 设置开机启动
systemctl enable nginx
```

## 第八步：配置

修改`conf/nginx.conf`配置文件。

找到`server`模块下的`80`端口。

- 修改`server_name`后面的域名，我这里改成`www.jeremy7.cn`。
- 在`location /`下添加`proxy_pass`反向代理:

```cpp
proxy_pass http://wwwtomcat; 
```

- 对应添加一个`upstream`模块,添加转发的路径：

```undefined
upstream wwwtomcat {
   server 127.0.0.1:8080;
}
```



```cpp
server {
    listen       80;
    server_name  www.jeremy7.cn;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        proxy_pass http://wwwtomcat; 
        root   html;
        index  index.html index.htm;
    }
}
```

## 第九步：配置https

### 开启 443端口

```csharp
server {
    listen 443 ssl;
    #配置HTTPS的默认访问端口为443。
    #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
    #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
    root html;
    index index.html index.htm;
    ssl_certificate cert/cert-file-name.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
    ssl_certificate_key cert/cert-file-name.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
    location / {
        root html;  #站点目录。
        index index.html index.htm;
    }
}
```

- `server_name` 改成自己的申请的域名
- `ssl_certificate` 替换成`.pem`后缀的证书文件
- `ssl_certificate_key` 替换成`.key`后缀的证书文件
- `location /` 里面添加反向代理,也是上面的反向代理：

```cpp
proxy_pass http://wwwtomcat; 
```

测试配置是否正确:

```bash
/usr/local/nginx/sbin/nginx -t 
```

### http强转https

`server`模块添加配置

```bash
rewrite ^(.*)$  https://$host$1 permanent;#将http转成https
```



## 第十步：Nginx 服务器中禁止恶意扫描与垃圾爬虫

### ng版本查询

```shell
$ nginx -v
nginx version: nginx/1.18.0
```

### 屏蔽恶意IP地址

#### 使用 awk 命令查询访问最频繁的IP

```shell
$ cd /usr/local/nginx/logs
# 使用 awk 命令查询访问最频繁的IP
$ awk '{print $1}' access.log|sort | uniq -c |sort -n -k 1 -r|more

# 使用 awk 命令查询访问最频繁的链接
$ awk '{print $7}' access.log|sort | uniq -c |sort -n -k 1 -r|more
```

#### 使用 deny 指令屏蔽 IP： 

deny 指令在 Nginx 的 http, server, location 上下文中指定

```shell
http {
    deny 108.179.194.35; # 扫描 wordpress 
    deny 159.203.31.171; # 扫描 wp-login
    deny 158.69.243.0/24;  # MJ12bot
    deny 46.229.168.0/24;  # SemRush 
    deny 54.36.148.0/24;   # AhrefsBot 
    deny 54.36.149.0/24;   # AhrefsBot
}
```

#### 使用allow 指令指明哪些IP可以访问：

```shell
location /admin {
    allow 192.168.1.0/24;
    allow 112.66.77.88;
    allow 112.65.12.0/24;
    deny  all;
}
```

#### 根据后缀名屏蔽掉一批请求:

```nginx
# 屏蔽恶意访问
if ($document_uri ~* \.(php|asp|aspx|jsp|swp|git|env|yaml|yml|sql|db|bak|ini|docx|doc|rar|tar|gz|zip|log)$) {
    return 404;
}
# 说明，这里使用 if 语句，匹配以 `.` 开始，以 php、asp 、jsp 、env 等后缀结尾的文件，如果存在，nginx 则直接返回 404 给客户端。
```

#### 根据关键词屏蔽一批请求:

```nginx
if ($document_uri ~* (wordpress|phpinfo|wlwmanifest|phpMyAdmin|xmlrpc)) {
    return 404;
}
```

#### 屏蔽恶意爬虫:

浏览器或者爬虫工具访问，一般都会带有 User Agent (浏览器用户标识)

```nginx
server {
    # 禁止Scrapy等工具的抓取
    if ($http_user_agent ~* (Scrapy|HttpClient|PostmanRuntime|ApacheBench|Java||python-requests|Python-urllib|node-fetch)) {
        return 403;
    }

    # 禁止指定的 user agent 以及为空的访问
    if ($http_user_agent ~ "Nimbostratus|MJ12bot|MegaIndex|YisouSpider|^$" ) {
        return 403;             
    }
}
```

### 对IP和服务进行限流

Nginx 提供了2个模块来处理限流，分别是 ngx_http_limit_conn_module 和 ngx_http_limit_req_module 模块

```nginx
http {
    ##### limit
    limit_conn_zone $binary_remote_addr zone=perIP:10m;
    limit_conn_zone $server_name zone=perServer:10m;
    limit_conn perIP 10;
    limit_conn perServer 200;
    limit_conn_status 503;
}

# 上边的配置会对全站都生效。如果只是想对某个 uri 限流，可以将 limit_conn 写在 location 上下文里
http {
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    
    server {
        location /download/ {
            limit_conn addr 1;
        }
    }
}
```

### 配置使用示例

为了方便管理，我们创建两个配置文件，对ip和spider分别管理

屏蔽IP访问 block_ips.conf

```nginx
# 屏蔽IP访问 block_ips.conf
deny 108.179.194.35;   # 扫描 wordpress 
deny 159.203.31.171;   # 扫描 wp-login
deny 158.69.243.0/24;  # MJ12bot
deny 46.229.168.0/24;  # SemRush 
deny 54.36.148.0/24;   # AhrefsBot 
deny 54.36.149.0/24;   # AhrefsBot
```

屏蔽UA访问 block_spiders.conf

```nginx
# 屏蔽UA访问 block_spiders.conf
# 禁止Scrapy等工具的抓取
if ($http_user_agent ~* (Scrapy|HttpClient|PostmanRuntime|ApacheBench|Java||python-requests|Python-urllib|node-fetch)) {
    return 403;
}

# 禁止指定的 user agent 以及为空的访问
if ($http_user_agent ~ "Nimbostratus|MJ12bot|MegaIndex|YisouSpider|^$" ) {
    return 403;             
}

# 屏蔽恶意访问
if ($document_uri ~* \.(php|asp|aspx|jsp|swp|git|env|yaml|yml|sql|db|bak|ini|docx|doc|rar|tar|gz|zip|log)$) {
    return 404;
}

# 屏蔽指定关键词
if ($document_uri ~* (wordpress|phpinfo|wlwmanifest|phpMyAdmin|xmlrpc)) {
    return 404;
}
```

nginx.conf 中引入 include 

```nginx
http {
    include       mime.types;
    default_type  text/html;

    ##### limit 限流
    limit_conn_zone $binary_remote_addr zone=perIP:10m;
    limit_conn_zone $server_name zone=perServer:10m;
    limit_conn perIP 10;
    limit_conn perServer 200;
    limit_conn_status 503;

    ### 引入屏蔽规则
    include /etc/nginx/block_ips.conf;
    include /etc/nginx/block_spiders.conf;

    ### 其他配置
}
```

## 温馨提示，重启前记得测试 配置文件是否有错误。

```shell
$ cd /usr/local/nginx
$ sbin/nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```



## 参考链接

[nginx.org/en/docs/](https://link.juejin.cn/?target=https%3A%2F%2Fnginx.org%2Fen%2Fdocs%2F)

[ssrvps.org/archives/53…](https://link.juejin.cn/?target=https%3A%2F%2Fssrvps.org%2Farchives%2F5370)

[wang123.net/posts/nginx…](https://link.juejin.cn/?target=https%3A%2F%2Fwang123.net%2Fposts%2Fnginx-how-to-block-url-and-ip-address)

