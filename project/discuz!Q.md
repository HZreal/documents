# discuz!Q

## 简介

快速搭建个人社区

doc： https://discuz.com/

api： https://developer.discuz.chat/#/api/get:_api_v3_check.user.get.redpacket

## 安装

linux下，推荐docker安装   https://discuz.com/docs/Linux%20%E4%B8%BB%E6%9C%BA.html

#### 安装启动容器命令

```bash
docker run -d
--restart=always
-p 20080:80
-p 20443:443
-v ~/discuzQ/data/discuz:/var/lib/discuz
-v ~/discuzQ/data/mysql-data:/var/lib/mysqldb
-v ~/discuzQ/data/certs:/etc/nginx/certs
ccr.ccs.tencentyun.com/discuzq/dzq:latest

```

#### 初始化安装 Discuz! Q

访问 http://ip:port/install 并配置网站相关信息。

- 配置mysql ip 用户名密码
- 配置后台用户名密码admin admin123456

## 使用

安装后，访问即可，一个社区就出现啦！可以注册咯

http://ip:20080/

https://ip:20443/

后台站点访问，用户名admin密码admin123456

http://ip:20080/admin

https://ip:20443/admin/