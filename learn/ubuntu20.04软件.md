# ubuntu20.04安装部分软件



## 安装docker

#### 1、卸载可能存在的或者为安装成功的Docker版本

```
sudo apt-get remove docker docker-engine docker-ce docker.io
```

#### 2、添加阿里云的GPG密钥

```
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

#### 3、使用以下命令设置存储库

```
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

#### 4、安装最新版本的Docker

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

#### 5、验证Docker是否安装成功

```
-- 查看docker 版本
docker version
```



## 二、安装postgresql

#### 1、Add Postgre SQL 13 repository

```
sudo apt update
sudo apt install curl gpg gnupg2 software-properties-common apt-transport-https lsb-release ca-certificates
```

#### add the APT repository，importing GPG key

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

#### add repository contents 

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```

#### 2、Install PostgreSQL 13

```
sudo apt update
```

```
sudo apt install postgresql-13 postgresql-client-13
```

```
systemctl status postgresql@13-main.service
```

#### 3、Configure remote Connection

```
sudo vi /etc/postgresql/13/main/postgresql.conf 

# Listen on all interfaces
listen_addresses = '*'

# Listen on specified private IP address
listen_addresses = '192.168.10.11'
```

```
sudo vi /etc/postgresql/13/main/pg_hba.conf
```

```
# Accept from anywhere
host all all 0.0.0.0/0 md5

# postgres用户通过密码登录
host all postgres  trust

# Accept from trusted subnet
host all all 10.10.10.0/24 md5
```

```
sudo systemctl restart postgresql
```

#### 4、修改默认postgres用户的密码

```
# 进入psql
sudo -u postgres psql

修改密码
\password postgres  并输入密码dizai123456
```



## docker安装rabbitMQ:3.8.9

#### 1、创建需要映射的目录

```
mkdir -p /home/sjcl/docker_file/rabbitmq3.8.9/{data,conf,log}

# 记得文件授权！
chmod -R 777 /home/sjcl/docker_file/rabbitmq3.8.9/{data,conf,log}
```

#### 2、下载并启动

```
docker run -d -it
--privileged=true 
--name rabbitmq3.8.9 
-p 5672:5672 
-p 15672:15672 
-v /home/sjcl/docker_file/rabbitmq3.8.9/data:/var/lib/rabbiitmq 
-v /home/sjcl/docker_file/rabbitmq3.8.9/conf:/etc/rabbitmq 
-v /home/sjcl/docker_file/rabbitmq3.8.9/log:/var/log/rabbitmq 
-e RABBITMQ_DEFAULT_USER=dizai 
-e RABBITMQ_DEFAULT_PASS=123456 
rabbitmq:3.8.9-management
```

#### 3、开启rabbitmq_management插件

```
# 进入容器中
docker exec -it <容器id> /bin/bash

# 开启插件，则UI可访问
rabbitmq-plugins enable rabbitmq_management
```



## docker安装geoserver:2.18.0

#### 1、创建需要映射的目录

```
mkdir /home/sjcl/docker_file/geoserver2.18.0/data

chmod 777 /home/sjcl/docker_file/geoserver2.18.0/data
```



#### 2、下载并启动

```
docker run -d 
--name geoserver2.18.0 
-p 48080:8080 
-v /home/sjcl/docker_file/geoserver2.18.0/data:/opt/geoserver/data_dir 
-e GEOSERVER_DATA_DIR=/opt/geoserver/data_dir 
-e GEOSERVER_ADMIN_PASSWORD=dizai123456 
kartoza/geoserver:2.18.0
```



## apt

配置目录：/etc/apt

软件源设置文件：/etc/apt/sources.list（设置阿里源、清华源等）



## 安装mysql

链接：https://developer.aliyun.com/article/758177

```
sudo apt update
sudo apt install mysql-server
sudo systemctl status mysql

# 运行初始化安全脚本
sudo mysql_secure_installation
```



## 安装nginx

```
sudo apt update
apt install nginx
```



## 安装redis

```
sudo apt update
sudo apt install redis-server
```

配置远程访问，修改/etc/redis/redis.conf

注释bind 127.0.0.1

设置 protect-mode=no（不推荐）或者添加密码



## 安装samba

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install samba samba-common
```

配置使用

```
# 创建要共享的目录并赋予权限
mkdir ~/shared
sudo chmod 777 ~/shared

# 添加samba用户(必须是系统已有用户)，并输入密码
sudo smbpasswd -a <用户名>

# 修改samba的配置文件，添加共享目录的设置
sudo vi /etc/samba/smb.conf
# 增添以下内容：
[shared]
comment = share folder
browseable = yes
path = ~/shared
valid users = <your registerd name>
public = yes
available = yes
writable = yes

# 重启smbd服务
sudo systemctl restart smbd.service
```



windows访问：win+R输入：\\\ip     然后回车输入用户名密码


