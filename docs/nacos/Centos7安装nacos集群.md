<div data-cover="" class="title">Centos7安装nacos集群</div>

### 一、前期环境准备

> nacos需要jdk,建议安装jdk8

### 二、 下载安装包并上传至服务器

* 节点规划

|主机ip|端口|安装目录|
|---|---|---|
| 10.170.202.4 | 8848 | /data/nacos |
| 10.170.202.5 | 8848 | /data/nacos |
| 10.170.202.6 | 8848 | /data/nacos |

* 上传nacos-server-1.4.2.tar.gz至服务器/data/nacos下
* 解压

```
[ifss@zhxaf-xxsf11d002-cs04x nacos] tar -zxvf nacos-server-1.4.2.tar.gz
# 查看
[ifss@zhxaf-xxsf11d002-cs04x nacos]$ ll
total 0
drwxr-xr-x 8 ifss ifss 110 Dec  3 16:14 nacos-1.4.2
​
```

nacos下载地址

### 三、集群配置

1. 数据库配置

```
vim cd /data/nacos/nacos-1.4.2/conf/application.properties
```

```
# 修改下面配置 网卡地址,msyql数据库
​
### Specify local server's IP:
nacos.inetutils.ip-address=10.170.202.4
​
#*************** Config Module Related Configurations ***************#
### If use MySQL as datasource:
spring.datasource.platform=mysql
​
### Count of DB:
db.num=1
​
### Connect URL of DB:
db.url.0=jdbc:mysql://10.170.202.25:3316/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=Asia/Shanghai
db.user.0=x
db.password.0=x
​
```

2. 集群配置

```
cd /data/nacos/nacos-1.4.2/conf
​
cp cluster.conf.example cluster.conf
​
vim cluster.conf
```

```
# cluster.conf文件内容
​
10.170.202.4:8848
10.170.202.5:8848
10.170.202.6:8848
​
```

### 四、启动nacos

分别登录3台主机执行启动命令

```
cd /data/nacos/nacos-1.4.2/bin
# 启动
sh start.sh
​
# 查看启动日志
tail -f /data/nacos/nacos-1.4.2/logs/start.out
```

### 五、防火墙设置

```
# 查询防火墙状态
sudo systemctl status firewalld
​
# 如果开启状态,开发8848端口
firewall-cmd --zone=public --permanent --add-port=8848/tcp
​
# 重新加载防火墙
firewall-cmd --reload
```

### 六、参考连接

https://nacos.io/zh-cn/docs/quick-start.html

中间件