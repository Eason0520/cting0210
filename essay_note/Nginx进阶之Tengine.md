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






