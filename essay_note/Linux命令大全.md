# Linux命令总结

## 基本操作

### Linux关机 | 重启

```Linux
# 关机
shutdown -h now

# 重启
shutdown -r now
```

### 查看系统  | CPU信息

```
# 查看系统内核信息
uname -a

# 查看系统内核版本
cat /proc/version

# 查看当前用户环境变量
env

# 查看cpu信息
cat /proc/cpuinfo

# 查看有几个逻辑cpu, 包括cpu型号
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c

# 查看有几颗cpu,每颗分别是几核
cat /proc/cpuinfo | grep physical | uniq -c

# 查看当前CPU运行在32bit还是64bit模式下, 如果是运行在32bit下也不代表CPU不支持64bit
getconf LONG_BIT

# 结果大于0, 说明支持64bit计算. lm指long mode, 支持lm则是64bit
cat /proc/cpuinfo | grep flags | grep ' lm ' | wc -l
```

### 建立软连接 | 硬连接

```
# 给路劲创建软链接，为/usr/local/jdk1.8/路劲创建软链接jdk，如果/usr/local/jdk1.8/丢失，jdk将失效
ln -s /usr/local/jdk1.8/ jdk

# 为指定的源文件创建快捷方式（默认为硬链接形式）
ln anaconda-ks.cfg ana.cfg
```

### rpm相关

```
# 查看是否通过rpm安装了该软件
rpm -qa | grep 软件名
```

### sshkey相关

```
# 创建sshkey
ssh-keygen -t rsa -C your_email@example.com

#id_rsa.pub 的内容拷贝到要控制的服务器的 home/username/.ssh/authorized_keys 中,如果没有则新建(.ssh权限为700, authorized_keys权限为600)
```

### 命令重命名

```
# 在各个用户的.bash_profile中添加重命名配置
alias ll='ls -alF'
```

### 同步服务器时间

```
sudo ntpdate -u ntp.api.bz
```

### 后台运行命令

```
# 后台运行,并且有nohup.out输出
nohup xxx &

# 后台运行, 不输出任何日志
nohup xxx > /dev/null &

# 后台运行, 并将错误信息做标准输出到日志中 
nohup xxx >out.log 2>&1 &
```

### 强制活动用户退出

```
# 命令来完成强制活动用户退出.其中TTY表示终端名称
pkill -kill -t [TTY]
```

### 查看命令路径

```
which <命令>
```

### 查看进程所有打开最大fd数

```
ulimit -n
```

### 配置 dns

```
vim /etc/resolv.conf
```

### nslookup 查看域名路由表

```
nslookup google.com
```

### last 最近登录信息列表

```
# 最近登录的5个账号
last -n 5
```

### 设置固定 ip

```
ifconfig em1  192.168.5.177 netmask 255.255.255.0
```

### 查看进程内加载的环境变量

```
# 也可以去 cd /proc 目录下, 查看进程内存中加载的东西
ps eww -p  XXXXX(进程号)
```

### 查看进程树找到服务器进程

```
ps auwxf
```

### 查看进程启动路径

```
cd /proc/xxx(进程号)
ls -all
# cwd对应的是启动路径
```

### 添加用户 | 配置sudo权限

```
# 新增用户
useradd 用户名
passwd 用户名

#增加sudo权限
vim /etc/sudoers
# 修改文件里面的
# root    ALL=(ALL)       ALL
# 用户名 ALL=(ALL)       ALL
```

### 强制关闭进程名包含xxx的所有进程

```
ps aux|grep xxx | grep -v grep | awk '{print $2}' | xargs kill -9
```





## 磁盘 | 文件 | 目录相关操作

### vim操作

```
# normal模式下 g表示全局, x表示查找的内容, y表示替换后的内容
:%s/x/y/g
# normal模式下 光标移到行首(数字0)
0
# normal模式下 光标移至行尾
$ 
# normal模式下 跳到文件最后
shift + g
# normal模式下 跳到文件头
gg

# 显示行号
:set nu

# 去除行号
:set nonu

# 检索 从头检索, 按n查找下一个
/xxx(检索内容)  
# 检索 从尾部检索
?xxx(检索内容) 
```

### 打开只读文件,修改后需要保存时

```
# 在normal模式下(不用切换用户即可保存的方式)
:w !sudo tee %
```

### 查看磁盘 ,文件目录基本信息

```
# 查看磁盘挂载情况
mount

# 查看磁盘分区信息
df

# 查看目录及子目录大小
du -H -h

# 查看当前目录下各个文件, 文件夹占了多少空间, 不会递归
du -sh *
```

### wc命令

```
# 查看文件里有多少行
wc -l filename

# 看文件里有多少个word
wc -w filename

# 文件里最长的那一行是多少个字
wc -L filename

# 统计字节数
wc -c
```







## 常用压缩 | 解压缩命令

### 压缩命令

```
tar czvf xxx.tar 压缩目录

zip -r xxx.zip 压缩目录
```

### 解压缩命令

```
tar zxvf xxx.tar

# 解压到指定文件夹
tar zxvf xxx.tar -C /xxx/yyy/

unzip xxx.zip
```





## 文件相关命令

### 变更文件所属用户, 用户组

```
chown eagleye.eagleye xxx.log
```

### cp | scp | mkdir

```
#复制
cp xxx.log

# 复制并强制覆盖同名文件
cp -f xxx.log

# 复制文件夹
cp -r xxx(源文件夹) yyy(目标文件夹)

# 远程复制
scp -P ssh端口 username@10.10.10.101:/home/username/xxx /home/xxx

# 级联创建目录
mkdir -p /xxx/yyy/zzz

# 批量创建文件夹, 会在test,main下都创建java, resources文件夹
mkdir -p src/{test,main}/{java,resources}
```

