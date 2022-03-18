# MySQL-Route

#### 介绍
MySQL学习路线


#### 从零开始MySQL 
+ 关系型数据库
    + 关系型数据库的特点
        +  数据结构化存储在二维表中
        +  支持事务的原子性，一致性，隔离性，持久性
        +  支持使用SQL语言对存储在其中的数据进行操作
    + 关系型数据库的适用场景
        +  数据之间存在着一定关系，需要关联查询数据的场景
        +  需要事务支持的业务场景
        +  需要使用SQL语言灵活操作数据的场景。
+ 非关系型数据库
    +  非关系型数据库的特点
        +  存储结构灵活，没有固定的结构
        +  对事务的支持比较弱，但对数据的并发处理性能高
        +  大多不使用SQL语言操作数据 
    +  非关系型数据库的适用场景
        + 数据结构不固定的场景
        + 对事务要求不高，但读写并发比较大的场景
        + 对数据的处理操作比较简单的场景。
+ 关系数据库选型原则
    + 数据库使用的广泛性
    + 数据库的可扩展性
        + 支持基于二进制日志的逻辑复制
        + 存在多种第三方数据库中间层，支持读写分离及分库分表
    + 数据库的安全性和稳定性
        +  MySQL主从复制集群可达到99%的可用性
        +  配合主从复制高可用架构可以达到99.99%的可用性 
        +  支持对存储在MySQL上的数据进行分级安全控制
    + 数据库所支持的系统
+ 数据库结构设计
    + 业务分析-> 逻辑设计 -> 数据类型-> 对象命名 -> 建立库表
+ 宽表模式存在的问题（适用于列存储的数据报表应用）
    +  数据插入异常： 部分数据由于缺失主键信息而无法写入表中 
    +  数据更新异常： 修改一行中某列的值时，同时修改了多行数据
    +  数据删除异常： 删除某一数据时不得不删除另一条数据
    +  数据冗余： 相同的数据在一个表中出现了多次。 
+ 数据库设计范式
    + 第一范式： 表中的所有字段都是不可再分的
    + 第二范式： 表中必须存在业务主键，并且非主键依赖于全部业务主键 
    + 第三范式： 表中的非主键列之间不能相互依赖
+ MySQL常见的存储引擎
    + MYISAM： MySQL 5.6之前的默认引擎，最常用的非事务型存储引擎
    + CSV：以CSV格式存储的非事务型存储引擎
    + Archive ： 只允许查询和新增数据而不允许修改的非事务型存储引擎
    + Memory : 是一种易失性非事务型存储引擎
    + INNODB ： 最常用的事务型存储引擎 
        +  事务型存储引擎支持ACID 
        +  数据按主键聚集存储
        +  支持行级锁及MVCC 
        +  支持Btree 和自适应hash 索引
        +  支持全文和空间索引 
+ 如何为数据选择合适的数据类型
    +  有限选择符合存储数据需求最小的数据类型
    +  谨慎使用ENUM，TEXT字符串类型
    +  同财务相关的数值型数据，必须使用decimal类型。
    +  decimal类型存储是9个字节。小数点前是4位，小数点后是4位，小数点占一位
+ 如何为表和列选择适合的名字
    +  所有数据库对象名称必须使用小写字母可选用下划线分割
    +  所有数据库对象名称定义禁止使用MySQL保留关键字
    +  数据库对象的命名要能做到见名识义，并且最好不要超过32个字
    +  临时库表必须以tmp为前缀并以日期为后缀
    +  用于备份的库，表必须以bak为前缀并以日期为后缀
    +  所有存储相同数据的列明和列类型必须一致 
+ DCL (Data Control Language) 访问控制语句
    + 建立数据库账号：create user
        + CREATE USER [IF NOT EXISTS] user [auth_option] [, user [auth_option]] ... [REQUIRE {NONE | tls_option [[AND] tls_option] ...}] [WITH resource_option [resource_option] ...]
    [password_option | lock_option] ...
    + 对用户授权：grant 
        +  priv_type [(column_list)]
      [, priv_type [(column_list)]] ... ON [object_type] priv_level TO user [auth_option] [, user [auth_option]] ... [REQUIRE{NONE | tls_option [[AND] tls_option] ...}] [WITH {GRANT OPTION | resource_option} ...]
    + 收回用户权限：revoke
        +  REVOKE priv_type [(column_list)] [, priv_type [(column_list)]] ... ON [object_type] priv_level FROM user [, user] ...
