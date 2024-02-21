# postgreSQL

## 安装搭建

https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/

指定版本安装  

```bash
sudo apt-get install postgresql-13
```

## 配置

- #### 路径

  - ubuntu下：
  - **/etc/postgresql/<version>/main**

  - centOS下：
    - **/var/lib/pgsql/13/data/postgresql.conf**

- #### postgres.conf 

  pg数据库配置文件

  包含设置data/hba/indet等目录，IP/port，最大连接数，认证，ssl，内存，磁盘等等

* #### pg_hba.conf 

  客户端认证配置文件即host-based authentication：基于主机的认证

  多种连接pg数据库的方式

* #### pg_ident.conf 

  用户名称映射，可添加映射用户

- #### 用户名密码

- #### 设置可远程访问

## 启动关闭

```bash
systemctl start 
systemctl stop

```



## 访问

切换用户

```bash
sudo -i -u postgres
```

登入

```bash
psql -h 192.168.1.7 -p 5432 -U postgres -d testdb -W 密码
```

## 基本操作

### 数据库操作

1、列举数据库：\l
2、选择数据库：\c 数据库名
3、查看该某个库中的所有表：\dt
4、切换数据库：\c db_name
5、查看某个库中的某个表结构：\d 表名
6、查看某个库中某个表的记录：select * from apps limit 1;
7、显示字符集：\encoding
8、退出psgl：\q

### 表、库操作

### 查询

#### 基本CURD

- insert
- update  set
- delete from

#### 条件查询

- and 
- or 
- not
- in 
- not in
- between and
- like
- group  having
- order by

#### 聚合

- 
- 

#### 关联查询

- inner join / join
- left join
- right join
- full outer join 全外连接 
- cross join 交叉连接 （笛卡尔积）

#### Union查询

- union
  - 用于组合两个或多个SELECT语句的结果，而不返回任何重复的行
  - 要使用UNION，每个SELECT必须具有相同的列数，相同数量的列表达式，相同的数据类型，并且具有相同的顺序，但不一定要相同。

  ```sql
  SELECT column1 [, column2 ]
  FROM table1 [, table2 ]
  [WHERE condition]
  
  UNION
  
  SELECT column1 [, column2 ]
  FROM table1 [, table2 ]
  [WHERE condition]
  ```

- UNION ALL

  - 运算符用于组合两个`SELECT`语句(包括重复行)的结果。 适用于UNION的相同规则也适用于`UNION ALL`运算符

  ```sql
  SELECT column1 [, column2 ]
  FROM table1 [, table2 ]
  [WHERE condition]
  
  UNION ALL
  
  SELECT column1 [, column2 ]
  FROM table1 [, table2 ]
  [WHERE condition]
  ```

#### 窗口函数

https://zhuanlan.zhihu.com/p/409561328

表达式：func(field) over(partition by field, order by field, rows  between 3 preceding and current row)

func有多种：

1. 排序类函数
   - ROW_NUMBER(field)
     - 每一行记录生成一个序号，依次排序且不会重复。 123456...
   - RANK(field)
     - 跳跃排序，生成的序号有可能不连续。1114557..
   - DENSE_RANK(field)
     - 在生成序号时是连续的。1112234444...
   - ntile(n)
     - 用于将分组数据按照顺序切分成n片，返回当前切片值.
2. 聚合类函数
   - min()、max()、avg()、sum()、count()
     - 如一年中每月的累计和，每四个月的移动平均，每四个月中的最大/小值
3. 偏移分析函数
   - lead(field, offset, defval)            ----- lead领先，表示当前记录之前（记录更靠后）
     - field：字段名， offset：偏移量，不指定默认是1。 defval：null时的默认值。
     - 当向上偏移了offset行已经超出了表的范围时，lag()函数将defval这个参数值作为函数的返回值，若没有指定默认值，则返回NULL。
   - lag(field, offset, defval)               ----- lag落后，表示当前记录之后（记录更靠前）

- percent_rank()、cume_dist()
- first_val(expr) 、 last_val(expr)、nth_val(expr, n)

#### 有用特殊函数

1. coalesce(expression_1, expression_2, ...,expression_n)

   1. 遇到非null值即停止并返回该值。如果所有的表达式都是空值，最终将返回一个空值。

   2. ```sql
      select coalesce(real_name ,nick_name,“xiaoai”) from table_name
      
      当real_name不为null，那么无论nick_name是否为null，都将返回real_name的具体值；当real_name为null，而nick_name,不为null的时候，返回nick_name具体值。只有当real_name和nick_name都为null的时候，才返回xiaoai。
      ```

