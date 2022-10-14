# 实现 SSL 证书到期监控

```
[root@Nginx ~]# cat ssl-monitor.sh
#!/bin/bash
# 定义网站域名和端口号信息
WebName="www.baidu.com"
Port="443"

# 通过 Openssl 工具获取到当前证书的到期时间
Cert_END_Time=$(echo | openssl s_client -servername ${WebName} -connect ${WebName}:${Port} 2> /dev/null | openssl x509 -noout -dates | grep 'After' | awk -F '=' '{print $2}' | awk '{print $1,$2,$4}')

# 将证书的到期时间转化成时间戳
Cert_NED_TimeStamp=$(date +%s -d "$Cert_END_Time")

# 定义当前时间的时间戳
Create_TimeStamp=$(date +%s)

# 通过计算获取到证书的剩余天数
Rest_Time=$(expr $(expr $Cert_NED_TimeStamp - $Create_TimeStamp) / 86400)

# 配置告警提示信息
echo "$WebName  网站的 SSL 证书还有 $Rest_Time 天后到期" > ssl-monitor.txt

# 判断出证书时间小于 30 天的
if [ $Rest_Time -lt 30 ];then

# 定义企业微信机器人的 API 接口
WebHook='https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=2743320b-0a2c-404b-87bc-25fedf1ff67a'

# 通过 Curl 命令来发送 Post 请求
curl "${WebHook}" -H 'Content-Type: application/json' -d '
{
    "msgtype": "text",
    "text": {
        "content": "'"$(cat ssl-monitor.txt)"'"
    }
}' &> /dev/null
fi
```

## 手动验证

```
[root@Nginx ~]# bash ssl-monitor.sh 
```

## 配置到 CronJob 中

```
[root@Nginx ~]# crontab -e
* 23 * * * /bin/bash /root/ssl-monitor.sh
```