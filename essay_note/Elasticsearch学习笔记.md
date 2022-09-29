### **Elasticsearch学习笔记**

#### **一、Elasticsearch的由来** 

  ![图片1](assets\图片1.png)      

​	有个帅哥名字叫“Shay Banon”，对就是这位。这是他2013年在dotScale大会上分享Elasticsearch的照片。

#### **二、Elasticsearch的诞生历史**

​	许多年前，一个叫Shay Banon的待业工程师跟随他的新婚妻子来到伦敦，他的妻子想在伦敦学习做一名厨师。而他在伦敦寻找工作的期间，接触到了Lucene的早期版本，他想为自己的妻子开发一个方便搜索菜谱的应用。

​	直接使用Lucene构建搜索会有很多的坑以及重复性的工作，所以Shay便在Lucene的基础上不断进行抽象来让Java程序嵌入搜索变得更容易一些，经过一段时间的打磨，就诞生了他的第一个开源作品，他给自己的这个作品起了个名字，叫 “Compass”，中文即“指南针”的意思。之后，Shay找到了一份新工作，新工作是处在一个高性能分布式的开发环境中。他在工作中渐渐发现，越来越需要一个易用的高性能、实时、分布式搜索服务，于是他决定重写Compass，将它从一个库打造成了一个独立的server，并将其改名为Elasticsearch。 

​	Elasticsearch发布的第一个版本是在2010年的二月份，从那之后，Elasticsearch便成了Github上最受人瞩目的项目之一，并且很快就有超过300名开发者加入进来贡献了自己的代码。后来Shay和另一位合伙人成立了公司专注打造Elasticsearch，他们对Elasticsearch进行了一些商业化的包装和支持。但是，Elasticsearch承诺，永远都将是开源并且免费的。

​	不过悲剧的是，Shay承诺为妻子开发的菜谱搜索应用，到现在还没做出来……

#### **三、Elasticsearch简介**

 ![图片2](assets\图片2.png)

​	http://bbs.itheima.com/forum.php?mod=viewthread&tid=383148&extra=

Lucene本身就被认为是一款很好的开源搜索引擎，Lucene相对复杂，要有深厚的搜索理论才能掌握，难以应用到实际的生活中。

​	Es采用java编写 的提供了简单易用的RESTFul API,开发中可以使用API，简单明了的实现搜索功能，而不必如面对Lucene的复杂性，elasticSearch目标就是屏蔽复杂性，让全文搜索变得相对简单。同时ElasticSearch能够进行大规模的横向扩展，支持PB级的结构化和非结构化数据的处理，

​	简单地说，当我们存储机器不够的时候，或者说存储容量不足的时候，我们可以通过不断的横向加节点，也就是加机器来解决存储容量的问题，通过这种方式可以让我们的存储从GB到TB甚至于PB级的转化。

![图片3](assets\图片3.png)

- 海量数据的分析引擎

假设你有海量的日志数据，每天有几百GB或者TB甚至PB级别的日志数据，当你要分析一些指标数据的时候就可以使用ES的聚合搜索功能，来分析解决。而不用花费大量的人力精力来设计一个新的系统。这样不仅节省大量的成本，还能有不错的使用效果。

- 站内搜索引擎

当你想搭建一站内搜索的时候，不用花费大量的成本，去开发，只需要简单的继承，并进行一些简单的封装，即可使用。

- 数据仓库

可以利用elasticsearch强大的分布式存储能力，可以作为数据仓库来使用。存储PB级别的结构化或者非结构化数据，为上层应用提供强大的数据存储能力。

![图片4](assets\图片4.png)

​	英国卫报 使用elasticsearch 实时收集用户日志和社交网络数据，来提供给他们的编辑，这样可以得到实时的反馈，以便及时了解公众对新发表文章的回应。

​	维基百科 GitHub 作为站内实时搜索，维基百科使用elasticsearch进行全文搜索，并高亮关键字，GitHub使用elasticsearch 检索他们1300亿行代码。

​	百度 实时日志监控平台，基于elasticsearch来搭建的。每天要存储几百TB甚至 PB的日志数据。

![图片5](assets\图片5.png)





腾讯 美团  小米 等很多的互联网巨头都在使用。



#### **四、ElasticSearch 安装配置使用入门**