+ DDL(data definition lanuage)
    + 建立/修改/删除数据库：create/alter/drop database
        + CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [create_option] ...
    + 建立/修改/删除表 ： create/alter/drop table
        + CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)
    [table_options]
    [partition_options]
    + 建立/删除索引：create/drop index
    + 清空表：truncate table 
    + 重命名表：rename table
    + 建立/修改/删除试图：create/alter/drop view
+ DML (data manipulation language)
    + 新增表中数据：insert into 
        + INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE] [INTO] tbl_name [PARTITION (partition_name [, partition_name] ...)] [(col_name [col_name] ...)]{VALUES | VALUE} (value_list) [, (value_list)] ...[ON DUPLICATE KEY UPDATE assignment_list]
        + 确认要把数据插入到那个表中 
        + 确认表的数据库结构，那些列不能为NULL，那些列可以为NULL,对于不能为NULL的列是否有默认值（class_name）
        + 确认对应插入列的插入之的清单 values
    + 删除表中数据：delete
        + DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM tbl_name[PARTITION (partition_name [, partition_name] ...)][WHEREwhere_condition][ORDER BY ...][LIMIT row_count]
        + 确定要删除的数据存储在那张表中 From 子句
        + 确认删除数据的过滤条件 where字句
        + 确认是否只删除有限条数据 ORDER BY ... LIMIT 子句 
    + 修改表中的数据： update
        + UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET assignment_list[WHERE where_condition][ORDER BY ...][LIMIT row_count]
        + 确定要更新的数据存储在那张表中 UPDATE子句
        + 确定要列新的列及值 SET子句
        + 确认更新数据的条件 WHERE子句 
    + 查询表中的数据:  select 
        + SELECT [ALL | DISTINCT | DISTINCTROW ] [HIGH_PRIORITY] [STRAIGHT_JOIN][SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT] [SQL_CACHESQL_NO_CACHE] [SQL_CALC_FOUND_ROWS] select_expr [, select_expr] ...[into_option] [FROM table_references [PARTITION partition_list]] [WHERE where_condition] [GROUP BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]] [HAVING where_condition] [ORDER BY {col_name | expr | position}[ASC | DESC], ...] [LIMIT {[offset,] row_count | row_count OFFSET offset}][PROCEDURE procedure_name(argument_list)] [into_option][FOR UPDATE | LOCK IN SHARE MODE]
        +  首先确定我们要获取的数据存在那些表中
        +  其次要确定我们要取现表中的那些列
        +  确认是否需要对表中的数据进行过滤
    + MySQL比较运算符
        + != < > >= <= 
        + between and  列的值大于等于最小值，小于等于最大值
        + IS NULL,IS NOT NULL 判断列的值是否为NULL
        + LIke NOT LIKE  %代表任何数量的字符，_代表任何一个字符
        + IN ,NOT IN  判断列的值是否在指定的范围内
        + AND && AND运算符两边的表达式都为真时，返回的结果才为真
        + OR || OR运算符两边的表达式有一条为真，返回的结果就为真
        + XOR XOR运算符两边的表达式一真一假时返回真，两真两假为假
        + 任何运算符和NULL值运算结果都为NULL 
    + 使用JOIN关联多个表
        + INNER JOIN 
            + select <select_list> from table a inner join table b on a.key = b.key
        + OUTE JOIN
            + select <select_list> from table a left join table b on a.key = b.key
            + select <select_list> from table a left join table b on a.key = b.key where b.key IS NULL 
            + select <select_list> from table a right join table b on a.key = b.key
            + select <select_list> from table a right join table b on a.key = b.key where b.key IS NULL 
    + GROUP BY ... HAVING 子句的作用
        +  把结果集按照某些列分成不同的组，并对分组后的数据进行聚合操作
            +  select level_name,count(*) from db group by level name
        +  可以通过可选的having子句对聚合后的数据进行过滤 
            +  select class_name,level_name,count(*) from course a join class b on b.class_id = a.class_id 
            join level c on c.level_id = a. level_id 
            group by class_name,level_name having count(*) >3 
    + 常用的聚合函数
        + count(*) 计算符合条件的数据行数
        + sum 计算表中符合条件的数值列的合计算
        + AVG() 计算表中符合条件的数值列的平均值
        + MAX() 计算表中符合条件的任意列中数据的最大值
        + MIN() 计算表中符合条件的任意列中数据的最小值
    + 使用Order by字句对查询结果进行排序
        + 使用order by 字句是对查询结果进行排序的最安全方法
        + 列名后增加ASC关键字指定按该列的升序进行排序，或是指定DESC关键字指定该列的降序进行排序
        + Order by 子句 也可以使用select子句中未出现的列或是函数
    + 使用Limit字句限制返回结果集的行数
        + 常用语数据列表分页
        + 一定要和order by 子句配合使用
        + limit 起始偏移量，结果集的行数
    + 创建视图
        + CREATE[OR REPLACE][ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}][DEFINER = user][SQL SECURITY { DEFINER | INVOKER }]VIEW view_name[(column_list)]AS select_statement[WITH [CASCADED | LOCAL] CHECK OPTION]
    + 系统函数
        + CURDATE()/CURTIME() 返回当前日期/返回当前时间
        + NOW() 返回当前的日期和时间
        + DATE_FORMAT(date,fmt) 按照fmt的格式，对日期date进行格式化
        + DATEDIFF(date1,date2)返回date1和date2两个日期相差的天数
        + DATE_ADD(date,inter val expr unit)对给定的日期增加或减少指定的时间单元
        + EXTRACT(unit FROM date) 返回日期date的指定部分
        + UNIX_TIMESTAMP 返回unix时间戳
        + FROM_UNIXTIME() 把unix时间戳转换为日期时间
    + 常用的字符串函数
        + CONCAT(str1,str2) 把字符串str1,str2连接成一个字符串
        + CONCAT_WS(sep,str1,str2)用指定的分隔符sep连接字符串
        + LEFT(str,len)/RIGHT(str,len) 从字符串左/右边起返回len长度的子字符串
        + SUBSTRING(str,pos,[len]) 从字符串str的pos位置起返回长度为len的子串
    + 其他常用函数
        + ROUND(X,D) 四舍五入
        + RADN() 返回在0和1之间的随机数
