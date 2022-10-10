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

#### 第一部分：一般模式可用的光标移动、复制粘贴、搜索替换等

| 移动光标的方法                                               |                                                              |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| h 或 向左箭头键(←)                                           | 光标向左移动一个字符                                         |
| j 或 向下箭头键(↓)                                           | 光标向下移动一个字符                                         |
| k 或 向上箭头键(↑)                                           | 光标向上移动一个字符                                         |
| l 或 向右箭头键(→)                                           | 光标向右移动一个字符                                         |
| 如果你将右手放在键盘上的话，你会发现 hjkl 是排列在一起的，因此可以使用这四个按钮来移动光标。 如果想要进行多次移动的话，例如向下移动 30 行，可以使用 "30j" 或 "30↓" 的组合按键， 亦即加上想要进行的次数(数字)后，按下动作即可！ |                                                              |
| [Ctrl] + [f]                                                 | 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)          |
| [Ctrl] + [b]                                                 | 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)           |
| [Ctrl] + [d]                                                 | 屏幕『向下』移动半页                                         |
| [Ctrl] + [u]                                                 | 屏幕『向上』移动半页                                         |
| +                                                            | 光标移动到非空格符的下一行                                   |
| -                                                            | 光标移动到非空格符的上一行                                   |
| n<space>                                                     | 那个 n 表示『数字』，例如 20 。按下数字后再按空格键，光标会向右移动这一行的 n 个字符。例如 20<space> 则光标会向后面移动 20 个字符距离。 |
| 0 或功能键[Home]                                             | 这是数字『 0 』：移动到这一行的最前面字符处 (常用)           |
| $ 或功能键[End]                                              | 移动到这一行的最后面字符处(常用)                             |
| H                                                            | 光标移动到这个屏幕的最上方那一行的第一个字符                 |
| M                                                            | 光标移动到这个屏幕的中央那一行的第一个字符                   |
| L                                                            | 光标移动到这个屏幕的最下方那一行的第一个字符                 |
| G                                                            | 移动到这个档案的最后一行(常用)                               |
| nG                                                           | n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20 行(可配合 :set nu) |
| gg                                                           | 移动到这个档案的第一行，相当于 1G 啊！ (常用)                |
| n<Enter>                                                     | n 为数字。光标向下移动 n 行(常用)                            |
| 搜索替换                                                     |                                                              |
| /word                                                        | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！ (常用) |
| ?word                                                        | 向光标之上寻找一个字符串名称为 word 的字符串。               |
| n                                                            | 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ |
| N                                                            | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。 例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird 。 |
| 使用 /word 配合 n 及 N 是非常有帮助的！可以让你重复的找到一些你搜寻的关键词！ |                                                              |
| :n1,n2s/word1/word2/g                                        | n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为 word2 ！举例来说，在 100 到 200 行之间搜寻 vbird 并取代为 VBIRD 则： 『:100,200s/vbird/VBIRD/g』。(常用) |
| **:1,$s/word1/word2/g** 或 **:%s/word1/word2/g**             | 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！(常用) |
| **:1,$s/word1/word2/gc** 或 **:%s/word1/word2/gc**           | 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！且在取代前显示提示字符给用户确认 (confirm) 是否需要取代！(常用) |
| 删除、复制与贴上                                             |                                                              |
| x, X                                                         | 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) (常用) |
| nx                                                           | n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。 |
| dd                                                           | 剪切游标所在的那一整行(常用)，用 p/P 可以粘贴。              |
| ndd                                                          | n 为数字。剪切光标所在的向下 n 行，例如 20dd 则是剪切 20 行(常用)，用 p/P 可以粘贴。 |
| d1G                                                          | 删除光标所在到第一行的所有数据                               |
| dG                                                           | 删除光标所在到最后一行的所有数据                             |
| d$                                                           | 删除游标所在处，到该行的最后一个字符                         |
| d0                                                           | 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符      |
| yy                                                           | 复制游标所在的那一行(常用)                                   |
| nyy                                                          | n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用) |
| y1G                                                          | 复制游标所在行到第一行的所有数据                             |
| yG                                                           | 复制游标所在行到最后一行的所有数据                           |
| y0                                                           | 复制光标所在的那个字符到该行行首的所有数据                   |
| y$                                                           | 复制光标所在的那个字符到该行行尾的所有数据                   |
| p, P                                                         | p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！ 举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20 行之后，亦即由 21 行开始贴。但如果是按下 P 呢？ 那么原本的第 20 行会被推到变成 30 行。 (常用) |
| J                                                            | 将光标所在行与下一行的数据结合成同一行                       |
| c                                                            | 重复删除多个数据，例如向下删除 10 行，[ 10cj ]               |
| u                                                            | 复原前一个动作。(常用)                                       |
| [Ctrl]+r                                                     | 重做上一个动作。(常用)                                       |
| 这个 u 与 [Ctrl]+r 是很常用的指令！一个是复原，另一个则是重做一次～ 利用这两个功能按键，你的编辑，嘿嘿！很快乐的啦！ |                                                              |
| .                                                            | 不要怀疑！这就是小数点！意思是重复前一个动作的意思。 如果你想要重复删除、重复贴上等等动作，按下小数点『.』就好了！ (常用) |