2. --
3.  --



### pg_dump工具

```
# 数据库备份
pg_dump -h localhost -U postgres <db_name> > ./dump_out.sql
```

### psql工具

```
导入sql数据源
psql "host=localhost port=5432 user=postgres password=<password> dbname=<db_name>" -f ./dump_out.sql
```



## 日期和时间函数

doc: https://www.postgresql.org/docs/current/functions-datetime.html

所有重要的日期和时间相关函数如下列表所示：

| 函数                  | 描述                                    |
| --------------------- | --------------------------------------- |
| `AGE()`               | 减去参数                                |
| `CURRENT DATE/TIME()` | 它指定当前日期和时间。                  |
| `DATE_PART()`         | 获取子字段(相当于提取)                  |
| `EXTRACT()`           | 获得子字段                              |
| `ISFINITE()`          | 测试有限的日期，时间和间隔(非+/-无穷大) |
| `JUSTIFY`             | 调整间隔                                |

##### 重要函数

1. **date_part**(type, timestamp)
   1. type的类型如下
      1. century
      2. decade
      3. year
      4. month
      5. day
      6. hour
      7. minute
      8. second
      9. microseconds
      10. milliseconds
      11. dow
      12. doy
      13. epoch
      14. isodow
      15. isoyear
      16. timezone
      17. timezone_hour
      18. timezone_minute
   2. 返回该类型下提取的值

2. **extract**(type from timestamp)

   1. type类型与上述date_part的类型一样

3. **date_trunc**

   1. 用于设置时间基点

   2. ```sql
      select date_trunc('month',current_date);   -- 计算当月第一天
      ```

   3. ```sql
      -- 计算上月最后一天
      select date_trunc('month',current_date) - interval'1 day'  --基于当月第一天,倒退1天
      ```

   4. ```sql
      -- 计算上月第一天
      select date_trunc('month',current_date) + interval'1 month - 1 day'  -- 基于当月第一天,前进一个月,再倒退1天
      ```

      

4. **to_char**(timestamp, format_string)

   1. 时间格式化字符串如下

      | 模式  | 描述                                         |
      | :---- | :------------------------------------------- |
      | HH    | 一天的小时数(01-12)                          |
      | HH12  | 一天的小时数(01-12)                          |
      | HH24  | 一天的小时数(00-23)                          |
      | MI    | 分钟(00-59)                                  |
      | SS    | 秒(00-59)                                    |
      | MS    | 毫秒(000-999)                                |
      | US    | 微秒(000000-999999)                          |
      | AM    | 正午标识(大写)                               |
      | Y,YYY | 带逗号的年(4和更多位)                        |
      | YYYY  | 年(4和更多位)                                |
      | YYY   | 年的后三位                                   |
      | YY    | 年的后两位                                   |
      | Y     | 年的最后一位                                 |
      | MONTH | 全长大写月份名(空白填充为9字符)              |
      | Month | 全长混合大小写月份名(空白填充为9字符)        |
      | month | 全长小写月份名(空白填充为9字符)              |
      | MON   | 大写缩写月份名(3字符)                        |
      | Mon   | 缩写混合大小写月份名(3字符)                  |
      | mon   | 小写缩写月份名(3字符)                        |
      | MM    | 月份号(01-12)                                |
      | DAY   | 全长大写日期名(空白填充为9字符)              |
      | Day   | 全长混合大小写日期名(空白填充为9字符)        |
      | day   | 全长小写日期名(空白填充为9字符)              |
      | DY    | 缩写大写日期名(3字符)                        |
      | Dy    | 缩写混合大小写日期名(3字符)                  |
      | dy    | 缩写小写日期名(3字符)                        |
      | DDD   | 一年里的日子(001-366)                        |
      | DD    | 一个月里的日子(01-31)                        |
      | D     | 一周里的日子(1-7；周日是1)                   |
      | W     | 一个月里的周数(1-5)(第一周从该月第一天开始)  |
      | WW    | 一年里的周数(1-53)(第一周从该年的第一天开始) |

5. **age**(timestamp，timestamp)＆AGE(timestamp)：