+ 优化SQL 
    + 发现问题
        + 分析慢查询日志发现存在问题的SQL
            + 配置慢查询日志
                + set global show_query_log = [ON|OFF]
                + set global show_query_log_file = /sql_log/slowlog.log
                + set global long_query_time = xx.xxx秒
                + set global log_queries_not_using_indexed = [ON|OFF]
            + 分析mysql慢查询日志
                + mysqldumpslow[OTPS][LOGS]
                + pt-query-digest[OPTIONS][FILES][DSN] 
        + 数据库实时监控长时间运行的SQL
            +  select id,user,host,DB,command,time,state,info from db.PROCESSLIST where time >60 
    + 分析执行计划
        + explain SQL 
            + ID
                + ID 表示查询执行的顺序
                + ID 相同时由上到下执行
                + ID 不同时，由大到小执行
            + select_type
                + SIMPLE 不包含子查询或者UNION操作的查询
                + PRIMARY 查询中如果包含任何子查询，那么最外层的查询则被标记为PRIMARY
                + SUBQUERY select 列表中的子查询
                + DEPENDENT SUBQUERY 依赖外部结果的子查询
                + UNION union操作的第二个或者之后的查询的值为union
                + DEPENDENT UNION 当 UNION做为子查询时，第二或者第二后的查询的select_type值
                + UNION RESULT UNION产生的结果集
                + DERIVED 出现在FROM子句中的子查询
            + partitions:
                + 对于分区表，显示查询的分区ID
                + 对于非分区表，显示NULL
            + type
                + system ： 这是const联接类型的一个特例，当查询的表只有一行时使用
                + const: 表中有且只有一个匹配的行时使用，如对主键或是唯一索引的查询，这是效率最高的联接方式
                + eq_ref:唯一索引或主键查找，对于每个索引键，表中只有一条记录与之匹配
                + ref  非唯一索引查找，分会匹配某个单独值的所有行
                + ref_or_null 类似于ref类型的查询，但是附加了对NULL值列的查询
                + index_merge 该联接类型表示使用了索引和并优化方法
                + range 索引范围扫描，常见于between ＜　＞这样的查询条件
                + index FULL index SCAN 全索引扫描，同ALL的区别是，遍历的是索引树
                + ALL FULL TABLE SCAN 全表扫描 这是效率最差的链接方式
            + possible_key:指出查询中可能会用到的索引
            + key:指出查询时实际用到的索引
            + key_len：实际使用索引的最大长度
            + ref : 指出那些列或常量被用于索引查找
            + row : 根据统计信息预估的扫描的行数
            + filtered：表示返回结果的行数占需读取行数的百分比
    + 优化索引
        + 索引的作用：指导存储引擎如何快速的查找到所需要的数据
        + Innodb 支持的索引
            + Btree索引
                + 以B+树的结构存储索引数据
                + 适用于全值匹配的查询
                + 适合处理范围查找
                + 从索引的最左侧开始匹配查找列
                + 限制：
                    +  只能从最左侧开始按索引键的顺序使用索引，不能跳过索引键
                    + NOT IN 和 <>操作无法使用索引
                    + 索引列上不能使用表达式或是函数
            + 自适应HASH索引
            + 全文索引
            + 空间索引
        + 如何建立索引
            + 应该在where子句中，或者 具有筛选性的列 建立索引
            + 包含在ORDER BY. GROUP BY 、DISTINCT中的字段
            + 多表JOIN的关联列
        + 如何选择复合索引键的顺序
            + 区分度最高的列放在联合索引的最左侧
            + 使用最频繁的列放到联合索引的最左侧
            + 尽量把字段长度小的列放在联合索引列的最左侧
    + 改写SQL
    + 数据库垂直切分，数据库水平切分