#### 第二部分：一般模式切换到编辑模式的可用的按钮说明

| 进入输入或取代的编辑模式                                     |                                                              |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| i, I                                                         | 进入输入模式(Insert mode)： i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。 (常用) |
| a, A                                                         | 进入输入模式(Insert mode)： a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』。(常用) |
| o, O                                                         | 进入输入模式(Insert mode)： 这是英文字母 o 的大小写。o 为在目前光标所在的下一行处输入新的一行； O 为在目前光标所在的上一行处输入新的一行！(常用) |
| r, R                                                         | 进入取代模式(Replace mode)： r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；(常用) |
| 上面这些按键中，在 vi 画面的左下角处会出现『--INSERT--』或『--REPLACE--』的字样。 由名称就知道该动作了吧！！特别注意的是，我们上面也提过了，你想要在档案里面输入字符时， 一定要在左下角处看到 INSERT 或 REPLACE 才能输入喔！ |                                                              |
| [Esc]                                                        | 退出编辑模式，回到一般模式中(常用)                           |

#### 第三部分：一般模式切换到指令行模式的可用的按钮说明

| 指令行的储存、离开等指令                                     |                                                              |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| :w                                                           | 将编辑的数据写入硬盘档案中(常用)                             |
| :w!                                                          | 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！ |
| :q                                                           | 离开 vi (常用)                                               |
| :q!                                                          | 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。    |
| 注意一下啊，那个惊叹号 (!) 在 vi 当中，常常具有『强制』的意思～ |                                                              |
| :wq                                                          | 储存后离开，若为 :wq! 则为强制储存后离开 (常用)              |
| ZZ                                                           | 这是大写的 Z 喔！如果修改过，保存当前文件，然后退出！效果等同于(保存并退出) |
| ZQ                                                           | 不保存，强制退出。效果等同于 **:q!**。                       |
| :w [filename]                                                | 将编辑的数据储存成另一个档案（类似另存新档）                 |
| :r [filename]                                                | 在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面 |
| :n1,n2 w [filename]                                          | 将 n1 到 n2 的内容储存成 filename 这个档案。                 |
| :! command                                                   | 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如 『:! ls /home』即可在 vi 当中察看 /home 底下以 ls 输出的档案信息！ |
| vim 环境的变更                                               |                                                              |
| :set nu                                                      | 显示行号，设定之后，会在每一行的前缀显示该行的行号           |
| :set nonu                                                    | 与 set nu 相反，为取消行号！                                 |



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

### cp | mv | scp | mkdir

```
# 复制
cp xxx.log

# 复制并强制覆盖同名文件
cp -f xxx.log

# 复制文件夹
cp -r xxx(源文件夹) yyy(目标文件夹)

# 将源文件名 source_file 改为目标文件名 dest_file
mv source_file(文件) dest_file(文件)

# 将文件 source_file 移动到目标目录 dest_directory 中
mv source_file(文件) dest_directory(目录)

# 目录名 dest_directory 已存在，将 source_directory 移动到目录名 dest_directory 中,目录名 dest_directory 不存在则 source_directory 改名为目录名 dest_directory
mv source_directory(目录) dest_directory(目录)

# 远程复制
scp -P ssh端口 username@10.10.10.101:/home/username/xxx /home/xxx

# 级联创建目录
mkdir -p /xxx/yyy/zzz

# 批量创建文件夹, 会在test,main下都创建java, resources文件夹
mkdir -p src/{test,main}/{java,resources}
```