| 函数                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `age(timestamp, timestamp)` | 当使用第二个参数的时间戳形式调用时，`age()`减去参数，产生使用年数和月份的类型为“`interval`”的“符号”结果。 |
| `age(timestamp)`            | 当仅使用时间戳作为参数调用时，`age()`从`current_date(午夜)`减去。 |

当前DATE/TIME()

以下是返回与当前日期和时间相关的值的函数的列表。

| 函数                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| CURRENT_DATE                 | 提供当前日期                                                 |
| CURRENT_TIME                 | 提供带时区的值                                               |
| CURRENT_TIMESTAMP            | 提供带时区的值                                               |
| CURRENT_TIME(precision)      | 可以选择使用`precision`参数，这将使结果在四分之一秒的范围内四舍五入到数位数。 |
| CURRENT_TIMESTAMP(precision) | 可以选择使用精度参数，这将使结果在四分之一秒的范围内四舍五入到数位数。 |
| LOCALTIME                    | 提供没有时区的值。                                           |
| LOCALTIMESTAMP               | 提供没有时区的值。                                           |
| LOCALTIME(precision)         | 可以选择使用精度参数，这将使结果在四分之一秒的范围内四舍五入到数位数。 |
| LOCALTIMESTAMP(precision)    | 可以选择使用精度参数，这将使结果在四分之一秒的范围内四舍五入到数位数。 |





## SQL查询计划分析

参考：https://blog.csdn.net/weixin_41287260/article/details/124394206

#### 背景

SQL慢了的原因？

查看下对应的查询计划，从而可以快速定位慢在哪里

#### explain语法

EXPLAIN [ ANALYZE ] [ VERBOSE ] [BUFFERS]



#### 节点类型

- 控制节点（Control Node)
- 扫描节点（ScanNode)
- 物化节点（Materialization Node)
- 连接节点（Join Node)

##### 扫描节点 -- 简单来说就是为了扫描表的元组，每次获取一条元组（Bitmap Index Scan除外）作为上层节点的输入。

- Seq Scan，顺序扫描
  - 全表顺序扫描，一般查询没有索引的表需要全表顺序扫描
- Index Scan，基于索引扫描，但不只是返回索引列的值
  - 索引扫描，主要用来在WHERE 条件中存在索引列时的扫描
- IndexOnly Scan，基于索引扫描，并且只返回索引列的值，简称为覆盖索引
  - 覆盖索引扫描，所需的返回结果能被所扫描的索引全部覆盖
- BitmapIndex Scan，利用Bitmap 结构扫描
  - BitmapIndex Scan 与Index Scan 很相似，都是基于索引的扫描，但是BitmapIndex Scan 节点每次执行返回的是一个位图而不是一个元组，其中位图中每位代表了一个扫描到的数据块。
- BitmapHeap Scan，把BitmapIndex Scan 返回的Bitmap 结构转换为元组结构
  - 而BitmapHeap Scan一般会作为BitmapIndex Scan 的父节点，将BitmapIndex Scan 返回的位图转换为对应的元组。这样做最大的好处就是把Index Scan 的随机读转换成了按照数据块的物理顺序读取，在数据量比较大的时候，这会大大提升扫描的性能。
- Subquery Scan，扫描一个子查询
- ...

扫描节点的小结：

- 大多数情况下，Index Scan 要比 Seq Scan 快。但是如果获取的结果集占所有数据的比重很大时，这时Index Scan 因为要先扫描索引再读表数据反而不如直接全表扫描来的快。
- 如果获取的结果集的占比比较小，但是元组数很多时，可能Bitmap Index Scan 的性能要比Index Scan 好。
- 如果获取的结果集能够被索引覆盖，则Index Only Scan 因为不用去读数据，只扫描索引，性能一般最好。但是如果VM 文件未生成，可能性能就会比Index Scan 要差。



#### explain输出结果描述

PS：

