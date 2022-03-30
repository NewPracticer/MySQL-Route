# MySQL-Route

#### MySQL学习路线
1. [从零开始MySQL](https://coding.imooc.com/class/332.html)[12小时]
2. 必知必会
    1. [MySQL必知必会](https://time.geekbang.org/column/intro/100073201?tab=catalog)[40小时]
        + 建议所有的SQL语句练习一遍
    2. [SQL必知必会](https://time.geekbang.org/column/intro/100073201)
    3. 书籍：MySQL必知必会
3. [阿里云数据库实战](https://coding.imooc.com/class/353.html)[22小时]
4. [Mysql45讲](https://time.geekbang.org/column/intro/100020801)
5. 高性能Mysql
    1. [Mysql实战进阶](https://coding.imooc.com/class/515.html)
    2. [高性能MySQL](https://item.jd.com/11220393.html)
    3. [深入浅出Mysql](https://item.jd.com/12574719.html)
6. SQL练习网站：
    1. https://www.nowcoder.com/exam/oj?tab=SQL%E7%AF%87&topicId=199
    2. https://leetcode-cn.com/problemset/database/

+  推荐理由
    + 从基础入门到必知必会，从实战再到MySQL45讲，最后落脚到高性能Mysql以及SQL的练习
    + 必知必会推荐第一门，建议跟着所有SQL的语句练习一遍
    + 高性能部分推荐实战进阶和深入浅出，最后是高性能MySql
    + SQL练习是牛客网和leetcode的练习地址

----------------------------以下是笔记--------------------------------------

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
            + 如何发现：
                + set global innodb_print_all_deadlocks = on
            + 处理方法：
                + 数据库自行回滚占用资源少的事务
                + 并发事务按照相同顺序占有资源
#### MySQL必知必会
通过一个实战项目使用SQL语句学习使用

+ 创建
    + -- 创建数据库 CREATE DATABASE demo； 
    + -- 删除数据库 DROP DATABASE demo； 
    + -- 查看数据库 SHOW DATABASES; 
    + -- 创建数据表：CREATE TABLE demo.test ( barcode text, goodsname text, price int );
    + -- 查看表结构 DESCRIBE demo.test; 
    + -- 查看所有表 SHOW TABLES; 
    + -- 添加主键 ALTER TABLE demo.test ADD COLUMN itemnumber int PRIMARY KEY AUTO_INCREMENT;
+ 数据类型
    + 由于实际存储的长度不确定，MySQL 不允许 TEXT 类型的字段做主键。遇到这种情况，你只能采用 CHAR(M)，或者 VARCHAR(M)。
    + -- 修改字段类型语句 ALTER TABLE demo.goodsmaster MODIFY COLUMN price DOUBLE; 
    + -- 计算字段合计函数： SELECT SUM(price) FROM demo.goodsmaster;
    + -- 在一个已经存在的表基础上，创建一个新表 CREATE demo.importheadhist LIKE demo.importhead; 
    + -- 修改表的相关语句 
        + ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型; 
        + ALTER TABLE 表名 ADD COLUMN 字段名 字段类型 FIRST|AFTER 字段名; 
        + ALTER TABLE 表名 MODIFY 字段名 字段类型 FIRST|AFTER 字段名;
+ 数据操作
    + INSERT INTO 表名 [(字段名 [,字段名] ...)] VALUES (值的列表); INSERT INTO 表名 （字段名）
      SELECT 字段名或值
      FROM 表名
      WHERE 条件
      DELETE FROM 表名
      WHERE 条件
      UPDATE 表名
      SET 字段名=值 WHERE 条件
      SELECT *|字段列表
      FROM 数据源
      WHERE 条件
      GROUP BY 字段
      HAVING 条件
      ORDER BY 字段
      LIMIT 起始点，行数
    + INSERT INTO demo.goodsmaster SELECT * FROM demo.goodsmaster1 as a ON DUPLICATE KEY UPDATE barcode = a.barcode,goodsname=a.goodsname;
    + SELECT b.membername,c.goodsname,a.quantity,a.salesvalue,a.transdate -> FROM demo.trans AS a -> JOIN demo.membermaster AS b -> JOIN demo.goodsmaster AS c -> ON (a.cardno = b.cardno AND a.itemnumber=c.itemnumber);
+ 连接查询
    + -- 定义外键约束：
    CREATE TABLE 从表名
    (
    字段 字段类型
    .... CONSTRAINT 外键约束名称
    FOREIGN KEY (字段名) REFERENCES 主表名 (字段名称) );ALTER TABLE 从表名 ADD CONSTRAINT 约束名 FOREIGN KEY 字段名 REFERENCES 主表名 （字段
    + 连接查询
        +  SELECT 字段名 FROM 表名 AS a JOIN 表名 AS b ON (a.字段名称=b.字段名称); 
        +  SELECT 字段名 FROM 表名 AS a LEFT JOIN 表名 AS b ON (a.字段名称=b.字段名称); 
        +  SELECT 字段名 FROM 表名 AS a RIGHT JOIN 表名 AS b ON (a 字段名称=b 字段名称);
+ 条件语句 where 和 having 
    +  如果需要通过连接从关联表中获取需要的数据，WHERE 是先筛选后连接，而 HAVING 是先连接后筛选。
    +  在需要对数据进行分组统计的时候，HAVING 可以完成 WHERE 不能完成的任务。
    +  where 先筛选数据再关联，执行效率高，不能使用分组中的计算函数进行筛选
    +  HAVING 可以使用分组中的计算函数，在最后的结果集中进行筛选，执行效率低
+ 聚合函数 SUM AVG MAX MIN COUNT
+ 时间函数
    + EXTRACT ：获取日期时间中指定的值
    + HOUR:获取小时
    + YEAR: 获取年份
    + MONTH：获取月份
    + DAY ： 获取日
    + MINUTE：获取分钟
    + SECOND： 获取秒
    + DATE_ADD():计算从某个时间点出发过去或未来一段时间间隔的时间
    + DDDATE()：与DATE_ADD()一样
    + DATE_SUB():与DATE_ADD()类似，但是方向相反
    + SUBDATE()： 与DATE_SUB()一致
+ 字符串函数及条件判断函数
    + SUBSTR(s,n) 获取从字符串s的第n个位置开始，到s结尾的子字符串
    + MID(s,n,len) 从字符串s的第n个位置,获取长度为len的子字符串
    + TRIM(s1 from s) 除去字符串s的两端所有的字符串s1
    + LTRIM(s) 除去字符串s左边的所有空格
    + RTRIM(s) 除去字符串s右边的所有空格
    + IFNULL(V1,V2) 如果V1值不为空值，则返回V1,否则返回V2
    + IF(表达式,V1,V2) 如果表达式为真，则返回V1,否则返回V2
+ 索引
    + 索引:CREATE INDEX index_trans on demo.trans(transdate)
    + 组合索引
        + CREATE TABLE 表名(字段 数据类型, ….{ INDEX | KEY } 索引名(字段1，字段2，...) )
        + ALTER TABLE 表名 ADD { INDEX | KEY } 索引名 (字段1，字段2，...);
    + 删除索引
        + DROP INDEX 索引名 ON 表名;
        + ALTER TABLE 表名 DROP PRIMARY KEY；
+ 事务
    + START TRANSACTION 或者 BEGIN （开始事务） 一组DML语句 COMMIT（提交事务） ROLLBACK（事务回滚）
    + 四种隔离级别
        + READ UNCOMMITTED：可以读取事务中还未提交的被更改的数据。
        + READ COMMITTED：只能读取事务中已经提交的被更改的数据。
        + REPEATABLE READ：表示一个事务中，对一个数据读取的值，永远跟第一次读取的值一致，不受其他事务中数据操作的影响。这也是 MySQL 的默认选项。
        + SERIALIZABLE：表示任何一个事务，一旦对某一个数据进行了任何操作，那么，一直到这个事务结束，MySQL 都会把这个数据锁住，禁止其他事务对这个数据进行任何操作。
+ 视图
    + 命令：
        + CREATE [OR REPLACE] VIEW 视图名称 [(字段列表)] AS 查询语句
        + ALTER VIEW 视图名 AS 查询语句;
        + DESCRIBE 视图名；
        + DROP VIEW 视图名；
    + 优点：
        + 视图可以简化查询
        + 视图不保存数据，不占用数据存储的空间
        + 视图具有隔离性。视图相当于在用户和实际的数据之间加了一层。用户不需要直接访问数据表，提高了数据表的安全性，也方便了用户查询
        + 数据架构相对独立，即便实际表结构产生变化，也可以通过修改试图的SQL语句，使用户不受影响
    + 缺点：
        + 数据表结构的变更，需要及时对视图进行维护。
+ 权限管理
    +  CREATE USER 用户名 [IDENTIFIED BY 密码];DROP USER 用户名;
    +  GRANT 权限 ON 表名 TO 用户名;
    +  SHOW GRANTS FOR 用户名;
+ 日志
    +  通用查询日志
        +  通用查询日志记录了所有用户的连接开始时间和截止时间,以及发给mysql数据库服务器的所有SQL指令
        + show VARIABLES LIKE '%general%'
        +  SET GLOBAL general_log = 'ON';
    +  慢查询日志,MySQL 的配置文件“my.ini”
        + slow-query-log=1 -- 表示开启慢查询日志，系统将会对慢查询进行记录。
        + slow_query_log_file="GJTECH-PC-slow.log" -- 表示慢查询日志的名称是"GJTECH-PC-slow
        + long_query_time=10 -- 表示慢查询的标准是查询执行时间超过10秒
    + 错误日志
        + 以在 MySQL 的配置文件“my.ini”中配置，log-error="GJTECH-PC.err"
    + 二进制日志
        + 查看当前正在写入的二进制日志的 SQL 语句,SHOW MASTER STATUS;
        + 查看所有的二进制日志,SHOW BINARY LOGS; 
        + 查看二进制日志中所有数据更新事件  SHOW BINLOG EVENTS IN 二进制文件名;
        + 刷新二进制日志 FLUSH BINARY LOGS;
        + mysqlbinlog 工具进行数据恢复 
            + mysqlbinlog –start-positon=xxx –end-position=yyy 二进制文件名 | mysql -u 用户 -p
        + 删除二进制日志  RESET MASTER;
    + 中继日志
        + 中继日志只在主从服务器架构的从服务器上存在。
    + 回滚日志。回滚日志的作用是进行事务回滚
        + SHOW VARIABLES LIKE '%innodb_max_undo_log_size%';
+ 数据备份、恢复、导入
    + 表备份： mysqldump -u 用户 -p 密码 数据库 > 备份文件
    + 数据库备份: mysqldump -h 服务器 -u 用户 -p 密码 --databases 数据库名称 … > 备份文件名
    + 数据恢复： SOURCE 备份文件名
    +  数据导入： 
        + LOAD DATA INFILE 文件名 INTO TABLE 表名 FIELDS TERMINATED BY 字符LINES TERMINATED BY 字符;
        + SELECT 字段列表 INTO OUTFILE 文件名称 FIELDS TERMINATED BY 字符 LINES TERMINATED BY 字符 FROM 表名;
+ [Mysql数据库规范化的三个范式](https://time.geekbang.org/column/article/367615)
    + 第一范式：数据表中所有字段都是不可拆分的基本数据项
    + 第二范式：在满足第一范式的基础上，数据表中所有非主键字段，必须完全依赖全部主键字段，不能存在部分依赖主键字段的字段。
    + 第三范式：在满足第二范式的基础上，数据表中不能存在可以被其他非主键字段派生出来的字段，或者说，不能存在依赖于非主键字段的字段。
+ [ER模型](https://time.geekbang.org/column/article/369434) 
    + 三要素：实体（可以独立存在）、属性（不可再分）、关系
    + ER模型图转换成数据表的原则
        + 一个实体通常转换成一个数据表
        + 一个多对多的关系，通常也转换成一个数据表
        + 一个 1对1，或者1对多的关系，往往通过表的外键来表达，而不是设计一个新的数据表
        + 属性转换成表的字段 
+ 优化查询
    + EXPLAIN
        + id: 是一个查询序列号
        + table：表示与查询结果相关的表的名称。
        + partiton：表示查询访问的分区
        + key:表示优化器最终决定使用的索引是什么
        + key_len：表示优化器选择的索引字段按字节计算的长度。如果没有使用索引，这个值是空的
        + ref:表示哪个字段或者常量被用来与索引字段比对，以读取表中的记录，如果这个值是func, 就表示用函数的值与索引字段进行比对
        + rows：表示为了得到查询结果，必须扫描多少行记录
        + filtered：表示查询筛选出的记录占全部表记录数的百分比
        + possible_key：表示mysql可以通过哪些索引找到查询的结果记录。如果这里的值是空，就说明没有合适的索引可用，你可以通过查看where条件语句中使用的字段，来决定是否可以通过创建索引提高查询的效率
        + Extra:表示mysql执行查询中的附加信息
        + Type：表示表是如何连接诶的。
        + select_type:
            + SIMPLE：表示简单查询，不包含子查询和联合查询
            + PRIMARY：表示是最外层的查询
            + UNION：表示联合查询中的第二个或者之后的查询
            + DEPENGENTUNION：表示联合查询中的第二个或者之后的查询,而且这个查询受外查询的影响
    + 使用关键字LIKE
        +  通配符在前面的筛选条件是不能用索引的。
        +  通配符在后面的筛选条件就可以使用索引。
    + 使用关键字OR 
        +  只有当条件语句中只有关键字OR，并且OR前后的表达式中的字段都建有索引的时候，查询才能用到索引
+ 优化表
    + 对整数类型数据进行优化
    + 既可以使用文本类型也可以使用整数类型的字段，使用整数类型，而不要用文本类型
    + 合理增加冗余字段以提高效率
    + 拆分表把一个包含很多字段的表拆分成2个或者多个相对较小的表
    + 使用非空约束
+ 优化参数
    + 参数：InnoDB_flush_log_at_trx_commit
        + 默认的值是 1
            + 每次提交事务的时候，都把数据写入日志，并把日志写入磁盘。
            + 数据安全性最佳，不足之处在于每次提交事务，都要进行磁盘写入的操作。
        + 当值为0时：表示每隔 1 秒将数据写入日志，并将日志写入磁盘
        + 当值为2时，每次提交事务的时候都将数据写入日志，但是日志每间隔1秒写入磁盘
    + 参数：InnoDB_buffer_pool_size 存储引擎使用缓存来存储索引和数据
    + 参数：InnoDB_buffer_pool_instances   InnoDB 的缓存区分成几个部分
    + 使用Performance Schema诊断系统资源问题
+ Mysql空间函数
    +  单个值的几何类型（GEOMETRY）、点类型（POINT）、线类型（LINESTRINIG）和多边形类型（POLYGON）
    +  包含多个值的多点类型（MULTIPOINT）、多线类型（MULTILINESTRING）、多多边形类型（MULTIPOLYGON）和几何集类型（GEOMETRYCOLLECTION）
    +  ST_Distance_Sphere() 函数
    +  MBRContains() 和 MBRWithin()    

#### 阿里新零售数据库实战
+ 前置知识
    + touch 命令用来创建文件 touch demo.txt
    + vi命令用来编辑文件内容  vi demo.txt
        + i 编辑。esc 退出。:w 保存  :q退出
    + mkdir 命令用来创建文件夹
    + rm 命令可以用来删除文件或者文件夹
    + ps 命令用于查询运行中的进程
    + kill 用来杀死进程
+ ER图
    + 实体关系图，提供了表示实体类型，属性和关系的方法，用来描述现实世界的概念模型
+ CRUD 部分    
    + 忽略主键重复
        + INSERT IGNORE INTO t_dept(deptno,dname,loc)VALUES(40,'企划部','北京'),(50,'培训部','上海'),(60,'后勤部','北京'),(70,'技术部','北京'),(80,'市场部','北京');
    + 不存在就插入，存在就更新
        + INSERT INTO t_emp_ip(id,empno,ip)VALUES(5,8004,'192.168.99.44'),(6,8005,'192.168.99.45'),(7,8006,'192.168.99.46'),(8,8001,'192.168.99.47')ON DUPLICATE KEY UPDATE ip = VALUES(ip)
    + 要不要使用子查询
        + Mysql数据库默认关闭了缓存，所以每个子查询都是相关子查询
        + 相关子查询就是要循环执行多次的子查询
        + MyBatis等持久层框架开启了缓存功能，其中一级缓存就会保存子查询的结果,所以可以放心使用子查询。
        + FROM子查询只会执行一次，所以不是相关子查询
    + 外连接的JOIN条件
        + 内连接里，查询条件写在ON子句或者WHERE子句，效果相同
        + 外连接里，查询条件写在ON子句或者WHERE子句，效果不同
    + 表连接修改
        + UPDATE语句中的WHERE子查询如何改成表连接
            + UPDATE t_emp set sal = 1000 where deptno = (select deptno from t_dept where dname = 'sales')
            + UPDATE t_emp e join t_dept d on e.deptno = d.deptno and 
            d.dname = 'sales' set e.sal = 10000,d.dname = '销售部'
        + DELETE子句的修改
            + DELETE e,d from t_emp e join t_dept d on e.deptno = d.deptno and d.dname = '销售部'
+ 事务机制
    + redo日志和undo日志与事务有关
        + mysql拷贝数据到undo日志，记录修改到redo日志，redo日志同步数据到mysql
    + 事务是一个或者多个sql语句组成的整体，要么全部执行成功，要么全都执行失败。
    + 事务的四个特性
        + 原子性：一个事务中所有的操作要么全部完成，要么全部失败。
        + 一致性：不管在任何给定的时间内，并发事务有多少，事务必须保证运行结果的一致性
        + 隔离性： 事务不受其他并发事务的影响，默认情况下A事务，只能看到日志中该事务的相关数据
        + 持久性： 事务一旦提交，结果便是永久性的，即便发生宕机，依然可以依靠事务日志完成数据的持久化。
+ SKU、SPU
    + 概念
        + SPU Standard Product Unit（标准产品单位）SPU是商品信息聚合的最小单位，是一组可复用、易检索的标准化信息的集合，该集合描述了一个产品的特性。
        + SKU 是库存进出剂量的单位，SKU是物理上不可分割的最小存货单元
    + 设计表
        + 一个品类表对应N个参数表，一个品类表对应一个产品表，一个产品表对应N个商品表

        
    




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
    

 