### diff  | tail | cat  | sed(去除特殊字符)

```
# 比较两个文件
diff -u 1.txt 2.txt

# 日志输出的字节数,可以用作性能测试
# 如果做性能测试, 可以每执行一次, 往日志里面输出 “.” , 这样日志中的字节数就是实际的性能测试运行的次数, 还可以看见实时速率.
tail -f xxx.log | pv -bt

# 查看特殊字符
cat -v xxx.sh

# 去除特殊字符
sed -i 's/^M//g’ env.sh  去除文件的特殊字符, 比如^M:  需要这样输入: ctrl+v+enter
```

### 处理因系统原因引起的文件中特殊字符的问题

```
# 可以转换为该系统下的文件格式
cat file.sh > file.sh_bak

# 先将file.sh中文件内容复制下来然后运行, 然后粘贴内容, 最后ctrl + d 保存退出
cat > file1.sh

# 在vim中通过如下设置文件编码和文件格式
:set fileencodings=utf-8 ，然后 w （存盘）一下即可转化为 utf8 格式，
:set fileformat=unix

# 在mac下使用dos2unix进行文件格式化
find . -name "*.sh" | xargs dos2unix
```

### tee 重定向的同时输出到屏幕

```
awk ‘{print $0}’ xxx.log | tee test.log
```



## 检索相关

### grep 

```
# 在当前目录中，查找后缀有file 字样的文件中包含 test 字符串的文件，并打印出该字符串的行
grep test *file(后缀)
grep test file*(前缀)

# 以递归的方式查找符合条件的文件，查找指定目录/etc/acpi 及其子目录（如果存在子目录的话）下所有文件中包含字符串"update"的文件，并打印出该字符串所在行的内容
grep -r update /etc/acpi 

# 反向匹配, 查找不包含xxx的内容
grep -v xxx xxx.log
# 查找文件名中包含 test 的文件中不包含test 的行
grep -v test *test*

# 排除所有空行
grep -v '^/pre>

# 返回结果 2,则说明第二行是空行
grep -n “^$” 111.txt    

# 查询以abc开头的行
grep -n “^abc” 111.txt 

# 同时列出该词语出现在文章的第几行
grep 'xxx' -n xxx.log

# 计算一下该字串出现的次数
grep 'xxx' -c xxx.log

# 不计较大小写的不同
grep 'xxx' -i xxx.log

# 查找包含“下载申请”字符串的行，除了显示符合样式的那一行之外，并显示该行之前后10行的内容
grep -C 10 "下载申请" info.log 
```

### awk

```
# 以':' 为分隔符,如果第五域有user则输出该行
awk -F ':' '{if ($5 ~ /user/) print $0}' /etc/passwd 

# 统计单个文件中某个字符（串）(中文无效)出现的次数
awk -v RS='character' 'END {print --NR}' xxx.txt
```

### find 

```
# 在目录下找后缀是.mysql的文件
find /home/eagleye -name '*.mysql' -print

# 会从 /usr 目录开始往下找，找最近3天之内存取过的文件。
find /usr -atime 3 –print

# 会从 /usr 目录开始往下找，找最近5天之内修改过的文件。
find /usr -ctime 5 –print

# 会从 /doc 目录开始往下找，找jacky 的、文件名开头是 j的文件。  
find /doc -user jacky -name 'j*' –print

# 会从 /doc 目录开始往下找，找寻文件名是 ja 开头或者 ma开头的文件。
find /doc \( -name 'ja*' -o- -name 'ma*' \) –print

# 会从 /doc 目录开始往下找，找到凡是文件名结尾为 bak的文件，把它删除掉。-exec 选项是执行的意思，rm 是删除命令，{ } 表示文件名，“\;”是规定的命令结尾。 
find /doc -name '*bak' -exec rm {} \;
```





