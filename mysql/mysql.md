
# mysql 数据库优化

- 表的设计合理（符合3NF）
- 添加适当索引（普通索引、主键索引、唯一索引、全文索引）
- 分表技术（水平分割、垂直分割）
- 读写分离
- 存储过程（模块化编程，可以提高速度）
sql 编译->执行->缓存
- Mysql配置优化（配置最大并发数my.ini，调整缓存大小）
- mysql服务器硬件升级
- 定时清除不需要的数据，定时进行碎片整理（myisam）

### 3NF

1NF：既表的列具有原子性，不可再分解，既列的信息，不能分解；只要是关系型数据库，就自动满足1NF；

2NF：表中的记录是唯一

3NF：表中不要有冗余数据，即表的信息如果能够被推导出来，就不应该单独设计一个字段存放

反3NF：在表1对N的情况下，为了提高效率，会在一方添加冗余字段；

### SQL优化

show status

- 常用

show status like 'update'

show status like 'com_select/com_update/com_insert/com_delete'

show [session|global] status like ...
默认为session回话，指取出当前端口的执行情况。如果想看所有，则应该使用global。

show status like 'connetions'

慢查询次数
show status like 'slow_queries' 

- 如何定位慢查询

慢查询时间查询
show variables like 'long_query_time'

修改默认慢查询时间（修改为1秒）
set long_query_time=1

修改语句结束符（默认；为语句结束符）
delimiter $$

- 如何把慢查询的sql记录到日志中

**安全模式启动

- 优化方式

explain语句优化

添加索引来优化sql

.frm 表结构
.MYD 表数据
.MYI 表索引

1、添加

1.1、主键索引添加

`alter table 表名 add primary key(id)`

1.2、普通索引

先创建表，然后再创建普通索引

create index 索引名 on 表（列） 

1.3、全文索引

全文索引主要针对文件、文本的检索，比如文章。全文索引针对于myisam生效。

使用方法是 match(字段名) against(关键字)

fulltext()

1.4、唯一索引

表的某列被指定为unique结束时，这列就是一个唯一索引。

unique字段可以为null，并可以有多个，但是如果是具体的内容，则不能重复。

2、查询索引

desc 表名；

show index from table;

3、删除

alter table 表名 drop index 索引名;

alter table table_name drop  primary key


- 索引使用的注意事项

磁盘占用
对DML语句的效率影响

- 那些列上适合添加索引

较频繁的查询字段

where条件后经常使用的
字段内容不是唯一的几个值
字段内容不是频繁变化的

- 使用索引的注意事项


export MYSQL_HOME=/usr/local/mysql
export PATH=.:$JAVA_HOME/bin:$MYSQL_HOME/bin:$PATH