- 按照查询计划树从底往上执行
- 基于火山模型（参考文档[火山模型介绍](http://dbms-arch.wikia.com/wiki/Volcano_Model)）执行，即可以简单理解为每个节点执行返回一行记录给父节点（Bitmap Index Scan 除外）

##### 代价估计信息

- cost 就是该执行节点的代价估计。它的格式是xxx…xxx，在… 之前的是预估的启动代价，即找到符合该节点条件的第一个结果预估所需要的代价，在…之后的是预估的总代价。而父节点的启动代价包含子节点的总代价。(是PostgreSQL 根据周期性收集到的统计信息（参考[PostgreSQL · 特性分析 · 统计信息计算方法](http://mysql.taobao.org/monthly/2016/05/09/)），按照一个代价估计模型计算而来的)
- rows 代表预估的行数（根据表的统计信息预估而来的）
- width 代表预估的结果宽度，单位为字节 (根据表的统计信息预估而来的)

##### 真实执行信息

- actual time 执行时间，格式为xxx…xxx，在… 之前的是该节点实际的启动时间，即找到符合该节点条件的第一个结果实际需要的时间，在…之后的是该节点实际的执行时间
- rows 指的是该节点实际的返回行数
- loops 指的是该节点实际的重启次数。如果一个计划节点在运行过程中，它的相关参数值（如绑定变量）发生了变化，就需要重新运行这个计划节点。





## 高级

#### 视图

视图(VIEW)是一个伪表。 它不是物理表，而是普通表的查询结果，

可以是单个表的部分行，部分列，也可以是多个表的关联结果

作用：

- 以自然和直观的方式构建数据，并使其易于查找
- 限制对数据的访问，使得用户只能看到有限的数据而不是完整的数据
- 归总来自各种表中的数据以生成报表

#### 存储过程

存储在数据库上并可以使用SQL界面调用的一组SQL和过程语句(声明，分配，循环，控制流程等)。 

它有助于在数据库中的单个函数中进行多次查询和往返操作。

```sql
CREATE [OR REPLACE] FUNCTION function_name (arguments)   
RETURNS return_datatype AS $variable_name$  
  DECLARE  
    declaration;  
    [...]  
  BEGIN  
    < function_body >  
    [...]  
    RETURN { variable_name | value }  
  END; LANGUAGE plpgsql;
```

#### 触发器

一组动作或数据库回调函数，当表上执行某些数据库事件(如`INSERT`，`UPDATE`，`DELETE`或`TRUNCATE`语句)时，触发器会被触发而自动运行。 

触发器用于验证输入数据，执行业务规则，保持审计跟踪等。

```sql
CREATE  TRIGGER trigger_name [BEFORE|AFTER|INSTEAD OF] event_name  
ON table_name  
[  
 -- Trigger logic goes here....  
];
```

#### 索引

##### 索引类型

B-tree   Hash   GiST   SP-GiST   GIN

默认情况下，`CREATE INDEX`命令使用**B树**索引

```sql
CREATE INDEX index_name ON table_name;
```

#### 事务

性质：

- 原子性(Atomicity)
- 一致性(Consistency)
- 隔离性(Isolation)
- 持久性(Durability)

事务控制

- BEGIN TRANSACTION：开始事务。

- COMMIT：保存更改，或者您可以使用`END TRANSACTION`命令。

- ROLLBACK：回滚更改。

#### 锁

锁或独占锁或写锁阻止用户修改行或整个表。 在`UPDATE`和`DELETE`修改的行在事务的持续时间内被自动独占锁定。 这将阻止其他用户更改行，直到事务被提交或回退

用户必须等待其他用户当他们都尝试修改同一行时。 如果他们修改不同的行，不需要等待。 SELECT查询不必等待。

数据库自动执行锁定。 然而，在某些情况下，必须手动控制锁定。 手动锁定可以通过使用`LOCK`命令完成。 它允许指定事务的锁类型和范围。

```sql
LOCK [ TABLE ]
name
 IN
lock_mode
```

##### 死锁

当两个事务正在等待彼此完成操作时，可能会发生死锁。 虽然PostgreSQL可以检测到它们并使用`ROLLBACK`结束，但死锁仍然可能不方便。 为了防止您的应用程序遇到此问题，请确保以这样的方式进行设计，以使其以相同的顺序锁定对象。

##### 咨询锁

PostgreSQL提供了创建具有应用程序定义含义的锁的方法。这些称为咨询锁(劝告锁,英文为：**advisory locks**)。 由于系统不强制使用它，因此应用程序正确使用它们。 咨询锁可用于锁定针对MVCC模型策略。

例如，咨询锁的常见用途是模拟所谓的“平面文件”数据管理系统的典型的悲观锁定策略。 虽然存储在表中的标志可以用于相同的目的，但是建议锁更快，避免了表的膨胀，并且在会话结束时被服务器自动清除。









