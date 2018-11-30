## mysql二进制版（5.7）安装
```sh
groupadd mysql
useradd -r -g mysql -s /bin/false mysql

tar zxvf /path/to/mysql-VERSION-OS.tar.gz -C /home/
mv /home/env/mysql-mysql-VERSION-OS /home/mysql


mkdir -p /home/mysql/{data,log,etc,run}
touch /home/mysql/log/error.log
touch /home/mysql/etc/my.cnf

chown -R mysql:mysql /home/mysql
chmod  -R 750 /home/mysql/{data,log,etc,run}

# my.cnf
[client]
port = 3306
socket = /home/mysql/run/mysql.sock

[mysqld]
basedir = /home/mysql
datadir = /home/mysql/data
port = 3306
socket = /home/mysql/run/mysql.sock
pid-file = /home/mysql/run/mysql.pid
default-storage-engine = InnoDB
log-error = /home/mysql/log/error.log
log=/home/mysql/log/mysql.log 

character-set-server=utf8

slow_query_log=1
long_query_time=10 # 设置默认超过时间记录慢查询日志
log-queries-not-using-indexes  = 1 # 未使用索引的查询语句是否记录
#slow-query-log-file = /home/mysql/data/mysql-slow.log

# SAFETY #
max-allowed-packet             = 16M
max-connect-errors             = 1000000
skip-name-resolve
sql-mode                       = STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_AUTO_VALUE_ON_ZERO,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ONLY_FULL_GROUP_BY
sysdate-is-now                 = 1
innodb = FORCE

cd /home/mysql
./bin/mysqld --defaults-file=/home/pmz/mysql/etc/my.cnf --initialize --user=mysql

# stdout 保存最后的临时root 密码 
2018-11-06T10:38:58.716208Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-11-06T10:38:58.716297Z 0 [Warning] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'ERROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will be merged with strict mode in a future release.
2018-11-06T10:38:58.716305Z 0 [Warning] 'NO_AUTO_CREATE_USER' sql mode was not set.
2018-11-06T10:39:00.192116Z 0 [Warning] InnoDB: New log files created, LSN=45790
2018-11-06T10:39:00.446269Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-11-06T10:39:00.561369Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 2c2061e0-e1b0-11e8-ad3c-fa163e2f72d5.
2018-11-06T10:39:00.583874Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2018-11-06T10:39:00.584786Z 1 [Note] A temporary password is generated for root@localhost: gUhYy71rdV#4

# bin/mysql_ssl_rsa_setup 

./bin/mysqld_safe --defaults-file=/home/mysql/etc/my.cnf --user=mysql &

ln -s /home/mysql/run/mysql.sock /tmp/mysql.sock 
             
./bin/mysql -u root --skip-password

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
grant all privileges  on *.* to root@'%' identified by "new_password";
flush privileges;
use mysql 
select * from user;
# update user set host='%' where host='localhost' and user = 'root'；

# Next command is optional
cp support-files/mysql.server /etc/init.d/mysqld
systemctl enable mysqld
systemctl start mysqld
systemctl stop mysqld
./bin/mysqladmin -uroot -S ./run/mysql.sock -p shutdown

set password =password(‘123456‘);
mysqladmin -uroot password “666666”;
```
## 主从
* 一二级索引
每个InnoDB表具有一个特殊的索引称为聚簇索引（也叫聚集索引，聚类索引，簇集索引）。如果表上定义有主键，该主键索引就是聚簇索引。如果未定义主键，MySQL取第一个唯一索引（unique）而且只含非空列（NOT NULL）作为主键，InnoDB使用它作为聚簇索引。如果没有这样的列，InnoDB就自己产生一个这样的ID值，它有六个字节，而且是隐藏的，使其作为聚簇索引。

表中的聚簇索引（clustered index ）就是一级索引，除此之外，表上的其他非聚簇索引都是二级索引，又叫辅助索引（secondary indexes）。

* show priviledges
* GTID是master产生的自增ID，每个事务唯一标识，它由server_uuid + 自增序号 构成。因此，不同mysql节点产生的GTID必然不同，因此在整个集群全局不会重复。
auto.cnf
在整个复制架构中GTID 是不变化的,即使在多个连环主从中也不会变。
例如：ServerA --->ServerB ---->ServerC 
GTID从在ServerA ,ServerB,ServerC 中都是一样的。

