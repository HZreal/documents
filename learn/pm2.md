# PM2

参考：https://mp.weixin.qq.com/s/S8Gw2XzuflnN2QMSVXhLhg



## 场景

-  node 应用跑的时候突然抛了个错，崩溃了，是不是需要重新跑起来？这时候是不是就需要另一个进程来自动做重启这件事情？
- node 应用的日志默认输出在控制台，如果想输出到不同的日志文件，是不是可以让另一个进程获取 node 应用的输出，然后写文件来实现？
- node 是单线程的，而机器是多个 cpu 的，为了充分利用 cpu 的能力，我们会用多个进程来跑 node 应用，来提高性能。这种通用逻辑是不是也可以放到一个单独进程里来实现？
- node 运行时的 cpu、内存等资源的占用，是不是需要监控？这时候是不是可以让另一个进程来做？



线上的 node 应用不只是跑起来就行了，还要做自动重启、日志、多进程、监控这些事情。



## 简介

pm2 是 process manager，进程管理，它是第二个大版本，和前一个版本差异很大，所以叫 pm2.

pm2 的主要功能就是**进程管理、日志管理、负载均衡、性能监控**这些。



## 安装

```shell
npm install -g pm2
```



## 基本命令

```
pm2 start main.js   pm2 start <pid>
pm2 stop main.js

pm2 logs
pm2 log <pid>
```

#### PM2 start 参数

-i num 就是启动 num 个进程做负载均衡

pm2 start app.js -i max   根据CPU核数启动进程的个数



## 动态调整进程数

pm2 scale main 3    把集群调整为 3 个进程

pm2 scale main +3  把集群增加3 个进程



## 性能监控

可以看到不同进程的 cpu 和内存占用情况

pm2 monit



## PM2配置文件



## PM2管理前端

配置文件：

```json
{
    "apps": [
        {
            "script": "serve",
            "name": "chilinnew",
            "namespace": "szkj",
            "env": {
                "PM2_SERVE_PATH": "./dist",
                "PM2_SERVE_PORT": 8080
            }
        }
    ]
}

```



PM2管理后端

配置文件：

```
# midway 框架
{
    "apps": [
        {
            "name": "sjcl-master",
            "namespace": "szkj",
            "script": "./bootstrap.js",
            "error_file": "~/logs/sjcl-master/err.log",
            "out_file": "~/logs/sjcl-master/out.log",
            "merge_logs": true,
            "env": {
                "NODE_ENV": "production",
                "MIDWAY_SERVER_ENV": "production"
            }
        }
    ]
}

# nestJS

```

