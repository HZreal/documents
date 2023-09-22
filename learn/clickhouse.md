# clickhouse

## 安装搭建

## server配置

#### 路径

/etc/clickhouse-server

用户配置

- user.xml
- user.d/

server配置

- config.xml
- config.d/

#### 设置用户名密码

vim users.xml，找到 users --> default --> 标签下的password修改成password_sha256_hex，并把密文填进去

```xml
<password_sha256_hex>密码密文</password_sha256_hex>
```

#### 设置远程可访问

vim config.xml 找到 listen_host 标签，修改为以下:

```xml
<listen_host>0.0.0.0</listen_host>
```

## 启动



## 访问

```bash
clickhouse-client -h ip地址 -d default -m -u default --password 密码明文
注意参数--password不能简写为-p
```

## 基本操作CURD