## 网络相关

### 查看什么进程使用了该端口

```
lsof -i:port
```

### 获取本机ip地址

```
/sbin/ifconfig -a|grep inet|grep -v 127.0.0.1|grep -v inet6|awk '{print $2}'|tr -d "addr:"
```

### iptables

```
# 备注: 参数-I是表示Insert（添加），-D表示Delete（删除）。后面跟的是规则，INPUT表示入站，***.***.***.***表示要封停的IP，DROP表示放弃连接。

# 查看iptables状态
service iptables status

# 要封停一个ip
iptables -I INPUT -s ***.***.***.*** -j DROP

# 要解封一个IP，使用下面这条命令：
iptables -D INPUT -s ***.***.***.*** -j DROP

# 开启9090端口的访问
/sbin/iptables -I INPUT -p tcp --dport 9090 -j ACCEPT 

# 防火墙开启、关闭、重启
/etc/init.d/iptables status
/etc/init.d/iptables start
/etc/init.d/iptables stop
/etc/init.d/iptables restart
```

### nc命令 | tcp调试利器

```
# 给某一个endpoint发送TCP请求,就将data的内容发送到对端
nc 192.168.0.11 8000 < data.txt

# nc可以当做服务器，监听某个端口号,把某一次请求的内容存储到received_data里
nc -l 8000 > received_data

# 上边只监听一次，如果多次可以加上-k参数
nc -lk 8000
```

### tcpdump

```
# dump出本机12301端口的tcp包
tcpdump -i em1 tcp port 12301 -s 1500 -w abc.pcap
```

### traceroute 跟踪网络路由路径

```
# traceroute默认使用udp方式, 如果是-I则改成icmp方式
traceroute -I www.163.com

# 从ttl第3跳跟踪
traceroute -M 3 www.163.com  

# 加上端口跟踪
traceroute -p 8080 192.168.10.11
```

### ss 命令

```
# 显示本地打开的所有端口
ss -l 

# 显示每个进程具体打开的socket
ss -pl 

# 显示所有tcp socket
ss -t -a 

# 显示所有的UDP Socekt
ss -u -a 

# 显示所有已建立的SMTP连接
ss -o state established '( dport = :smtp or sport = :smtp )'  

# 显示所有已建立的HTTP连接 
ss -o state established '( dport = :http or sport = :http )'  

# 找出所有连接X服务器的进程
ss -x src /tmp/.X11-unix/*  

# 列出当前socket统计信息
ss -s 

# 解释：netstat是遍历/proc下面每个PID目录，ss直接读/proc/net下面的统计信息。所以ss执行的时候消耗资源以及消耗的时间都比netstat少很多
```

### netstat 命令

```
# 输出每个ip的连接数，以及总的各个状态的连接数
netstat -n | awk '/^tcp/ {n=split($(NF-1),array,":");if(n<=2)++S[array[(1)]];else++S[array[(4)]];++s[$NF];++N} END {for(a in S){printf("%-20s %s\n", a, S[a]);++I}printf("%-20s %s\n","TOTAL_IP",I);for(a in s) printf("%-20s %s\n",a, s[a]);printf("%-20s %s\n","TOTAL_LINK",N);}'

# 统计所有连接状态, 
# CLOSED：无连接是活动的或正在进行
# LISTEN：服务器在等待进入呼叫
# SYN_RECV：一个连接请求已经到达，等待确认
# SYN_SENT：应用已经开始，打开一个连接
# ESTABLISHED：正常数据传输状态
# FIN_WAIT1：应用说它已经完成
# FIN_WAIT2：另一边已同意释放
# ITMED_WAIT：等待所有分组死掉
# CLOSING：两边同时尝试关闭
# TIME_WAIT：主动关闭连接一端还没有等到另一端反馈期间的状态
# LAST_ACK：等待所有分组死掉
netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}'

# 查找较多time_wait连接
netstat -n|grep TIME_WAIT|awk '{print $5}'|sort|uniq -c|sort -rn|head -n20
```







