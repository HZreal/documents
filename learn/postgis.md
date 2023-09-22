# postgis

## 安装扩展

先查找最新postgis插件版本号

```bash
sudo apt-cache search postgis
```

找到当前已安装postgreSQL版本对应的postgis版本

ps:查看postgreSQL版本  select version()

```bash
sudo apt-get install postgresql-14-postgis-3
```

赋予某个库空间数据库的能力

```sql
CREATE EXTENSION postgis;
```

postgis_topology支持拓扑

```sql
CREATE EXTENSION postgis_topology;
```

pgrouting 提供了对路网的分析支持，包括双向Dijkstra最短路径等10多种功能，是postgis的插件，需要额外安装