二、 GTID的工作原理：
1、master更新数据时，会在事务前产生GTID，一同记录到binlog日志中。
2、slave端的i/o 线程将变更的binlog，写入到本地的relay log中。
3、sql线程从relay log中获取GTID，然后对比slave端的binlog是否有记录。
4、如果有记录，说明该GTID的事务已经执行，slave会忽略。
5、如果没有记录，slave就会从relay log中执行该GTID的事务，并记录到binlog。
6、在解析过程中会判断是否有主键，如果没有就用二级索引，如果没有就用全部扫描。

* gtid_executed(global)：MySQL数据库已经执行过的Gtid事务，处于内存中。show master status/show slave status中的Executed_Gtid_Set也取自这里
* gtid_purged(global)：由于binlog文件的删除(如purge binary logs或者超过expire_logs_days设置)已经丢失的Gtid事务，它是gtid_executed的子集  
select @@server_uuid;
### master
```sh
log-bin = mysql-bin #配置文件里的参数和show variables中的参数不一致，例如my.cnf里log-bin在show variables的时候看到的是log_bin
server-id = 10
expire-logs-days               = 14
sync-binlog                    = 1
binlog-do-db=
binlog-ignore-db=
binlog-format=ROW
binlog-row-image = minimal

gtid_mode = ON #mysql.gtid_executed 记录同步复制的信息
enforce-gtid-consistency=true
binlog_gtid_simple_recovery = 1
log-slave-updates=true
master-info-repository=TABLE
relay-log-info-repository=TABLE
sync-master-info=1

auto-increment-offset = 1
auto-increment-increment = 5

#report-host=10.11.15.114
#report-port=3306
## 主主互备
relay-log = mysql-relay-bin
skip-slave-start = 1
replicate-do-db = 
replicate-wild-ignore-table=mysql.%   #指定不需要复制的库，mysql.%表示mysql库下的所有对象
replicate-wild-ignore-table=information_schema.%
replicate-wild-ignore-table=performance_schema.%
replicate-wild-ignore-table=sys.%
#slave-skip-errors=all  # 害怕遇到错误停止同步，忽略所有错误
slave_parallel_type=LOGICAL_CLOCK
slave-parallel-workers=4 
relay-log-recovery=1
```

### slave
```sh
[mysqld]
server-id=11  # 配置本台机器mysql的id
auto-increment-offset = 2
auto-increment-increment = 5
```
```sh
show grants for root@'%';
show global variables like '%gtid%';
grant replication slave on *.* to 'repli'@'%' identified by 'repli';
flush privileges;
  
FLUSH TABLES WITH READ LOCK;
mysqldump

RESET MASTER
UNLOCK TABLES;

STOP SLAVE IO_THREAD
SHOW PROCESSLIST
STOP SLAVE and RESET MASTER.

DROP DATABASE db1;

    DROP DATABASE db2;

    8.导入数据

    SOURCE /root/bak.sql;

    9.重置slave服务

    RESET SLAVE;
# 配置主从同步	
stop slave;
reset slave;
CHANGE MASTER TO MASTER_HOST='172.16.1.151',MASTER_PORT=3306,MASTER_USER='repli',MASTER_PASSWORD='repli',master_auto_position=1;
start slave;
show processlist;

# 增加从库
先masterA锁表-->masterA备份数据-->masterA解锁表 -->masterB导入数据-->masterB设置主从-->查看主从　

MASTER_LOG_FILE='binlog.000001',
　　MASTER_LOG_POS=1304; #后面两个参数的值与主库保持一致

```
### 管理

create table test_repli
(
  id         int auto_increment
    primary key,
  name  varchar(255) null
  )
  collate = utf8_bin;

 #将指定时间之前的日志清理
purge binary logs before '2018-02-01 12:00:00';

#将指定日志文件之前的日志清除
purge binary logs to 'mysql-bin.000003';

show master status\G 
show slave status\G  
show variables like 'expire_logs_days';  
## [MySQL的变量分类总结](http://www.cnblogs.com/kerrycode/p/9021378.html)
局部变量与用户自定义变量的区分在于下面这些方面:
1.用户自定义变量是以"@"开头的。局部变量没有这个符号。
2.定义变量方式不同。用户自定义变量使用set语句，局部变量使用declare语句定义
3.作用范围不同。局部变量只在begin-end语句块之间有效。在begin-end语句块运行完之后，局部变量就消失了。而用户自定义变量是对当前连接（会话）有效。