## 监控linux性能命令

### top 命令

```
# 按大写的 F 或 O 键，然后按 a-z 可以将进程按照相应的列进行排序, 然后回车。而大写的 R 键可以将当前的排序倒转
top
```

| 列名    | 含义                                                         |
| :------ | :----------------------------------------------------------- |
| PID     | 进程id                                                       |
| PPID    | 父进程id                                                     |
| RUSER   | Real user name                                               |
| UID     | 进程所有者的用户id                                           |
| USER    | 进程所有者的用户名                                           |
| GROUP   | 进程所有者的组名                                             |
| TTY     | 启动进程的终端名。不是从终端启动的进程则显示为 ?             |
| PR      | 优先级                                                       |
| NI      | nice值。负值表示高优先级，正值表示低优先级                   |
| P       | 最后使用的CPU，仅在多CPU环境下有意义                         |
| %CPU    | 上次更新到现在的CPU时间占用百分比                            |
| TIME    | 进程使用的CPU时间总计，单位秒                                |
| TIME+   | 进程使用的CPU时间总计，单位1/100秒                           |
| %MEM    | 进程使用的物理内存百分比                                     |
| VIRT    | 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES                |
| SWAP    | 进程使用的虚拟内存中，被换出的大小，单位kb。                 |
| RES     | 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA    |
| CODE    | 可执行代码占用的物理内存大小，单位kb                         |
| DATA    | 可执行代码以外的部分(数据段+栈)占用的物理内存大小，单位kb    |
| SHR     | 共享内存大小，单位kb                                         |
| nFLT    | 页面错误次数                                                 |
| nDRT    | 最后一次写入到现在，被修改过的页面数。                       |
| S       | 进程状态。D=不可中断的睡眠状态,R=运行,S=睡眠,T=跟踪/停止,Z=僵尸进程 |
| COMMAND | 命令名/命令行                                                |
| WCHAN   | 若该进程在睡眠，则显示睡眠中的系统函数名                     |
| Flags   | 任务标志，参考 sched.h                                       |



### dmesg 查看系统日志

```
dmesg
```

### iostat 磁盘IO情况监控

```
iostat -xz 1

# r/s, w/s, rkB/s, wkB/s：分别表示每秒读写次数和每秒读写数据量（千字节）。读写量过大，可能会引起性能问题。
# await：IO操作的平均等待时间，单位是毫秒。这是应用程序在和磁盘交互时，需要消耗的时间，包括IO等待和实际操作的耗时。如果这个数值过大，可能是硬件设备遇到了瓶颈或者出现故障。
# avgqu-sz：向设备发出的请求平均数量。如果这个数值大于1，可能是硬件设备已经饱和（部分前端硬件设备支持并行写入）。
# %util：设备利用率。这个数值表示设备的繁忙程度，经验值是如果超过60，可能会影响IO性能（可以参照IO操作平均等待时间）。如果到达100%，说明硬件设备已经饱和。
# 如果显示的是逻辑设备的数据，那么设备利用率不代表后端实际的硬件设备已经饱和。值得注意的是，即使IO性能不理想，也不一定意味这应用程序性能会不好，可以利用诸如预读取、写缓存等策略提升应用性能。
```

### free 内存使用情况

```
free -m

eg:

     total       used       free     shared    buffers     cached
Mem:          1002        769        232          0         62        421
-/+ buffers/cache:          286        715
Swap:          1153          0       1153

第一部分Mem行:
total 内存总数: 1002M
used 已经使用的内存数: 769M
free 空闲的内存数: 232M
shared 当前已经废弃不用,总是0
buffers Buffer 缓存内存数: 62M
cached Page 缓存内存数:421M

关系：total(1002M) = used(769M) + free(232M)

第二部分(-/+ buffers/cache):
(-buffers/cache) used内存数：286M (指的第一部分Mem行中的used – buffers – cached)
(+buffers/cache) free内存数: 715M (指的第一部分Mem行中的free + buffers + cached)

可见-buffers/cache反映的是被程序实实在在吃掉的内存,而+buffers/cache反映的是可以挪用的内存总数.

第三部分是指交换分区
```