+ SQL
    + 事务
        + 原子性：一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节
        + 一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏
        + 隔离性：事务的隔离性要求每个读写事务的对象与其他事务的操作对象相互分离，即该事务提交前对其它事务都不可见
        + 事务一旦提交了，其结果就是永久性，就算发生了宕机等事故，数据库也能将数据恢复。
    + 并发带来的问题
        + 更新丢失：最后的更新覆盖了其他事务之前的更新，而事务之间并不知道，发生更新丢失。
        + 脏读： 一个事务读取了另一个事务未提交的数据
        + 不可重复读：一个事务前后两次读取的同一数据不一致
        + 幻读：一个事务两次查询的结果记录数不一致
    + Innodb的隔离级别
        + 顺序读
        + 可重复读
        + 读已提交
        + 读未提交
    + Innodb中的锁
        + 查询需要对资源共享锁
        + 数据修改需要对资源加排它锁
        + 阻塞
            + 成因：
                + 由于不同锁之间的兼容关系，造成的一事务需要等待另一个事务释放所占用的资源的现象
            + 处理方法
                + 终止占用资源的事务
                + 优化占用资源事务的SQL，使其尽快释放资源
        + 死锁
            + 成因：
                + 并行执行的多个事务相互之间占有了对方所需要的资源
            

    
#### Mysql知识点
+ MySQL中的SQL执行
    + mysql由三层组成
        + 连接层：客户端和服务器端建立连接，客户端发送sql至服务器端
        + SQL层： 对SQL语句进行查询处理
        + 存储引擎层： 与数据库文件打交道，负责数据的存储和读取 
    + mysql执行流程：SQL语句→缓存查询→解析器→优化器→执行器
        + 查询缓存，server如果在查询缓存中发现了这条SQL语句，就会直接将结果返回给客户端；如果没有，就进入到解析器阶段。
        + 解析器：在解析器中对SQL语句进行语法分析、语义分析。
        + 优化器：在优化器中会确定SQL语句的执行路径，比如是根据全表检索，还是根据索引来检索等
        + 执行器：在执行之前需要判断该用户是否具备权限，如果具备权限就执行SQL查询并返回结果。
    
#### MySQL学习路线
1. [零基础入门](https://coding.imooc.com/class/332.html)
2. [MySQL必知必会](https://time.geekbang.org/column/intro/100073201?tab=catalog)
3. 必知必会
    1. [SQL必知必会](https://time.geekbang.org/column/intro/100073201)
    2. 书籍：MySQL必知必会
4. [Mysql45讲](https://time.geekbang.org/column/intro/100020801)
5. SQL练习网站：
    1. https://www.nowcoder.com/exam/oj?tab=SQL%E7%AF%87&topicId=199
    2. https://leetcode-cn.com/problemset/database/