​	现在企业开发中，更常用是的solr搜索服务器和ElasticSearch搜索服务器 

    	如果大家使用过 Apache Lucene 或 Apache Solr，就会知道它们的使用体验非常有趣。尤其在您需要扩展基于 Lucene 或 Solr 的解决方案时，您就会了解 [Elasticsearch](https://www.elastic.co/products/elasticsearch) 项目背后的动机。Elasticsearch（构建于 Lucene 之上）在一个容易管理的包中提供了高性能的全文搜索功能，支持开箱即用地集群化扩展。您可以通过标准的 REST API 或从特定于编程语言的客户端库与 Elasticsearch 进行交互。

    	本教程将展示 Elasticsearch 的实际工作原理。首先了解命令行访问该 REST API 来了解它的基本信息。然后设置一个本地 Elasticsearch 服务器，并使用Java 应用程序与它交互。

​	对于 Java 示例，还需要安装 [Eclipse](https://eclipse.org/downloads/) 和 [Apache Maven](https://maven.apache.org/download.cgi)。如果您的系统上还没有它们，请下载和安装它们。

##### **4.1 ElasticSearch 下载**

官网： [https://www.elastic.co/products/elasticsearch](https://www.elastic.co/products/elasticsearch) 

中文官网：https://www.elastic.co/cn/products/elasticsearch

![图片6](assets\图片6.png) 

======

![图片7](assets\图片7.png)

Window系统下载**zip**版本，linux系统下载**tar**版本 

下载安装包：https://www.elastic.co/downloads/elasticsearch

下载指定版本的安装包：

​	linux 系统下载： elasticsearch-6.4.0.tar.gz

​	Windows系统下载：elasticsearch-6.4.0.zip



##### **4.2 ElasticSearch 安装配置**

为了模拟真实场景，我们将在linux下安装Elasticsearch。

###### **4.2.1 新建一个用户yunwu**

```
useradd yunwu
```

设置密码：

```
passwd cqy061987
```

出于安全考虑，elasticsearch默认不允许以root账号运行。

切换用户：

```
su - yunwu
```

###### **4.2.2 安装配置**

1. 规划安装目录

   ```
   /home/yunwu
   ```

2. 通过使用FTP工具上传安装包到指定目录

   ![图片8](assets\图片8.png)

3. 解压安装包

   ```
   tar -zxvf elasticsearch-6.4.0.tar.gz
   ```

   ![图片9](assets\图片9.png)

   ​

4. 重命名安装目录并删除安装包

   ```
   mv elasticsearch-6.4.0 elasticsearch
   ```

   ![图片10](assets\图片10.png)

   ```
   rm -rf elasticsearch-6.4.0.tar.gz
   ```

5. 进入，查看目录结构：

    ![图片11](assets\图片11.png)




###### **4.2.3 修改配置文件**

​      进入到 cd elasticsearch/config/ 需要修改的配置文件有两个：

```
elasticsearch.yml 
jvm.options
```

找到elasticsearch.yml 文件，修改elasticsearch.yml 文件中的内容	

> 修改elasticsearch.yml

```
vim elasticsearch.yml
```

修改数据和日志目录：

```yml
path.data: /home/yunwu/elasticsearch/data # 数据目录位置
path.logs: /home/yunwu/elasticsearch/logs # 日志目录位置
```

修改绑定的ip：

```
network.host: 0.0.0.0 # 绑定到0.0.0.0，允许任何ip来访问
```

默认只允许本机访问，修改为0.0.0.0后则可以远程访问

> 修改jvm配置

Elasticsearch基于Lucene的，而Lucene底层是java实现，因此我们需要配置jvm参数

```
vim jvm.options
```

默认配置如下：

```
-Xms1g
-Xmx1g
```

内存占用太多了，我们调小一些：

```
-Xms512m
-Xmx512m
```

目前我们是做的单机安装，如果要做集群，只需要在这个配置文件中添加其它节点信息即可。

> elasticsearch.yml的其它可配置信息：

| 属性名                                | 说明                                       |
| ---------------------------------- | ---------------------------------------- |
| cluster.name                       | 配置elasticsearch的集群名称，默认是elasticsearch。建议修改成一个有意义的名称。 |
| node.name                          | 节点名，es会默认随机指定一个名字，建议指定一个有意义的名称，方便管理      |
| path.conf                          | 设置配置文件的存储路径，tar或zip包安装默认在es根目录下的config文件夹，rpm安装默认在/etc/ elasticsearch |
| path.data                          | 设置索引数据的存储路径，默认是es根目录下的data文件夹，可以设置多个存储路径，用逗号隔开 |
| path.logs                          | 设置日志文件的存储路径，默认是es根目录下的logs文件夹            |
| path.plugins                       | 设置插件的存放路径，默认是es根目录下的plugins文件夹           |
| bootstrap.memory_lock              | 设置为true可以锁住ES使用的内存，避免内存进行swap            |
| network.host                       | 设置bind_host和publish_host，设置为0.0.0.0允许外网访问 |
| http.port                          | 设置对外服务的http端口，默认为9200。                   |
| transport.tcp.port                 | 集群结点之间通信端口                               |
| discovery.zen.ping.timeout         | 设置ES自动发现节点连接超时的时间，默认为3秒，如果网络延迟高可设置大些     |
| discovery.zen.minimum_master_nodes | 主结点数量的最少值 ,此值的公式为：(master_eligible_nodes / 2) + 1 ，比如：有3个符合要求的主结点，那么这里要设置为2 |
|                                    |                                          |



###### **4.2.4 创建data和logs目录**

刚才我们修改配置，把data和logs目录修改指向了elasticsearch的安装目录。但是这两个目录并不存在，因此我们需要创建出来：

进入Elasticsearch的根目录，然后创建：

```
mkdir data
mkdir logs
```

 ![图片12](assets\图片12.png)





##### **4.3 运行**

进入elasticsearch/bin目录，可以看到下面的执行文件：

![图片13](assets\图片13.png)

然后输入命令：

```
./elasticsearch
```

发现报错了，启动失败：

###### **4.3.1.错误1：内核过低** ![图片14](assets\图片14.png)

我们使用的是centos6，其linux内核版本为2.6。而Elasticsearch的插件要求至少3.5以上版本。不过没关系，我们禁用这个插件即可。

修改elasticsearch.yml文件，在最下面添加以后配置：

```
bootstrap.system_call_filter: false
```

然后重启



###### **4.3.2.错误2：文件权限不足** 

再次启动，又出错了：

```
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
```

我们用的是yunwu用户，而不是root，所以文件权限不足。

**首先用root用户登录。**

然后修改配置文件:

```
vim /etc/security/limits.conf
```

添加下面的内容：

*代表任意用户

```shell
* soft nofile 65536

* hard nofile 131072

* soft nproc 4096

* hard nproc 4096
```

了解：nofile 是针对用户打开最大文件数的限制的一个设置，nproc是操作系统级别对每个用户创建的进程数的限制,在Linux下运行多线程时,每个线程的实现其实是一个轻量级的进程,对应的术语是:light weight process(LWP)。

soft nproc: 可打开的文件描述符的最大数(软限制)

hard nproc： 可打开的文件描述符的最大数(硬限制)

soft nofile：单个用户可用的最大进程数量(软限制)

hard nofile：单个用户可用的最大进程数量(硬限制)



###### **4.3.3.错误3   线程数不够**

刚才报错中，还有一行：

```shell
[1]: max number of threads [1024] for user [yunwu] is too low, increase to at least [4096]
```

这是线程数不够。

继续修改配置：

```
vim /etc/security/limits.d/90-nproc.conf 
```

修改下面的内容：

```
* soft nproc 1024
```

改为：

```
* soft nproc 4096
```

###### **4.3.4.错误4      进程虚拟内存**

```
[3]: max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```

继续修改配置文件：	

```
vim /etc/sysctl.conf 
```

添加下面内容：

```
vm.max_map_count = 655360
```

https://www.cnblogs.com/Jtianlin/p/4339931.html  注：stsctl.conf 详解

然后执行命令：-p   从指定的文件加载系统参数，如不指定即从/etc/sysctl.conf中加载

```
sysctl -p
```

###### **4.3.5.重启终端窗口**

所有错误修改完毕，一定要重启你的 Xshell终端，否则配置无效。

###### **4.3.6.启动**

再次启动，终于成功了！

![图片15](assets\图片15.png)

可以看到绑定了两个端口:

- 9300：集群节点间通讯接口
- 9200：客户端访问接口

我们在浏览器中访问：http://192.168.57.101:9200

 ![图片16](assets\图片16.png)



#### **五、安装elasticsearch-head插件**

##### **5.1 Head插件简介**

ElasticSearch-head是一个H5编写的ElasticSearch集群操作和管理工具，可以对集群进行傻瓜式操作。

- 显示集群的拓扑,并且能够执行索引和节点级别操作
- 搜索接口能够查询集群中原始json或表格格式的检索数据
- 能够快速访问并显示集群的状态
- 有一个输入窗口,允许任意调用RESTful API。这个接口包含几个选项,可以组合在一起以产生有趣的结果;
- 5.0版本之前可以通过plugin名安装，5.0之后可以独立运行。

##### 5.2 node.js安装

​	因为ElasticSearch-head启动需要node.js环境，所以我们在安装ElasticSearch-head之前需要安装node.js的环境。主要还是/home/yunwu/elasticsearch-head-master/package.json文件运行需要node.js的环境。

```
/home/yunwu/elasticsearch-head-master/package.json
```

###### **5.2.1 安装node.js**

​	简单的说 Node.js 就是运行在服务端的 JavaScript。Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

1、下载对应你系统的Node.js版本:

[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

（我们现在使用的版本是8.12.0，资源中也已提供） ![图片28](assets\图片28.png)



2、选安装目录进行安装

​	2.1 切换到root用户，使用ftp工具。上传到yunwu用户的文件夹下 ![图片29](assets\图片29.png)

​	2.2 解压tar.xz结尾的压缩文件

```shell
[yunwu@quanyi ~]$ xz -d node-v8.12.0-linux-x64.tar.xz

[yunwu@quanyi ~]$ tar -xvf node-v8.12.0-linux-x64.tar 
```

![图片30](assets\图片30.png)

2.3 root用户配置环境变量

2.3.1. 配置环境变量

```shell
[root@quanyi etc]# vim /etc/profile
```

```shell
#set java environment
export JAVA_HOME=/usr/java/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin:/home/yunwu/elasticsearch/bin:/home/yunwu/node-v8.12.0-linux-x64/bin:
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME PATH CLASSPATH
```

执行 source /etc/profile 

```shell
[root@quanyi ~]# source /etc/profile
```

执行node -v验证安装

```shell
#root用户下
[root@quanyi /]# node -v
v8.12.0
```

```shell
#其他用户下 使用xshell5 等连接工具的情况下，断开从新连接
[yunwu@quanyi ~]$ node -v
v8.12.0
```



###### **5.2.2 npm**

​	npm全称Node Package Manager，他是node包管理和分发工具。其实我们可以把NPM理解为前端的Maven .安装完成node.js，已经集成了npm工具，在命令提示符输入 npm -v 可查看当前npm版本

在控制台输入`npm -v`查看：

```shell
[root@quanyi ~]# npm -v
6.4.1
```

```shell
[yunwu@quanyi ~]$ npm -v
6.4.1
```



###### **5.2.3 nrm**

npm默认的仓库地址是在国外网站，速度较慢，建议大家设置到淘宝镜像。但是切换镜像是比较麻烦的。推荐一款切换镜像的工具：nrm

我们首先安装nrm，这里`-g`代表全局安装

```
[root@quanyi ~]# npm install nrm -g
```

然后通过`nrm ls`命令查看npm的仓库列表,带*的就是当前选中的镜像仓库：

```shell
[root@quanyi ~]# nrm ls

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

通过`nrm use taobao`来指定要使用的镜像源：

```shell
[root@quanyi ~]# nrm use taobao
                        
Registry has been set to: https://registry.npm.taobao.org/

[root@quanyi ~]# nrm ls

  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

然后通过`nrm test npm `来测试速度：

```shell
[root@quanyi ~]# nrm test npm
  npm ---- 826ms
```

测试淘宝请输入：

```shell
[root@quanyi ~]# nrm test taobao
* taobao - 181ms
```

注意：

- 安装完成请一定要重启下电脑！！！



##### **5.3 elasticsearch-head插件的安装**

下载地址：https://github.com/mobz/elasticsearch-head

![图片31](assets\图片31.png)

 ![图片32](assets\图片32.png)

上传到/home/yunwu目录下 解压

```
[yunwu@quanyi ~]$ unzip elasticsearch-head-master.zip 
```

![图片33](assets\图片33.png)



 /home/yunwu/elasticsearch-head-master/package.json

我们再打开package.json文件，发现刚才下载的express已经添加到依赖列表中了.

关于版本号定义：

```
指定版本：比如1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。

波浪号（tilde）+指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，也就是说安装时不改变大版本号和次要版本号。

插入号（caret）+指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。

latest：安装最新版本。
```

##### **5.5启动elastichsearch-head**

npm镜像的安装

```
[yunwu@quanyi elasticsearch-head-master]$ npm install
```

启动elasticsearch-head插件：

```shell
[yunwu@quanyi ~]$ cd elasticsearch-head-master
[yunwu@quanyi elasticsearch-head-master]$ grunt server
(node:2646) ExperimentalWarning: The http2 module is an experimental API.
Running "connect:server" (connect) task
Waiting forever...
Started connect web server on http://localhost:9100
```

或者：

```shell
[yunwu@quanyi elasticsearch-head-master]$ npm run start

> elasticsearch-head@0.0.0 start /home/yunwu/elasticsearch-head-master
> grunt server

(node:5055) ExperimentalWarning: The http2 module is an experimental API.
Running "connect:server" (connect) task
Waiting forever...
Started connect web server on http://localhost:9100
```

打开浏览器访问： ![图片48](assets\图片48.png)

也就是说，没有连接到Elasticsearch服务。因为elasticsearch服务与elasticsearch-head之间可能存在跨域，修改elasticsearch配置即可，在elastichsearch.yml中添加如下命名即可：

```shell
http.cors.enabled: true
http.cors.allow-origin: "*"
```

```
说明：了解
http.cors.enabled ：是否支持跨域，默认为false
http.cors.allow-origin：当设置允许跨域，默认为*,表示支持所有域名，如果我们只是允许某些网站能访问，那么可以使用正则表达式。比如只允许本地地址。 /https?:\/\/localhost(:[0-9]+)?/
```

重新启动elastichsearch  

重新启动elastichsearch-head

![图片49](assets\图片49.png)

#### **六、ElasticSearch 基本操作入门**

推荐书籍 ： 

《Elasticsearch服务器开发（第2版）.pdf 》

《Elasticsearch权威指南（中文版）.pdf》

 ![图片34](assets\图片34.png)

全文检索： 针对文本中每个词，创建词条建立索引，进行搜索。 ElasticSearch 操作服务器上的数据，通过 Rest API 操作数据。

![图片35](assets\图片35.png)

 ![图片36](assets\图片36.png)

​	Elasticsearch可以作为一个独立的单个搜索服务器。不过，为了能够处理大型数据集，实现容错和高可用性，Elasticsearch可以运行在许多互相合作的服务器上。这些服务器称为集群（cluster），形成集群的每个服务器称为节点（node）。 ![图片37](assets\图片37.png)

​	如果操作Elasticsearch上数据，访问提供Rest API的URL地址，传递json数据给服务器 ![图片38](assets\图片38.png)

##### **6.1ElasticSearch 基础 数据架构的主要概念**

 ![图片39](assets\图片39.png)

 ![图片40](assets\图片40.png)

 ![图片41](assets\图片41.png)

 ![图片42](assets\图片42.png) 

详细了解：

![图片43](assets\图片43.png)

```
索引对象（blob）： 存储数据的表结构 ，任何搜索数据，存放在索引对象上 。

映射（mapping）： 数据如何存放到索引对象上，需要有一个映射配置， 包括：数据类型、是否存储、是否分词 … 等。

文档（document）： 一条数据记录， 存在索引对象上 

文档类型（type）： 一个索引对象 存放多种类型数据，数据用文档类型进行标识  
```

【后续编程步骤】：

```
第一步：建立索引对象 
第二步：建立映射 
第三步：存储数据【文档】 
第四步：指定文档类型进行搜索数据【文档】
```



##### **6.5 Elasticsearch和mysql对比**

​	Elasticsearch 集群可以包含多个索引（Index），每个索引可以包含多个类型（Type），每个类型可以包含多个文档（Document），每个文档可以包含多个字段（Field）。以下是 MySQL 和 Elasticsearch 的术语类比图，帮助理解：

 ![图片44](assets\图片44.png)

```
Elasticsearch也是基于Lucene的全文检索库，本质也是存储数据，很多概念与MySQL类似的。

对比关系：

索引（indices）--------------------------------Databases 数据库

	类型（type）-----------------------------Table 数据表

	     文档（Document）----------------Row 行

		   字段（Field）-------------------Columns 列 

```

就像使用 MySQL 必须指定 Database 一样，要使用 Elasticsearch 首先需要创建 Index：

​	client.indices.create({index : 'blog'});

​	这样就创建了一个名为 blog的 Index。Type 不用单独创建，在创建 Mapping 时指定就可以。Mapping 用来定义 Document 中每个字段的类型，即所使用的 analyzer、是否索引等属性，非常关键等。创建 Mapping 的代码示例如下：

```json
client.indices.putMapping({
    index : 'blog',
    type : 'article',
    body : {
        article: {
            properties: {
                id: {
                    type: 'string',
                    analyzer: 'ik',
                    search_analyzer: 'ik',
                },
                title: {
                    type: 'string',
                    analyzer: 'ik',
                    search_analyzer: 'ik',
                },
                content: {
                    type: 'string',
                    analyzer: 'ik',
                    search_analyzer: 'ik',
                }
            }
        }
    }
});
```



#### **七、CURL命令操作执行REST命令**

​	Elasticsearch 是无模式的，这意味着它可以接受您提供的任何命令，并处理它以供以后查询。Elasticsearch 中的所有内容都被存储为文档，所以您的第一个练习是存储一个包含博客的文档。首先创建一个索引，它是您的所有文档类型的容器 — 类似于 MySQL 等关系数据库中的数据库。然后，将一个文档插入该索引中，以便可以查询该文档的数据。

##### **7.1创建一个索引**

​	Elasticsearch 命令的一般格式是：REST VERBHOST:9200/index/doc-type— 其中 REST VERB 是 PUT、GET 或DELETE。（使用 cURL -X 动词前缀来明确指定 HTTP 方法。）

要创建一个索引，可在您的 shell 中运行以下命令：

```shell
[yunwu@quanyi ~]$ curl -XPUT "http://localhost:9200/blog01/"
```

 ![图片45](assets\图片45.png)

​	尽管 Elasticsearch 是无模式的，但它在幕后使用了 Lucene，后者使用了模式。不过 Elasticsearch 为您隐藏了这种复杂性。实际上，您可以将 Elasticsearch 文档类型简单地视为子索引或表名称。但是，如果您愿意，可以指定一个模式，所以您可以将它视为一种模式可选的数据存储。



##### **7.2插入一个文档**

​	要在 /blog01 索引下创建一个类型，可插入一个文档。

​	要将包含 “Whatiselasticsearch” 的文档插入索引中，可运行以下命令（将该命令和本教程的其他 CURL 命令都键入到一行中）：

```shell
[yunwu@quanyi ~]$ curl -XPUT "http://localhost:9200/blog01/article/1" -d  '{"id": "1", "title": "Whatiselasticsearch"}'
```

报错：

```json
{"error":"Content-Type header [application/x-www-form-urlencoded] is not supported","status":406}
```

解决方案：

```shell
[yunwu@quanyi ~]$ curl -H "Content-Type: application/json" -XPUT "http://localhost:9200/blog01/article/1" -d  '{"id": "1", "title": "Whatiselasticsearch"}'
```

成功：

```json
{"_index":"blog01","_type":"article","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}
```

截图：

![图片46](assets\图片46.png)

​	前面的命令使用 PUT 动词将一个文档添加到 /article文档类型，并为该文档分配 ID 为1。URL 路径显示为index/doctype/ID（索引/文档类型/ID）。

##### **7.3 查看文档**

要查看该文档，可使用简单的 GET 命令：

```shell
[yunwu@quanyi ~]$ curl -XGET "http://localhost:9200/blog01/article/1"
```

结果：

```json
{"_index":"blog01","_type":"article","_id":"1","_version":1,"found":true,"_source":{"id": "1", "title": "Whatiselasticsearch"}}
```

##### **7.4 更新文档**

如果您认识到title字段写错了，并想将它更改为 Whatislucene 怎么办？可运行以下命令来更新文档：

```shell
[yunwu@quanyi ~]$ curl -H "Content-Type: application/json" -XPUT "http://localhost:9200/blog01/article/1" -d  '{"id": "1", "title": "Whatislucene"}'
```

响应：

```json
{"_index":"blog01","_type":"article","_id":"1","_version":2,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":1,"_primary_term":1}
```

查看是否更新成功：

```shell
[yunwu@quanyi ~]$ curl -XGET "http://localhost:9200/blog01/article/1"
```

响应：

```json
{"_index":"blog01","_type":"article","_id":"1","_version":2,"found":true,"_source":{"id": "1", "title": "Whatislucene"}}
```



##### **7.5 搜索文档**

​	是时候运行一次基本查询了，此查询比您运行来查找 “Get the Halls” 文档的简单 GET 要复杂一些。文档 URL 有一个内置的 _search 端点用于此用途。在标题中找到所有包含单词 lucene 的数据：

```shell
[yunwu@quanyi ~]$ curl -XGET "http://localhost:9200/blog01/article/_search?q=title:'whatislucene'"
```

响应：

```json
{"took":51,"timed_out":false,"_shards":{"total":5,"successful":5,"skipped":0,"failed":0},"hits":{"total":1,"max_score":0.2876821,"hits":[{"_index":"blog01","_type":"article","_id":"1","_score":0.2876821,"_source":{"id": "1", "title": "Whatislucene"}}]}}
```

检查搜索返回对象：

```
上图中给出了 Elasticsearch 从前面的查询返回的数据。
    在结果中，Elasticsearch 提供了多个 JSON 对象。第一个对象包含请求的元数据：看看该请求花了多少毫秒 (took) 和它是否超时 (timed_out)。_shards 字段需要考虑 Elasticsearch 是一个集群化服务的事实。甚至在这个单节点本地部署中，Elasticsearch 也在逻辑上被集群化为分片。在往后看可以观察到 hits 对象包含：
total 字段，它会告诉您获得了多少个结果
max_score，用于全文搜索

实际结果包含 fields 属性，因为您将 fields 参数添加到了查询中。否则，结果中会包含 source，而且包含完整的匹配文档。_index、_type 和 _id 分别表示索引、文档类型、ID；_score 指的是全文搜索命中长度。这 4 个字段始终会在结果中返回。
```

相关说明：

- took：查询花费时间，单位是毫秒
- time_out：是否超时
- _shards：分片信息
- hits：搜索结果总览对象
  - total：搜索到的总条数
  - max_score：所有结果中文档得分的最高分
  - hits：搜索结果的文档对象数组，每个元素是一条搜索到的文档信息
    - _index：索引库
    - _type：文档类型
    - _id：文档id
    - _score：文档得分
    - _source：文档的源数据



##### **7.6 删除文档**

在正式的开发中是不能删除数据的，只要知道怎么删除就可以了。

```shell
curl -XDELETE "http://localhost:9200/blog01/article/1"
```

响应：

```json
{"_index":"blog01","_type":"article","_id":"1","_version":5,"result":"deleted","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":4,"_primary_term":1}
```

##### **7.7 删除索引**

在正式的开发中是不能删除索引的，只要知道怎么删除就可以了。

```shell
[yunwu@quanyi ~]$ curl -XDELETE "http://localhost:9200/blog01"
```

响应：

```json
{"acknowledged":true}
```



#### **八、 使用Java操作客户端**

当直接在ElasticSearch 建立文档对象时，如果索引不存在的，默认会自动创建，映射采用默认方式 

l 	ElasticSearch 服务默认端口 9300 

l 	Web 管理平台端口 9200 

​	Elasticsearch 的 Java 客户端非常强大；它可以建立一个嵌入式实例并在必要时运行管理任务。运行一个 Java 应用程序和 Elasticsearch 时，有两种操作模式可供使用。该应用程序可在 Elasticsearch 集群中扮演更加主动或更加被动的角色。在更加主动的情况下（称为 Node Client），应用程序实例将从集群接收请求，确定哪个节点应处理该请求，就像正常节点所做的一样。（应用程序甚至可以托管索引和处理请求。）另一种模式称为 Transport Client，它将所有请求都转发到另一个 Elasticsearch 节点，由后者来确定最终目标。

8.1 创建maven项目 导入pom

```xml
<dependencies>
  <!-- Elasticserach6 -->
  <dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>transport</artifactId>
    <version>6.4.0</version>
  </dependency>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.9.1</version>
  </dependency>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
  </dependency>
</dependencies>
```



##### **8.1测试java客户端连接Elasticsearch**



```java
package cn.itheima.elasticsearch.demo1;

import java.net.InetAddress;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.TransportAddress;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import org.junit.Test;

public class test {

	public final static String HOST = "192.168.57.101";

	public final static int PORT = 9300;// http请求的端口是9200，客户端是9300

	private TransportClient client;

	@Test
	public void init() throws Exception {
		client = new PreBuiltTransportClient(Settings.EMPTY)
				.addTransportAddress(new TransportAddress(InetAddress.getByName(HOST), 9300));

		// on shutdown
		System.out.println(client);
		client.close();
	}
}
```

##### **8.2  建立文档， 自动创建索引**

方式一：拼装json的字符串

```java
//使用字符串
@Test
public void createIndexNoMapping() {
  String json = "{" + "\"id\":\"1\"," + "\"title\":\"基于Lucene的搜索服务器\","
    + "\"content\":\"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口\"" + "}";
  IndexResponse indexResponse = this.client.prepareIndex("blog", "article", "1").setSource(json,XContentType.JSON).execute()
    .actionGet();
  // 获取响应的信息
  System.out.println("索引名称：" + indexResponse.getIndex());
  System.out.println("文档类型：" + indexResponse.getType());
  System.out.println("ID：" + indexResponse.getId());
  System.out.println("版本：" + indexResponse.getVersion());
  System.out.println("存储位置："+indexResponse.getLocation(HOST));
  client.close();
}
```

方式二：使用map集合

```java
/**创建索引、类型、文档*/
	@Test
	public void createIndexNoMapping1() {
		Map<String, Object> json = new HashMap<String, Object>();
		json.put("id", "2");
		json.put("title", "基于Lucene的搜索服务器");
		json.put("content", "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
		IndexResponse indexResponse = this.client.prepareIndex("blog", "article", "2").setSource(json).execute().actionGet();
		// 获取响应的信息
		System.out.println("索引名称："+indexResponse.getIndex());
		System.out.println("文档类型："+indexResponse.getType());
		System.out.println("ID："+indexResponse.getId());
		System.out.println("版本："+indexResponse.getVersion());
		System.out.println("存储位置："+indexResponse.getLocation(HOST));
		client.close();
	}
```

##### 

方式三：使用es的帮助类

```java
@Test
	public void createIndexNoMapping2() throws Exception{
		// 使用es的帮助类，创建一个json方式的对象
		/**
		 * 描述json 数据
		 * {id:xxx, title:xxx, content:xxx}
		 */
		XContentBuilder sourceBuilder = XContentFactory.jsonBuilder()
			.startObject()
				.field("id", 3)
				.field("title", "ElasticSearch是一个基于Lucene的搜索服务器")
				.field("content",
						"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。")
			.endObject();
		// 创建索引
		IndexResponse indexResponse = client.prepareIndex("blog", "article", "3").setSource(sourceBuilder).get();
		// 获取响应的信息
		System.out.println("索引名称："+indexResponse.getIndex());
		System.out.println("文档类型："+indexResponse.getType());
		System.out.println("ID："+indexResponse.getId());
		System.out.println("版本："+indexResponse.getVersion());
		System.out.println("存储位置："+indexResponse.getLocation(HOST));
		client.close();
	}
```



没有映射创建，自动创建索引 和 映射

名称为blog

 ![图片50](assets\图片50.png)

自动创建索引映射

文档数据 （type 文档类型 ）

![图片51](assets\图片51.png)



##### **8.3 搜索文档数据**

###### **8.3.1 搜索单个索引**

```java
/**
 * get API 获取指定文档信息
 */
@Test
public void testGetData() throws Exception {

  GetResponse response = client.prepareGet("blog", "article", "1")

    .get();
  System.out.println(response.getSourceAsString());
  // 关闭连接
  client.close();
}
```

###### **8.3.2 搜索多个索引**

```java
@Test
public void testMultiGet() {
  MultiGetResponse multiGetResponse = client.prepareMultiGet()
    .add("blog", "article", "1")
    .add("blog", "article", "2", "3", "4")
    .add("blog", "article", "2")
    .get();

  for (MultiGetItemResponse itemResponse : multiGetResponse) {
    GetResponse response = itemResponse.getResponse();
    if (response.isExists()) {
      String sourceAsString = response.getSourceAsString();
      System.out.println(sourceAsString);
    }
  }
  client.close();
}
```



##### **8.4更新文档数据**

【更新方式一】

```java
/**
 * 测试更新 update API 使用 updateRequest 对象
 */
@Test
public void testUpdate() throws Exception {
  UpdateRequest updateRequest = new UpdateRequest();
  updateRequest.index("blog");
  updateRequest.type("article");
  updateRequest.id("1");
  updateRequest.doc(XContentFactory.jsonBuilder().startObject()
                    // 对没有的字段添加, 对已有的字段替换
                    .field("title", "ElasticSearch是一个基于Lucene的搜索服务器")
                    .field("content",
                           "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。")
                    .field("createDate", "2018-10-11").endObject());
  UpdateResponse updateResponse = client.update(updateRequest).get();

  // 获取响应的信息
  System.out.println("索引名称："+updateResponse.getIndex());
  System.out.println("文档类型："+updateResponse.getType());
  System.out.println("ID："+updateResponse.getId());
  System.out.println("版本："+updateResponse.getVersion());
  System.out.println("是否更新成功："+updateResponse.status());
  //关闭
  client.close();
}
```

【更新方式二】

```java
/**
	 * 测试更新 update API 使用 updateRequest 对象
	 */
	@Test
	public void testUpdate2() throws Exception {
		// 使用updateRequest对象及documents进行更新
		UpdateResponse updateResponse = client
				.update(new UpdateRequest("blog", "article", "1").doc(XContentFactory.jsonBuilder().startObject()
						.field("title", "什么是Elasticsearch，ElasticSearch是一个基于Lucene的搜索服务器").endObject()))
				.get();
		// 获取响应的信息
		System.out.println("索引名称："+updateResponse.getIndex());
		System.out.println("文档类型："+updateResponse.getType());
		System.out.println("ID："+updateResponse.getId());
		System.out.println("版本："+updateResponse.getVersion());
		System.out.println("是否更新成功："+updateResponse.status());
		//关闭
		client.close();
	}
```

【更新方式三】

```java
/**
     * 测试upsert方法
     */
    @Test
    public void testUpsert() throws Exception {
        // 设置查询条件, 查找不到则添加
    	// 设置一个查询的条件，使用ID查询，如果查找不到数据，添加IndexRequest中的文档数据
        IndexRequest indexRequest = new IndexRequest("blog", "article", "4")
            .source(XContentFactory.jsonBuilder()
                .startObject()
                .field("title", "搜索服务器")
				.field("content",
						"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。")
                .endObject());
        // 设置更新, 查找到更新下面的设置
        // 设置更新的数据，使用ID查询，如果查找到数据，更新UpdateRequest中的文档数据
       UpdateRequest upsert = new UpdateRequest("blog", "article", "4")
            .doc(XContentFactory.jsonBuilder()
                    .startObject()
                        .field("user", "李四")
                    .endObject())
            .upsert(indexRequest);
        
        client.update(upsert).get();

        client.close();
    }
```

##### **8.5 条件查询QueryBuilder**

​	到目前为止，我们使用了REST API和简单查询或GET请求来搜索数据。更改索引时，无论想执行的操作是更改映射还是文档索引化，都要用REST API向Elasticsearch发送JSON结构的数据。类似地，如果想发送的不是一个简单的查询，仍然把它封装为JSON结构并发送给Elasticsearch。这就是所谓的查询DSL。从更宏观的角度看，Elasticsearch支持两种类型的查询：基本查询和复合查询。

​	基本查询，如词条查询用于查询实际数据。

​	第二种查询为复合查询，如布尔查询，可以合并多个查询。查询数据 主要依赖QueryBuilder对象 ，可以通过QueryBuilders获取各种查询 ：（基于lucene）

```
boolQuery() 布尔查询，可以用来组合多个查询条件 
fuzzyQuery() 相似度查询 
matchAllQuery() 查询所有数据 
regexpQuery() 正则表达式查询 
termQuery() 词条查询 
wildcardQuery() 模糊查询
```

使用SearchResponse获取，支持各种查询：

```java
/**
	 * 搜索在elasticSearch中创建的文档对象
	 */
	@Test
	public void testSearch() throws Exception {
		SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.matchAllQuery()).get();

		// 获取命中次数，查询结果有多少对象
		SearchHits hits = searchResponse.getHits();

		long totalHits = hits.getTotalHits();
		System.out.println("查询结果有：" + totalHits + "条");

		Iterator<SearchHit> iterator = hits.iterator();

		while (iterator.hasNext()) {
			// 每个查询对象
			SearchHit searchHit = iterator.next();
			// 获取字符串格式打印
			String sourceAsString = searchHit.getSourceAsString();
			System.out.println(sourceAsString);

			System.out.println("id:" + searchHit.getId());
			System.out.println("Index:" + searchHit.getIndex());
			System.out.println("type:"+searchHit.getType());
			System.out.println("文档id:" + searchHit.getSourceAsMap().get("id"));
			System.out.println("文档title:" + searchHit.getSourceAsMap().get("title"));
			System.out.println("文档content:" + searchHit.getSourceAsMap().get("content"));

		}
		// 关闭连接
		client.close();

	}
```

提取打印结果的方法：

```java
	private void searchValue(SearchHits hits) {
		Iterator<SearchHit> iterator = hits.iterator();

		while (iterator.hasNext()) {
			// 每个查询对象
			SearchHit searchHit = iterator.next();
			// 获取字符串格式打印
			String sourceAsString = searchHit.getSourceAsString();
			System.out.println(sourceAsString);

			System.out.println("id:" + searchHit.getId());
			System.out.println("Index:" + searchHit.getIndex());
			System.out.println("type:"+searchHit.getType());
			System.out.println("文档id:" + searchHit.getSourceAsMap().get("id"));
			System.out.println("文档title:" + searchHit.getSourceAsMap().get("title"));
			System.out.println("文档content:" + searchHit.getSourceAsMap().get("content"));

		}
	}
```



```java
/**
	 * 1、ElasticSearch提供QueryBuileders.queryStringQuery(搜索内容) 针对多字段的query_string查询
	 * 查询query_string方法，对所有字段进行分词查询 例如：在title字段，或者content字段去进行条件搜索
	 */
	@Test
	public void testSearch2() throws Exception {
		SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.queryStringQuery("全面")).get();
		SearchHits hits = searchResponse.getHits();
		System.out.println("查询结果有：" + hits.getTotalHits() + "条");
		searchValue(hits);
		
		client.close();

	}
```

```java
/**
	 * 2、 只想查询content里包含全文 ，使用QueryBuilders.wildcardQuery模糊查询 *任意字符串 ?任意单个字符
	 */
	/**
	 * 通配符查询允许我们在查询值中使用*和?等通配
	 * elasticsearch提供QueryBuilders.wildcardQuery("content", "*全文?")
	 * 对一个字段完成查询
	 * 在content字段上是否有*全文?的词条呢？  * 表示多个字符（任意的字符串） ?表示单个字符（例如：我们学习的Lucene全文检）
	 *    * 没有查询到“*全文?”的结果
	 *    * 查询到“*全”的数据结果
	 */
	@Test
	public void testSearch3() throws Exception {
		
		SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.wildcardQuery("content", "*全文?")).get();
		SearchHits hits = searchResponse.getHits();
		System.out.println("查询结果有：" + hits.getTotalHits() + "条");
		searchValue(hits);
		
		client.close();
		
	}
```

```java
/** 3、 查询content词条为“搜索” 内容，使用TermQuery */
	/**
	 * 
	 * 词条查询是Elasticsearch中的一个简单查询。它仅匹配在给定字段中含有该词条的文档，
	 * 而且是确切的、未经分析的词条
	 * elasticsearch提供QueryBuilders.termQuery("content", "全文") 对一个字段完成查询
	 * 在content字段上是否有全文的词条呢？ * 没有查询到“全文”的结果 * 查询到“全”的数据结果
	 */
	@Test
	public void testSearch4() throws Exception {

		SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.termQuery("content", "全文")).get();
		SearchHits hits = searchResponse.getHits();
		System.out.println("查询结果有：" + hits.getTotalHits() + "条");
		searchValue(hits);
		client.close();
	}

```

小结：

ElasticSearch 支持所有Lucene查询，并对其进行简化封装 

```
	WildcardQuery 通配符查询 
    TermQuery 词条查询 
	FuzzyQuery 相似度查询（模糊查询）
	BooleanQuery 布尔查询 
```

1、ElasticSearch提供QueryBuileders.queryStringQuery(搜索内容) 查询方法，对所有字段进行分词查询

```java
SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.queryStringQuery("全面")).get();
```

2、 只想查询content里包含全文 ，使用QueryBuilders.wildcardQuery通配符查询 *任意字符串 ?任意单个字符 

```java
SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.wildcardQuery("content", "*全文?")).get();
```

发现查询不到！！！！ ，涉及词条查询理解， 说明没有词条包含“全文”

3、 查询content词条为“搜索” 内容，使用QueryBuilders.termQuery进行词条查询

```java
SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article")
				.setQuery(QueryBuilders.termQuery("content", "全文")).get();
```

发现查询不到！！！，说明没有搜索“全文”这个词条

这是为什么呢？

词条： 就是将文本内容存入搜索服务器，搜索服务器进行分词之后的内容。

例如：“ElasticSearch是一个基于Lucene的搜索服务器”

​	分词（好的）： ElasticSearch、是、一个、基于、Lucene、搜索、服务、服务器 

​	默认单字分词（差的）： ElasticSearch、 是、一、个、基、于、搜、索

    使用QueryBuileders.queryStringQuery(搜索内容)，搜索“全面” 也能够查询到，这是为什么呢？看图：

![图片52](assets\图片52.png)



#### **九、安装ik分词器**

Lucene的IK分词器早在2012年已经没有维护了，现在我们要使用的是在其基础上维护升级的版本，并且开发为Elasticsearch的集成插件了，与Elasticsearch一起维护升级，版本也保持一致，最新版本：6.4.0

下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

主页地址：https://github.com/medcl/elasticsearch-analysis-ik

![图片25](assets\图片25.png)

##### **9.1.安装**

/home/yunwu/elasticsearch/plugins

创建名为`ik-analyzer`目录：

```shell
[yunwu@quanyi plugins]$ mkdir ik-analyzer
```

上传课前资料中的zip包(elasticsearch-analysis-ik-6.4.0.zip) ,到Elasticsearch目录的plugins/ik-analyzer目录中：

![图片26](assets\图片26.png)

使用unzip命令解压：

```shell
[yunwu@quanyi ik-analyzer]$ unzip elasticsearch-analysis-ik-6.4.0.zip
```

使用rm命令删除elasticsearch-analysis-ik-6.4.0.zip

```shell
[yunwu@quanyi ik-analyzer]$ rm -rf elasticsearch-analysis-ik-6.4.0.zip 
```

然后重启elasticsearch：

![图27](assets\图27.png)



##### **9.2 测试**

大家先不管语法，使用Advanced REST client工具，我们先测试一波。 Advanced REST client工具的安装在资料里。![图片53](assets\图片53.png)

控制台输入下面的请求：

使用POST请求 和 _analyze端点

```json
{
  "analyzer": "ik_max_word",
  "text":     "我是中国人"
}
```

运行得到结果：

```json
{
  "tokens": [
    {
      "token": "我",
      "start_offset": 0,
      "end_offset": 1,
      "type": "CN_CHAR",
      "position": 0
    },
    {
      "token": "是",
      "start_offset": 1,
      "end_offset": 2,
      "type": "CN_CHAR",
      "position": 1
    },
    {
      "token": "中国人",
      "start_offset": 2,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 2
    },
    {
      "token": "中国",
      "start_offset": 2,
      "end_offset": 4,
      "type": "CN_WORD",
      "position": 3
    },
    {
      "token": "国人",
      "start_offset": 3,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 4
    }
  ]
}
```

IK 的 ik_max_word 和 ik_smart 两种分词策略。根据 IK 的文档，二者区别如下：

- ik_max_word：会将文本做最细粒度的拆分，例如「中华人民共和国国歌」会被拆分为「中华人民共和国、中华人民、中华、华人、人民共和国、人民、人、民、共和国、共和、和、国国、国歌」，会穷尽各种可能的组合；
- ik_smart：会将文本做最粗粒度的拆分，例如「中华人民共和国国歌」会被拆分为「中华人民共和国、国歌」；



#### **十、ElasticSearch 常用编程操作**

​	在ElasticSearch没有索引情况下，插入文档，默认创建索引和索引映射 （但是无法使用ik分词器）

要想使用IK分词器，需要重新创建索引。

##### **10.1  创建索引和删除索引**

创建索引：

```java
@Test
public void createIndex() {
  client.admin().indices().prepareCreate("blog2").get();
  client.close();
}
```

默认创建好索引，mappings为空  ![图片54](assets\图片54.png)

删除索引：

```java
@Test
public void deleteIndex() {
  client.admin().indices().prepareDelete("blog2").get();
  client.close();
}
```

##### **10.2映射配置**

索引有了，接下来肯定是添加数据。不过数据存储到索引库中，必须指定一些相关属性，在学习Lucene中我们都见到过，包括到不限于：

- 字段的数据类型
- 是否要存储
- 是否要索引
- 是否分词
- 分词器是什么

只有配置清楚，Elasticsearch才会帮我们进行索引库的创建（不一定）

###### **10.2.1 创建映射字段**

> 语法

请求方式依然是PUT

```
PUT /索引库名/_mapping/类型名称
{
  "properties": {
    "字段名": {
      "type": "类型",
      "index": true，
      "store": true，
      "analyzer": "分词器"
    }
  }
}
```

- 类型名称：就是前面将的type的概念，类似于数据库中的不同表
   字段名：任意填写	，可以指定许多属性，例如：
  - type：类型，可以是text、long、short、date、integer、object等
  - index：是否索引，默认为true
  - store：是否存储，默认为false
  - analyzer：分词器，这里的`ik_max_word`即使用ik分词器


###### **10.2.2 创建映射**

```java
@Test
	// 映射操作 创建映射之前必须先创建索引
	public void createMapping() throws Exception {
		
		XContentBuilder builder = XContentFactory.jsonBuilder()
				.startObject()
					.startObject("article")
						.startObject("properties")
							.startObject("id").field("type", "integer").field("store", true).endObject()
							.startObject("title").field("type", "text").field("store", true).field("analyzer", "ik_max_word").endObject()
							.startObject("content").field("type", "text").field("store", true).field("analyzer", "ik_max_word").endObject()
						.endObject()
					.endObject()
				.endObject();
		PutMappingRequest mapping = Requests.putMappingRequest("blog2").type("article").source(builder);
		
		client.admin().indices().putMapping(mapping).get();

		// 关闭连接
		client.close();
		
	}
```

通过head插件查看:

 ![图片55](assets\图片55.png)

###### **10.2.3文档相关操作：**

回顾：直接在XContentBuilder中构建json数据，建立文档 。

```java
@Test
	public void  prepareData() throws Exception {
		// 描述json 数据
		/*
		 * {id:xxx, title:xxx, content:xxx}
		 */
		XContentBuilder builder = XContentFactory.jsonBuilder()
				.startObject()
				.field("id", 1)
				.field("title", "ElasticSearch是一个基于Lucene的搜索服务器")
				.field("content",
						"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。")
				.endObject();
		client.prepareIndex("blog2", "article", "1").setSource(builder).get();
		
		client.close();
	}
```

![图片56](assets\图片56.png)



使用Jackson建立文档数据：

​	Jackson 是一个 Java 用来处理 JSON 格式数据的类库，性能非常好。Jackson可以轻松的将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象。Jackson库于2012.10.8号发布了最新的2.1版。Jackson源码目前托管于GitHub，地址：https://github.com/FasterXML/。

​	Jackson 2.x介绍，Jackson 2.x版提供了三个JAR包供下载：

1. Core库：streaming parser/generator，即流式的解析器和生成器。
   下载：
   http://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.1.0/jackson-core-2.1.0.jar
2. Databind库：ObjectMapper, Json Tree Model，即对象映射器，JSON树模型。
   下载：
   http://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.1.0/jackson-databind-2.1.0.jar
3. Annotations库：databinding annotations，即带注释的数据绑定包。
   下载：
   http://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.1.0/jackson-annotations-2.1.0.jar

从Jackson 2.0起，
核心组件包括：jackson-annotations、jackson-core、jackson-databind。
数据格式模块包括：Smile、CSV、XML、YAML。

本课程开发使用的Jackson的maven依赖：

```xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-core</artifactId>
	<version>2.8.1</version>
</dependency>
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.8.1</version>
</dependency>
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-annotations</artifactId>
	<version>2.8.1</version>
</dependency>
```

对一个已经存在对象，转换为json ，建立文档

创建包，com.itheima.elasticsearch.domain

编写set，get，toString方法

```java
package com.itheima.elasticsearch.domain;
public class Article {

private Integer id;
	private String title;
	private String content;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	@Override
	public String toString() {
		return "Article [id=" + id + ", title=" + title + ", content=" + content + "]";
	}
}
```



使用Jackson方式插入数据：

```java
	@Test
	// 文档相关操作
	public void prepareData2() throws IOException, InterruptedException,
			ExecutionException {
		// 描述json 数据
		/*
		 * {id:xxx, title:xxx, content:xxx}
		 */
		Article article = new Article();
		article.setId(2);
		article.setTitle("搜索工作其实很快乐");
		article.setContent(
				"我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
		ObjectMapper objectMapper = new ObjectMapper();		
		// 建立文档
		client.prepareIndex("blog2", "article", article.getId().toString())				.setSource(objectMapper.writeValueAsString(article),XContentType.JSON).get();
		// 关闭连接
		client.close();
	}
```



修改文档：

方式一： 使用prepareUpdate

```java
//方式一 prepareUpdate
	@Test
	public void prepareUpdateData() throws Exception {
		
		Article article = new Article();
		article.setId(2);
		article.setTitle("更新：搜索工作其实很快乐");
		article.setContent(
				"更新：我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
		ObjectMapper objectMapper = new ObjectMapper();
		
		UpdateResponse updateResponse = client.prepareUpdate("blog2", "article", article.getId().toString()).setDoc(objectMapper.writeValueAsString(article), XContentType.JSON).get();
		// 获取响应的信息
		System.out.println("索引名称：" + updateResponse.getIndex());
		System.out.println("文档类型：" + updateResponse.getType());
		System.out.println("ID：" + updateResponse.getId());
		System.out.println("版本：" + updateResponse.getVersion());
		System.out.println("是否更新成功：" + updateResponse.status());
		// 关闭连接
		client.close();
		
	}
```



方式二：使用update

```java
// 方式二 Update
@Test
public void prepareUpdateData2() throws Exception {

	Article article = new Article();
	article.setId(2);
	article.setTitle("更新2：搜索工作其实很快乐");
	article.setContent(
			"更新2：我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
	ObjectMapper objectMapper = new ObjectMapper();

	UpdateResponse updateResponse = client.update(new UpdateRequest("blog2", "article", article.getId().toString())
			.doc(objectMapper.writeValueAsString(article),XContentType.JSON)).get();
	
	// 获取响应的信息
	System.out.println("索引名称：" + updateResponse.getIndex());
	System.out.println("文档类型：" + updateResponse.getType());
	System.out.println("ID：" + updateResponse.getId());
	System.out.println("版本：" + updateResponse.getVersion());
	System.out.println("是否更新成功：" + updateResponse.status());
	// 关闭连接
	client.close();
}
```



删除文档：

方式一：使用prepareDelete 

```java
@Test
public void prepareDeleteDate() {
  client.prepareDelete("blog2", "article", "2").get();
  client.close();
}
```

方式二：使用delete

```java
@Test
public void deleteDate() throws Exception {
  client.delete(new DeleteRequest("blog2", "article", "2")).get();
  client.close();
}
```

###### **10.2.4 搜索数据**

查询所有：

```java
@Test
public void searchquery() {
  //查询所有
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.matchAllQuery()).get();
  SearchHits hits = searchResponse.getHits();
  searchValue(hits);
  client.close();
}
```

QueryBuileders.queryStringQuery(搜索内容) 查询方法:

```java
/**
	 * 1、ElasticSearch提供QueryBuileders.queryStringQuery(搜索内容) 查询方法，对所有字段进行分词查询
	 */
@Test
public void queryStringQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.queryStringQuery("搜索")).get();

  SearchHits hits = searchResponse.getHits();
  searchValue(hits);

  client.close();
}
```

QueryBuilders.wildcardQuery模糊查询

```java
/**
	 * 2、 只想查询content里包含全文 ，使用QueryBuilders.wildcardQuery模糊查询 *任意字符串 ?任意单个字符
	 */
@Test
public void wildcardQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.wildcardQuery("content", "*搜索*")).get();
  SearchHits hits = searchResponse.getHits();
  searchValue(hits);

  client.close();
}
```

QueryBuilders.termQuery词条查询

```java
 /**3、 查询content词条为“搜索” 内容，使用TermQuery */
 @Test
public void termQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.termQuery("content", "搜索")).get();
  SearchHits hits = searchResponse.getHits();
  searchValue(hits);

  client.close();
}
```

##### **10.3 IK分词器，自定义词库**

添加文档数据到elasticsearch，并在content字段添加“传智播客”

```java
@Test
public void insertDataToElS() throws Exception{
  Article article = new Article();
  article.setId(2);
  article.setTitle("搜索工作其实很快乐");
  article.setContent(
    " 传智播客 希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
  ObjectMapper objectMapper = new ObjectMapper();

  // 建立文档
  client.prepareIndex("blog2", "article", article.getId().toString())
    .setSource(objectMapper.writeValueAsString(article), XContentType.JSON).get();
  // 关闭连接
  client.close();
}
```

使用词条查询，对“传智播客”进行搜索

```java
@Test
public void termQueryByIK() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.termQuery("content", "传智播客")).get();
  SearchHits hits = searchResponse.getHits(); // 获取命中次数，查询结果有多少对象
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
}
```

查询结果有：0条。使用“传”，“智”，“播”，“客”任意一个字都能查询出结果。其实“传智播客”使用IK中文分词器的时候，进行单字分词了。

我们可以使用Advanced REST client工具，进行测试。

![图片57](assets\图片57.png)

![图片58](assets\图片58.png)

这个时候如果我们要把“传智播客”当做一个词条查询，我们就要使用自定义词库，如何使用自定义词库呢？

首先进入elasticsearch/plugins下的ik-analyzer/config目录

```shell
[yunwu@quanyi ~]$ cd /home/yunwu/elasticsearch/plugins/ik-analyzer/config
[yunwu@quanyi config]$ pwd
/home/yunwu/elasticsearch/plugins/ik-analyzer/config
[yunwu@quanyi config]$ 
```

查看 IKAnalyzer.cfg.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict"></entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords"></entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

可以配置自己的自定义词库，进入

```shell
[yunwu@quanyi ~]$ cd /home/yunwu/elasticsearch/plugins/ik-analyzer/config
```

创建一个文件夹用来存放自定义词库的文件

```shell
[yunwu@quanyi config]$ mkdir custom
[yunwu@quanyi config]$ ll
total 8264
drwxrwxr-x. 2 yunwu yunwu    4096 Sep 19 02:07 custom
-rw-r--r--. 1 yunwu yunwu 5225922 Aug 26 17:52 extra_main.dic
-rw-r--r--. 1 yunwu yunwu   63188 Aug 26 17:52 extra_single_word.dic
-rw-r--r--. 1 yunwu yunwu   63188 Aug 26 17:52 extra_single_word_full.dic
-rw-r--r--. 1 yunwu yunwu   10855 Aug 26 17:52 extra_single_word_low_freq.dic
-rw-r--r--. 1 yunwu yunwu     156 Aug 26 17:52 extra_stopword.dic
-rw-r--r--. 1 yunwu yunwu     625 Aug 26 17:52 IKAnalyzer.cfg.xml
-rw-r--r--. 1 yunwu yunwu 3058510 Aug 26 17:52 main.dic
-rw-r--r--. 1 yunwu yunwu     123 Aug 26 17:52 preposition.dic
-rw-r--r--. 1 yunwu yunwu    1824 Aug 26 17:52 quantifier.dic
-rw-r--r--. 1 yunwu yunwu     164 Aug 26 17:52 stopword.dic
-rw-r--r--. 1 yunwu yunwu     192 Aug 26 17:52 suffix.dic
-rw-r--r--. 1 yunwu yunwu     752 Aug 26 17:52 surname.dic
```

在custom文件夹下上传创建自己的自定义词语的文件mydict.dic，同时要把自定义词语写进文件要以UTF-8无BOM格式编码保存，

上传到/home/yunwu/elasticsearch/plugins/ik-analyzer/config/custom/。

![图片59](assets\图片59.png)



```shell
[yunwu@quanyi ~]$ cd /home/yunwu/elasticsearch/plugins/ik-analyzer/config
[yunwu@quanyi config]$ vim IKAnalyzer.cfg.xml
```

配置：

```shell
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">custom/mydict.dic;custom/single_word_low_freq.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">custom/ext_stopword.dic</entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

从启elasticsearch服务

使用Advanced REST client工具，进行测试。

![图片57](assets\图片57.png)  ![图片60](assets\图片60.png)

自定义词库分词成功！

再次使用词条查询，对“传智播客”进行搜索

```java
@Test
public void termQueryByIK() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.termQuery("content", "传智播客")).get();
  SearchHits hits = searchResponse.getHits(); // 获取命中次数，查询结果有多少对象
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
}
```

查询结果有：1条

##### **10.4 elasticsearch中的各种查询**

​	matchAllQuery()匹配所有文件match_all查询是Elasticsearch中最简单的查询之一。它使我们能够匹配索引中的所有文件。

​	解析字符串，相比其他可用的查询，query_string查询支持全部的Apache Lucene查询语法针对多字段的query_string查询。

​	通配符查询（wildcardQuery）匹配多个字符，?匹配1个字符注意：避免开始, 会检索大量内容造成效率缓慢。

​	词条查询是Elasticsearch中的一个简单查询。它仅匹配在给定字段中含有该词条的文档，而且是确切的、未经分析的词条termQuery("key", obj) 完全匹配termsQuery("key", obj1, obj2..)   一次匹配多个值，只有有一个值是正确的，就可以查询出数据。

​	字段匹配查询时，Elasticsearch将对一个字段选择合适的分析器，所以可以确定，传给match查询的词条将被建立索引时相同的分析器处理。matchQuery("key", Obj) 单个匹配, field不支持通配符, 前缀具高级特性match查询把query参数中的值拿出来，加以分析，然后构建相应的查询。使用match查询。

```java
@Test
public void matchQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.matchQuery("content", "搜索")).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```



```java
@Test
public void multiMatchQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.multiMatchQuery("搜索", "article","title")).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```



只查询ID

```java
@Test
public void idsQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.idsQuery().addIds("1")).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```

相识度查询

fuzzy查询是模糊查询之一，它基于编辑距离算法来匹配文档

```java
@Test
public void fuzzyQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.fuzzyQuery("content", "elasticsearxx")).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```



范围查询

范围查询使我们能够找到在某一字段值在某个范围里的文档，字段可以是数值型，也可以是基于字符串的

```java
@Test
public void rangeQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    //.setQuery(QueryBuilders.rangeQuery("id").gte("1").lte("2")).get();
    .setQuery(QueryBuilders.rangeQuery("id").from("1").to("2")).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```

跨度查询

下面代码表示，从首字母开始，查询content字段=问题的数据，问题前面的词为300个，可以测试30看是否能查询出数据。

```java
@Test
public void spanFirstQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.spanFirstQuery(QueryBuilders.spanTermQuery("content", "发布"), 300)).get();
  SearchHits hits = searchResponse.getHits();

  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```



排序查询

```java
@Test
public void sortQuery() {
  SearchResponse searchResponse = client.prepareSearch("blog2").setTypes("article")
    .setQuery(QueryBuilders.matchAllQuery()).addSort("id", SortOrder.DESC).get();
  SearchHits hits = searchResponse.getHits();

  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);
  client.close();
}
```

**==注：排序的索引中的排序字段不能为null==**

##### **10.5  查询文档分页操作**

###### **10.5.1 批量向数据表 插入100条记录  准备数据**



```java
@Test
public void createDocument100() throws Exception {
  ObjectMapper objectMapper = new ObjectMapper();

  for (int i = 1; i <= 100; i++) {
    Article article = new Article();
    article.setId(i);
    article.setTitle(i + "搜索工作其实很快乐");
    article.setContent(i
                       + "我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
    client.prepareIndex("blog3", "article", article.getId().toString())
      .setSource(objectMapper.writeValueAsString(article), XContentType.JSON).get();

  }
  client.close();
}
```

###### 

###### **10.5.2 查询所有elasticsearch默认输出10条**

```java
// 查询分页
@Test
public void searchRequestBuilder() {
  SearchRequestBuilder searchRequestBuilder = client.prepareSearch("blog3").setTypes("article")
    .setQuery(QueryBuilders.matchAllQuery());

  SearchResponse searchResponse = searchRequestBuilder.get();
  SearchHits hits = searchResponse.getHits();
  searchValue(hits);
  client.close();
}
```



###### **10.5.3  searchRequestBuilder 的 setFrom【从0开始】 和 setSize【查询多少条记录】方法实现**

```java
@Test
public void searchRequestBuilder() {
  SearchRequestBuilder searchRequestBuilder = client.prepareSearch("blog3").setTypes("article")
    .setQuery(QueryBuilders.matchAllQuery());
  // 查询第2页数据，每页20条
  //setFrom()：从第几条开始检索，默认是0。
  //setSize():每页最多显示的记录数。
  searchRequestBuilder.setFrom(20).setSize(20);

  SearchResponse searchResponse = searchRequestBuilder.get();
  SearchHits hits = searchResponse.getHits();
  searchValue(hits);
  client.close();
}
```



#### **十一、得分（加权）**

​	随着应用程序的增长，提高搜索质量的需求也进一步增大。我们把它叫做搜索体验。我们需要知道什么对用户更重要，关注用户如何使用搜索功能。这导致不同的结论，例如，有些文档比其他的更重要，或特定查询需强调一个字段而弱化其他字段。这就是可以用到加权的地方。

在Query和Field中可以设置加权，创建3条数据，通过加权影响我们的数据结果和得分

創建索引

```java
/**
 * testCreateIndex_boost 創建索引
 */
@Test
public void testCreateIndex_boost() {
  client.admin().indices().prepareCreate("blog4").get();
  client.close();
}
```



创建映射

```java
/**
 * 创建映射
 */
@Test
public void testCreateIndexMapping_boost() throws Exception {
  XContentBuilder builder = XContentFactory.jsonBuilder()
    .startObject()
    .startObject("article")
    .startObject("properties")
    .startObject("id").field("type", "integer").field("store", true).endObject()
    .startObject("title").field("type", "text").field("store", true).field("analyzer", "ik_max_word").endObject()
    .startObject("content").field("type", "text").field("store", true).field("analyzer", "ik_max_word").endObject()
    .startObject("comment").field("type", "text").field("store", true).field("analyzer", "ik_max_word").endObject()
    .endObject()
    .endObject()
    .endObject();
  PutMappingRequest putMappingRequest = Requests.putMappingRequest("blog4").type("article").source(builder);

  client.admin().indices().putMapping(putMappingRequest).get();

  client.close();
}
```



创建文档:

```java
/** 创建文档 */
@Test
public void createDocument_boost() throws Exception {
  Article article = new Article();
  //article.setComment("我们学习Elasticsearch搜索引擎服务器");// 有搜索
  //article.setId(1);
  //article.setTitle("搜索引擎服务器"); // 有搜索
  //article.setContent("基于restful的数据风格"); // 无搜索

  article.setId(2);
  article.setTitle("什么是Elasticsearch"); // 无搜索
  article.setContent("Elasticsearch搜索引擎服务器"); // 有搜索
  article.setComment("Elasticsearch封装了lucene");// 无搜索

  ObjectMapper objectMapper = new ObjectMapper();
  String source = objectMapper.writeValueAsString(article);
  System.out.println("source:" + source);

  IndexResponse indexResponse = client.prepareIndex("blog4", "article", article.getId().toString())
    .setSource(source, XContentType.JSON).get();
  // 获取响应的信息
  System.out.println("索引名称：" + indexResponse.getIndex());
  System.out.println("文档类型：" + indexResponse.getType());
  System.out.println("ID：" + indexResponse.getId());
  System.out.println("版本：" + indexResponse.getVersion());
  System.out.println("是否创建成功：" + indexResponse.status());
  client.close();
}
```



测试:

```java
@Test
public void testQueryString_boost() throws Exception {
  /*
		 * SearchResponse searchResponse =
		 * client.prepareSearch("blog4").setTypes("article")
		 * .setQuery(QueryBuilders.matchAllQuery()).get();
		 */

  SearchResponse searchResponse = client.prepareSearch("blog4").setTypes("article")
    .setQuery(QueryBuilders.queryStringQuery("搜索").field("content",10.0f).field("title",5.0f).field("comment")).get();

  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  System.out.println("结果中最高分：" + hits.getMaxScore());
  searchValue(hits);
  client.close();
}

private void searchValue(SearchHits hits) {
  Iterator<SearchHit> iterator = hits.iterator();

  while (iterator.hasNext()) {
    // 每个查询对象
    SearchHit searchHit = iterator.next();
    // 获取字符串格式打印
    String sourceAsString = searchHit.getSourceAsString();
    System.out.println(sourceAsString);
    System.out.println("每条得分："+searchHit.getScore());
    System.out.println("id:" + searchHit.getId());
    System.out.println("Index:" + searchHit.getIndex());
    System.out.println("type:" + searchHit.getType());
    System.out.println("文档id:" + searchHit.getSourceAsMap().get("id"));
    System.out.println("文档title:" + searchHit.getSourceAsMap().get("title"));
    System.out.println("文档content:" + searchHit.getSourceAsMap().get("content"));

  }
}
```

也可以在Field字段的映射中定义加权：

![图片61](assets\图片61.png)

可以在映射中添加： ![图片62](assets\图片62.png)

等同于：

![图片63](assets\图片63.png)

#### **十二、过滤器**

​	我们已经介绍了如何使用不同的条件和查询来构建查询并搜索数据。我们还熟知了评分，它告诉我们在给定的查询中，哪些文档更重要以及查询文本如何影响排序。然而，有时我们可能要在不影响最后分数的情况下，选择索引中的某个子集，这就要使用过滤器。

​	如果可以，应该尽可能使用过滤器。过滤器不影响评分，而得分计算让搜索变得复杂，而且需要CPU资源。另一方面，过滤是一种相对简单的操作。由于过滤应用在整个索引的内容上，过滤的结果独立于找到的文档，也独立于文档之间的关系。过滤器很容易被缓存，从而进一步提高过滤查询的整体性能。

##### **12.1范围过滤器：**

```java
/**范围过滤器*/
@Test
public void testRangeQuery() {

  SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article").setQuery(QueryBuilders.rangeQuery("id").from(1).to(2)).get();

  SearchHits hits = searchResponse.getHits();
  searchValue(hits);
  client.close();
}
```

##### **12.2 布尔过滤器**

```java
@Test
public void testFilter() throws Exception{
  SearchResponse searchResponse = client.prepareSearch("blog4").setTypes("article").setQuery(QueryBuilders.boolQuery().must(QueryBuilders.termQuery("title", "搜索")).must(QueryBuilders.termQuery("comment", "搜索"))).get();
  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");		
  searchValue(hits);
  client.close();
}
```

##### **12.3 添加缓存：**

 	过滤器的缓存，关于过滤器最后要提到的是缓存。缓存加速了使用过滤器的查询，代价是第一次执行过滤器时的内存成本和查询时间。因此，缓存的最佳选择是那些可以重复使用的过滤器，例如，经常会使用并包括参数值的那些。

有些过滤器不支持_cache参数，因为他们的结果总是被缓存。默认情况下有这些：

- exists
- missing
- range
- term
- terms

还有一些过滤器缓存是无效的。默认情况下有这些：

ids过滤器

match_all过滤器

limit过滤器



示例：

在范围过滤器上添加缓存

```java
/**范围过滤器中添加缓存*/
@Test
public void testRangeQueryAddCache() {

  SearchResponse searchResponse = client.prepareSearch("blog").setTypes("article").setQuery(QueryBuilders.rangeQuery("id").from(1).to(2)).setRequestCache(true).get();

  SearchHits hits = searchResponse.getHits();
  System.out.println("查询结果有：" + hits.getTotalHits() + "条");
  searchValue(hits);

  client.close();
}
```