### sar 查看网络吞吐状态

```
# sar命令在这里可以查看网络设备的吞吐率。在排查性能问题时，可以通过网络设备的吞吐量，判断网络设备是否已经饱和
sar -n DEV 1

# sar命令在这里用于查看TCP连接状态，其中包括：
# active/s：每秒本地发起的TCP连接数，既通过connect调用创建的TCP连接；
# passive/s：每秒远程发起的TCP连接数，即通过accept调用创建的TCP连接；
# retrans/s：每秒TCP重传数量；
# TCP连接数可以用来判断性能问题是否由于建立了过多的连接，进一步可以判断是主动发起的连接，还是被动接受的连接。TCP重传可能是因为网络环境恶劣，或者服务器压力过大导致丢包
sar -n TCP,ETCP 1
```

### vmstat 给定时间监控CPU使用率, 内存使用, 虚拟内存交互, IO读写

```
# 2表示每2秒采集一次状态信息, 1表示只采集一次(忽略既是一直采集)
vmstat 2 1

eg:
r b swpd free buff cache si so bi bo in cs us sy id wa
1 0 0 3499840 315836 3819660 0 0 0 1 2 0 0 0 100 0
0 0 0 3499584 315836 3819660 0 0 0 0 88 158 0 0 100 0
0 0 0 3499708 315836 3819660 0 0 0 2 86 162 0 0 100 0
0 0 0 3499708 315836 3819660 0 0 0 10 81 151 0 0 100 0
1 0 0 3499732 315836 3819660 0 0 0 2 83 154 0 0 100 0

# 解释：
r 表示运行队列(就是说多少个进程真的分配到CPU)，我测试的服务器目前CPU比较空闲，没什么程序在跑，当这个值超过了CPU数目，就会出现CPU瓶颈了。这个也和top的负载有关系，一般负载超过了3就比较高，超过了5就高，超过了10就不正常了，服务器的状态很危险。top的负载类似每秒的运行队列。如果运行队列过大，表示你的CPU很繁忙，一般会造成CPU使用率很高。
b 表示阻塞的进程,这个不多说，进程阻塞，大家懂的。
swpd 虚拟内存已使用的大小，如果大于0，表示你的机器物理内存不足了，如果不是程序内存泄露的原因，那么你该升级内存了或者把耗内存的任务迁移到其他机器。
free 空闲的物理内存的大小，我的机器内存总共8G，剩余3415M。
buff Linux/Unix系统是用来存储，目录里面有什么内容，权限等的缓存，我本机大概占用300多M
cache cache直接用来记忆我们打开的文件,给文件做缓冲，我本机大概占用300多M(这里是Linux/Unix的聪明之处，把空闲的物理内存的一部分拿来做文件和目录的缓存，是为了提高 程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。)
si 每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了，要查找耗内存进程解决掉。我的机器内存充裕，一切正常。
so 每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。
bi 块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte，我本机上没什么IO操作，所以一直是0，但是我曾在处理拷贝大量数据(2-3T)的机器上看过可以达到140000/s，磁盘写入速度差不多140M每秒
bo 块设备每秒发送的块数量，例如我们读取文件，bo就要大于0。bi和bo一般都要接近0，不然就是IO过于频繁，需要调整。
in 每秒CPU的中断次数，包括时间中断
cs 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目,例如在apache和nginx这种web服务器中，我们一般做性能测试时会进行几千并发甚至几万并发的测试，选择web服务器的进程可以由进程或者线程的峰值一直下调，压测，直到cs到一个比较小的值，这个进程和线程数就是比较合适的值了。系统调用也是，每次调用系统函数，我们的代码就会进入内核空间，导致上下文切换，这个是很耗资源，也要尽量避免频繁调用系统函数。上下文切换次数过多表示你的CPU大部分浪费在上下文切换，导致CPU干正经事的时间少了，CPU没有充分利用，是不可取的。
us 用户CPU时间，我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80(机器在做压力测试，性能表现不佳)。
sy 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。
id 空闲 CPU时间，一般来说，id + us + sy = 100,一般我认为id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。
wt 等待IO CPU时间。
```

```html

```