## 自启
```sh
# mysqld.service

[Unit]
Description=MySQL3306
After=network.target
After=syslog.target

[Service]
User=mysql # 此用户必须存在，即为启动mysql的用户
PIDFile=/var/run/mysqld/mysqld.pid
Type=forking
ExecStart=/home/ygsh/env/mysql/bin/mysqld_safe --defaults-file=/etc/my.cnf --user=mysql &
LimitNOFILE=5000

[Install]
WantedBy=multi-user.target
```

[mysql manual in chinase](http://doc.mysql.cn/mysql5/refman-5.1-zh.html-chapter/ "中文")  
[w3c mysql manual](http://www.w3school.com.cn/sql/sql_alter.asp)  
[runoob mysql manual](http://www.runoob.com/mysql/mysql-database-import.html)

### sql查找mysql安装目录
`select @@basedir;`
### sql查看链接数
`show full processlist;`
### cmd查看mysql版本
`mysql -V`  
`select version()`

### my.cnf 配置
```
[mysqld]

port = 3308

character-set-server=utf8

#basedir=D:/soft/package/mysql-5.6

#datadir=D:/soft/package/mysql-5.6/data

default-storage-engine=INNODB

collation-server=utf8_general_ci
[mysql]

default-character-set=utf8

```
### 允许root用户远程登陆
```sql
use mysql
update user set host='%' where User='root' and host='localhost' limit 1;
flush privileges;

```
或者
用root用户 进入 mysql

```sql
grant all on CommGuard.* to 'root'@'localhost' IDENTIFIED BY '你的密码';
flush privileges;
```
### 将MySQL 添加到服务中。


在Windows Run中输入cmd，这时上面有提示（cmd.exe），右键单击cmd.exe, 选择Run as administrator，进入路径： d:/appspace/mysql /bin>

输入  mysqld --install MySQL --defaults-file="C:\Windows\my.ini"

要指定defaults-file.

命令行中输入services.msc回车，可以看到MySQL已被添加到Services中，

Path to executable中的内容为

d:\appspace\mysql\bin\mysqld --defaults-file=C:\windows\my.ini MySQL
### 删除mysql服务
mysqld --remove Mysql
### 开启/关闭mysql服务
net start/stop mysql
### 查看字符集
`show variables like 'character%'`;

### 删除数据库
`drop database if exists drop_database`

### 删除数据库所有表
```sql
use information_schema;
select concat('drop table ',table_name,';') from tables where TABLE_SCHEMA = '数据库名'; // 注意有空格
# 拼接删除SQL
SET FOREIGN_KEY_CHECKS=0;#取消外键约束状态
```
### 创建表
```sql
CREATE TABLE `user` (
  `id` INT(32) NOT NULL,
  `name` varchar(100) DEFAULT NULL,
  `psw` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 是在命令行中查询显示乱码（DOS不支持UTF8从MYSQL中显示），解决如下：

mysql> set names gbk;
sql 对大小写不敏感
日期
基于集合理论

## 在创建mysql数据库的时候如何支持UTF-8编码

1. 用工具  
CHARSET 选择 utf8
COLLATION 选择 utf8_general_ci
2. 用SQL语句  
GBK: CREATE DATABASE `test1` DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
UTF-8: CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
XHG!;3sM2cPQ
### 导出
```sql
mysqldump [OPTIONS] database [tables]
mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
mysqldump [OPTIONS] --all-databases [OPTIONS]

-B db1 db2
-S sock
mysqldump -h localhost -P3306 -u root -p mydb >e:\mysql\mydb.sql
- 然后输入密码，等待一会导出就成功了，可以到目标文件中检查是否成功。
- 2.将数据库mydb中的mytable导出到e:\mysql\mytable.sql文件中：
mysqldump -h localhost -u root -p mydb mytable>e:\mysql\mytable.sql
- 3.将数据库mydb的结构导出到e:\mysql\mydb_stru.sql文件中：
mysqldump -h localhost -u root -p mydb -d --add-drop-database --add-drop-table >e:\mysql\mydb_stru.sql
- 3.将数据库mydb中 某表的表结构导出到e:\mysql\mydb_stru.sql文件中：
mysqldump -h localhost -u root -p -B mydb --table tableName --opt >e:\mysql\mydb_stru.sql

show variables like '%secure%';
select phone,create_date from table  into outfile '/var/lib/mysql-files/p_wechat_bind.csv' FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n';
```

### 导入
```sql
mysql -h localhost -u root -p mydb2 < e:\mysql\mydb2.sql
然后输入密码，就OK了。
```
### 解决cmd下，乱码问题
你也可以使用 在 windows 中 dos 中 mysql -uroot --default-character-set=gbk
连接方式 （注意 在Windows 下 不管你的数据是什么格式的 都得用gbk ,原因是 dos 中文只支持 gbk ）

### 删除表数据
1. truncate table wp_comments;
2. delete * from wp_comments;  

其中truncate操作中的table可以省略，delete操作中的*可以省略。这两者都是将wp_comments表中数据清空，不过也是有区别的，如下：
* truncate是整体删除（速度较快）， delete是逐条删除（速度较慢）。  
* truncate不写服务器log，delete写服务器log，也就是truncate效率比delete高的原因。  
* truncate不激活trigger(触发器)，但是会重置Identity（标识列、自增字段），相当于自增列会被置为初始值，又重新从1开始记录，而不是接着原来的ID数。而delete删除以后，Identity依旧是接着被删除的最近的那一条记录ID加1后进行记录。  
如果只需删除表中的部分记录，只能使用DELETE语句配合where条件。 DELETE FROM wp_comments WHERE ...

 ### order by排序 返回游标不能做表达式
 `select atime, deathnumber, province from full_accident_tbl where (deathnumber>=5 and deathnumber<=10) and province='山东' order by 2`

### 分组统计
```sql
 select province, sum(deathnumber) from full_accident_tbl where (deathnumber>=5 and deathnumber<=10) group by province;  
+----------+------------------+
| province | sum(deathnumber) |
+----------+------------------+
| 上海     | 151              |
| 云南     | 1828             |
| 内蒙     | 695              |
| 北京     | 165              |
| 吉林     | 315              |
| 四川     | 1565             |
| 天津     | 84               |
| 宁夏     | 163              |
| 安徽     | 881              |
| 山东     | 1342             |
| 山西     | 1251             |
| 广东     | 1435             |
| 广西     | 966              |
| 新疆     | 818              |
| 江苏     | 569              |
| 江西     | 599              |
| 河北     | 723              |
| 河南     | 895              |
| 浙江     | 1289             |
| 海南     | 151              |
| 湖北     | 754              |
| 湖南     | 1601             |
| 甘肃     | 825              |
| 福建     | 937              |
| 西藏     | 435              |
| 贵州     | 2469             |
| 辽宁     | 772              |
| 重庆     | 915              |
| 陕西     | 957              |
| 青海     | 210              |
| 黑龙江   | 984              |
+----------+------------------+
31 rows in set
```
### 分组计数
```sql
select t1.pwd ,count(t1.pwd) from newuser t1 group by t1.pwd;

+----------+---------------+
| pwd      | count(t1.pwd) |
+----------+---------------+
| 123      |             2 |
| 123123   |             1 |
| 123456   |            15 |
| 123465   |             1 |
| 654321   |             1 |
| yr910118 |             1 |
+----------+---------------+
```
### 分组过滤
```sql
select province, sum(deathnumber) from full_accident_tbl where (deathnumber>=5 and deathnumber<=10) group by province having sum(deathnumber) <200;
+----------+------------------+
| province | sum(deathnumber) |
+----------+------------------+
| 上海     | 151              |
| 北京     | 165              |
| 天津     | 84               |
| 宁夏     | 163              |
| 海南     | 151              |
+----------+------------------+
5 rows in set

select t1.pwd ,count(t1.pwd) from newuser t1 group by t1.pwd having count(pwd) <= 2;
| pwd      | count(t1.pwd) |
+----------+---------------+
| 123      |             2 |
| 123123   |             1 |
| 123465   |             1 |
| 654321   |             1 |
| yr910118 |             1 |
+----------+---------------+

```

### 子查询
```sql
-- 当我们在 WHERE 子句或 HAVING 子句中插入另一个 SQL 语句时，我们就有一个 subquery 的架构。第一，它可以被用来连接表格。另外，有的时候 subquery 是唯一能够连接两个表格的方式。

SELECT SUM(Sales) FROM Store_Information WHERE Store_name IN (SELECT store_name FROM Geography
WHERE region_name = 'West');
```
### UNION
```sql
--  该指令的目的是将两个 SQL 语句的结果合并起来。从这个角度来看， UNION 跟 JOIN 有些许类似，因为这两个指令都可以由多个表格中撷取资料。 UNION 的一个限制是两个 SQL 语句所产生的列需要是同样的资料种类和个数。另外，当我们用 UNION 这个指令时，我们只会看到不同的资料值 (类似 SELECT DISTINCT)。  

-- UNION ALL 这个指令的目的也是要将两个 SQL 语句的结果合并在一起。 UNION ALL 和 UNION 不同之处在于 UNION ALL 会将每一笔符合条件的资料都列出来，无论资料值有无重复。

-- 和 UNION 指令类似，INTERSECT 也是对两个 SQL 语句所产生的结果做处理的。不同的地方是， UNION 基本上是一个 OR (如果这个值存在于第一句或是第二句，它就会被选出)，而 INTERSECT 则比较像 AND ( 这个值要存在于第一句和第二句才会被选出)。UNION 是联集，而 INTERSECT 是交集。
-- 请注意，在 INTERSECT 指令下，不同的值只会被列出一次。
```

### mysql 命令下执行脚本

第一种方式：在未连接数据库的情况下，输入 `mysql -h localhost -u root -p 123456 database < d:\book.sql` 回车即可；

第二种方式：在已连接数据库的情况下，此时命令提示符为mysql>，输入` source d:\book.sql`   回车即可。

### 聚合查询`oracle`
```sql

SELECT
	DQBM_BM.province as 省,
	WKKDB_BM.WKKDBMC as 等别,
	COUNT (*) as 数量,
	SUM (
		DECODE (SFAQSCXKZ, '是', 1, 0)
	) AS 已取得安全生产许可证,
	sum(DECODE (SFAZZXJCXT, '是', 1, 0)) AS 已安装在线监测设备
FROM
	WKK
join WKKDB_BM on WKK.WKKDB = WKKDB_BM.WKKDB
JOIN DQBM_BM ON WKK.DQBM = DQBM_BM.DQBM
GROUP BY
	WKKDB_BM.WKKDBMC,
	DQBM_BM.PROVINCE;

--别名
SELECT
	QY.QYDZ AS 企业地址,
	COUNT (s.qyid) AS 企业数量,
	COUNT (S.AQSCXKZBH) AS 安全生产许可证,
	SUM (s.CYRYSL) AS 从业人员数量,
	SUM (s.yycl) AS 原油产量,
	SUM (s.trqcl) AS 天然气产量,
	SUM (s.yjsl),
	SUM (GHLYJSL) AS 高含硫油井数量
FROM
	SYK s
JOIN QYQK qy ON QY.QYID = s.QYID
WHERE
	s.syfl = 1 -- 1-> 陆油， 2-> 海油
AND s.ZYKCJZ = 1 -- 1 -> 原油, 2 -> 天然气
GROUP BY
	QY.qydz;
```

### EXISTS
将外查询表的每一行，代入内查询作为检验，如果内查询返回的结果取非空值，则EXISTS子句返回TRUE，这一行行可作为外查询的结果行，否则不能作为结果。
### NULL处理
* IS NULL: 当列的值是NULL,此运算符返回true。
* IS NOT NULL: 当列的值不为NULL, 运算符返回true。
* <=>: 比较操作符（不同于=运算符），当比较的的两个值为NULL时返回true。
### 正则匹配
`SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';`

### SELECT INTO导入
`SELECT * INTO Persons IN 'Backup.mdb' FROM Persons`
### sql删除列
`ALTER TABLE bying_flow	DROP COLUMN last_modify_time;`

* 修改字段默认值

alter table 表名 drop constraint 约束名字   ------说明：删除表的字段的原有约束

alter table 表名 add constraint 约束名字 DEFAULT 默认值 for 字段名称 -------说明：添加一个表的字段的约束并指定默认值

* 修改字段名：

alter table 表名 rename column A to B

* 修改字段类型：

alter table 表名 alter column UnitPrice decimal(18, 4) not null

* 修改增加字段：

alter table 表名 ADD 字段 类型 NOT NULL Default 0

```sql
ALTER TABLE `message`
	ADD COLUMN `parent_id` int(11) NULL AFTER `id`,
	MODIFY COLUMN `to_id` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '消息对象的id，如评论他人，则应该是他人的评论的id' AFTER `from_id`,
	ADD COLUMN `from_user_nick` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NULL AFTER `to_id`,
	ADD COLUMN `from_user_head` varchar(255) NULL AFTER `from_user_nick`,
	ADD COLUMN `to_user_nick` varchar(255) NULL AFTER `from_user_head`,
	ADD COLUMN `to_user_head` varchar(255) NULL AFTER `to_user_nick`,
	ADD COLUMN `star_level` int(1) NULL COMMENT '评分' AFTER `to_user_head`,
	MODIFY COLUMN `content` varchar(2000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL AFTER `star_level`,
	MODIFY COLUMN `type` int(2) NULL DEFAULT 1 COMMENT '1，反馈；2，评论，3维修单' AFTER `email`,
	MODIFY COLUMN `rel_id` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '当type为评论时此id放plan_id；当type为维修单时此id放厂商id；' AFTER `create_op`,
	MODIFY COLUMN `temp` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '当type为3（维修单）时，此字段存储维修单id' AFTER `last_modify_time`,
	MODIFY COLUMN `temp2` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL COMMENT '当type为3（维修单）时，此字段存储维修单所属的订单id' AFTER `temp`;

```
## stored procedure
* 语法
```sql
CREATE
    [DEFINER = { user | CURRENT_USER }]
    PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

CREATE
    [DEFINER = { user | CURRENT_USER }]
    FUNCTION sp_name ([func_parameter[,...]])
    RETURNS type
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

func_parameter:
    param_name type

type:
    Any valid MySQL data type

characteristic:
    COMMENT 'string'
  | LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }

routine_body:
    Valid SQL routine statement
```

* example  

```sql
-- ----------------------------
-- Procedure structure for `proc_adder`
-- ----------------------------
DROP PROCEDURE IF EXISTS `proc_adder`;
DELIMITER ;;
CREATE DEFINER=`root`@`localhost` PROCEDURE `proc_adder`(IN a int, IN b int, OUT sum int)
BEGIN
    #Routine body goes here...

    DECLARE c int;
    if a is null then set a = 0;
    end if;

    if b is null then set b = 0;
    end if;

    set sum  = a + b;
END
;;
DELIMITER ;
```
* 存储过程中变量赋值  
语法1：  
SET var_name=expr [, var_name=expr]...;  
语法2：  
SELECT col_name[,...] INTO var_name[,...] from table [WHERE...];  

## stored function  
* example
```sql
DELIMITER //
CREATE FUNCTION addTwoNumber(x SMALLINT UNSIGNED, Y SMALLINT UNSIGNED)
RETURNS SMALLINT
BEGIN
DECLARE a, b SMALLINT UNSIGNED DEFAULT 10;
SET  a = x, b = y;
RETURN a+b;
END//
```

# [索引](https://segmentfault.com/a/1190000003072424)

```sql
SHOW FULL PROCESSLIST ;
SHOW VARIABLES LIKE 'long%';
SET LONG_QUERY_TIME = 2;

SHOW VARIABLES LIKE 'slow%';
SET GLOBAL SLOW_QUERY_LOG = 'ON';


```
* 确定字符串索引长度  
 count(distinct left(列名, 索引长度))/count(*)  
* 字段区分度计算  
SELECT count(DISTINCT(bill_id))/count(*) AS Selectivity FROM p_order;


* [查询分析](https://cloud.tencent.com/developer/article/1005175)
* [查询优化](https://cloud.tencent.com/developer/article/1004912)
* [sleep(n)](https://blog.csdn.net/zyz511919766/article/details/42241211)

* [MySQL的filesort](http://mingxinglai.com/cn/2016/04/mysql-filesort/)  
`SHOW variables LIKE '%sort%';`


## shell 中sql
```sql
mysql -uroot -pxxx db --skip-column-names -se "sql"
```

## varchar 转 langint
select crc32('hello_world');

## 数据库表结构导出工具
* https://github.com/voidint/tsdump

## mysql data 目录迁移

```sh
mkdir -p /home/data/run
systemctl stop mysqld
cp -r /var/lib/mysql/* /home/data/
chown -R mysql:mysql /home/data
# vim /etc/my.cnf
socket=/home/data/run/mysql.sock
datadir=/home/data

systemctl restart mysqld

mysql -u root -p
show variables like '%dir%';
```