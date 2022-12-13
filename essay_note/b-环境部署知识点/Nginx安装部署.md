

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



