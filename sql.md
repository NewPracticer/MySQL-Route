### SQL 练习

#### 牛客网
+ [SQL9 查找除复旦大学的用户信息](https://www.nowcoder.com/practice/c12a056497404d1ea782308a7b821f9c?tpId=199&tqId=1971604&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D199)
    + select device_id,gender,age,university from user_profile where university NOT IN ('复旦大学')
    + select device_id, gender, age, university from user_profile where university != '复旦大学'
+ [SQL10 用where过滤空值练习](https://www.nowcoder.com/practice/08c9846a423540319eea4be44e339e35?tpId=199&tqId=1971605&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D199)
    + select device_id,gender,age,university from user_profile where age IS NOT NULL  
+ [SQL14 操作符混合运用](https://www.nowcoder.com/practice/d5ac4c878b63477fa5e5dfcb427d9102?tpId=199&tqId=1975666&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D199)
    + select device_id,gender,age,university,gpa from user_profile where (gpa > 3.5 and university = '山东大学') OR ( gpa > 3.8 and university = '复旦大学')
+ [SQL19 分组过滤练习题](https://www.nowcoder.com/practice/ddbcedcd9600403296038ee44a172f2d?tpId=199&tqId=1975671&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D199)
    + 问题分解：
        + 限定条件：平均发贴数低于5或平均回帖数小于20的学校，avg(question_cnt)<5 or avg(answer_cnt)<20，聚合函数结果作为筛选条件时，不能用where，而是用having语法，配合重命名即可；
        + 按学校输出：需要对每个学校统计其平均发贴数和平均回帖数，因此group by university
    + select university, avg(question_cnt) as avg_question_cnt,avg(answer_cnt) as avg_answer_cntfrom user_profile group by university having avg_question_cnt<5 or avg_answer_cnt<20
+ [SQL21 浙江大学用户题目回答情况](https://www.nowcoder.com/practice/55f3d94c3f4d47b69833b335867c06c1?tpId=199&tqId=1975673&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D199)
    + 问题分解：
        +  限定条件：来自浙江大学的用户，学校    信息在用户画像表，答题情况在用户练习明细表，因此需要通过device_id关联两个表的数据；
        +  方法1：join两个表，用inner join，条件是on up.device_id=qpd.device_id and up.university='浙江大学'
        +  方法2：先从画像表找到浙江大学的所有学生id列表where university='浙江大学'，再去练习明细表筛选出id在这个列表的记录，用where in
    + select qpd.device_id, qpd.question_id, qpd.result from question_practice_detail as qpd inner join user_profile as up on up.device_id=qpd.device_id and up.university='浙江大学'
    + select device_id, question_id, result from question_practice_detail where device_id in ( select device_id from user_profile  where university='浙江大学')
+ [SQL22 统计每个学校的答过题的用户的平均答题数](https://www.nowcoder.com/practice/88aa923a9a674253b861a8fa56bac8e5?tpId=199&tqId=1975674&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解：
        + 限定条件：无；
        + 每个学校：按学校分组，group by university
        + 平均答题数量：在每个学校的分组内，用总答题数量除以总人数即可得到平均答题数量count(question_id) / count(distinct device_id)。
        + 表连接：学校和答题信息在不同的表，需要做连接
    + select university, COUNT(question_id)/ COUNT(distinct qpd.device_id) as avg_answer_cnt from question_practice_detail as qpd inner join user_profile as up  on qpd.device_id = up.device_id group by university
+ [SQL23  统计每个学校各难度的用户平均刷题数](https://www.nowcoder.com/practice/5400df085a034f88b2e17941ab338ee8?tpId=199&tqId=1975675&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：无；
        + 每个学校：按学校分组group by university
        + 不同难度：按难度分组group by difficult_level
        + 平均答题数：总答题数除以总人数count(qpd.question_id) / count(distinct qpd.device_id)
        + 来自上面信息三个表，需要联表，up与qpd用device_id连接，qd与qpd用question_id连接。
    + select  university,difficult_level,round(count(qpd.question_id) / count(distinct qpd.device_id), 4) as avg_answer_cntfrom question_practice_detail as qpd left join user_profile as up on up.device_id=qpd.device_id left join question_detail as qd on qd.question_id=qpd.question_id group by university, difficult_level
+ [SQL24 统计每个用户的平均刷题数](https://www.nowcoder.com/practice/f4714f7529404679b7f8909c96299ac4?tpId=199&tqId=1975676&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：山东大学的用户 up.university="山东大学"；
        + 不同难度：按难度分组group by difficult_level
        + 平均答题数：总答题数除以总人数count(qpd.question_id) / count(distinct qpd.device_id) 来自上面信息三个表，需要联表，up与qpd用device_id连接并限定大学，qd与qpd用question_id连接。
    + select university,difficult_level,count(qpd.question_id) / count(distinct qpd.device_id) as avg_answer_cnt from question_practice_detail as qpd inner join user_profile as up on up.device_id=qpd.device_id and up.university="山东大学" inner join question_detail as qd on qd.question_id=qpd.question_id group by difficult_level
+ [SQL25 查找山东大学或者性别为男生的信息](https://www.nowcoder.com/practice/979b1a5a16d44afaba5191b22152f64a?tpId=199&tqId=1975677&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：学校为山东大学或者性别为男性的用户：university='山东大学', gender='male'；
        + 分别查看&结果不去重：所以直接使用两个条件的or是不行的，直接用union也不行，要用union all，分别去查满足条件1的和满足条件2的，然后合在一起不去重
    + select device_id, gender, age, gpa from user_profile where university='山东大学' union all select device_id, gender, age, gpa from user_profile where gender='male'
+ [SQL26 计算25岁以上和以下的用户数量](https://www.nowcoder.com/practice/30f9f470390a4a8a8dd3b8e1f8c7a9fa?tpId=199&tqId=1975678&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + SELECT CASE WHEN age < 25 OR age IS NULL THEN '25岁以下'  WHEN age >= 25 THEN '25岁及以上' END age_cut,COUNT(*)number FROM user_profile GROUP BY age_cut
+ [SQL28 计算用户8月每天的练题数量](https://www.nowcoder.com/practice/847373e2fe8d47b4a2c294bdb5bda8b6?tpId=199&tqId=1975680&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：2021年8月，写法有很多种，比如用year/month函数的year(date)=2021 and month(date)=8，比如用date_format函数的date_format(date, "%Y-%m")="202108"
        + 每天：按天分组group by date
        + 题目数量：count(question_id)
    + select day(date) as day, count(question_id) as question_cnt from question_practice_detail where month(date)=8 and year(date)=2021 group by date
+ [SQL29 计算用户的平均次日留存率](https://www.nowcoder.com/practice/126083961ae0415fbde061d7ebbde453?tpId=199&tqId=1975681&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分级
        + 所谓次日留存，指的是同一用户（在本题中则为同一设备，即device_id）在当天和第二天都进行刷题。注意，在这题我们不关心同一用户（设备）在这天答了什么题、答题结果如何，只关心他是否答题，因此对于这题来说存在重复的数据（如下图红框所示），需要使用 DISTINCT 去重。
        + 使用两个子查询，查询出两个去重的数据表，并使用条件（q2.date应该是q1.date的后一天）进行筛选，如下所示（数据未显示完全，从左至右顺序，列表名为 q1.device_id, q1.date, q2.device_id, q2.date)
    + SELECT  COUNT(q2.device_id) / COUNT(q1.device_id) AS avg_ret  FROM (SELECT DISTINCT device_id, date FROM question_practice_detail)as q1 LEFT JOIN (SELECT DISTINCT device_id, date FROM question_practice_detail) AS q2 ON q1.device_id = q2.device_id AND q2.date = DATE_ADD(q1.date, interval 1 day)
+ [SQL30 统计每种性别的人数](https://www.nowcoder.com/practice/f04189f92f8d4f6fa0f383d413af7cb8?tpId=199&tqId=1975682&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + select case when profile like '%,male' then 'male'  when profile like '%,female' then 'female'  end gender,count(*) number from user_submit group by gender;
+ [SQL31 提取博客URL中的用户名](https://www.nowcoder.com/practice/26c8715f32e24d918f15db69518f3ad8?tpId=199&tqId=1975683&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + select 
      -- 替换法 replace(string, '被替换部分','替换后的结果')
      -- device_id, replace(blog_url,'http:/url/','') as user_name
      
      -- 截取法 substr(string, start_point, length*可选参数*)
      -- device_id, substr(blog_url,11,length(blog_url)-10) as user_nam
      
      -- 删除法 trim('被删除字段' from 列名)
      -- device_id, trim('http:/url/' from blog_url) as user_name
      
      -- 字段切割法 substring_index(string, '切割标志', 位置数（负号：从后面开始）)
      device_id, substring_index(blog_url,'/',-1) as user_name
      
      from user_submit;
+ [SQL32 截取出年龄](https://www.nowcoder.com/practice/b8d8a87fe1fc415c96f355dc62bdd12f?tpId=199&tqId=1975684&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：无；
        + 每个年龄：按年龄分组group by age，但是没有age字段，需要从profile字段截取，按字符,分割后取出即可。可使用substring_index函数可以按特定字符串截取源字符串。
        substring_index(FIELD, sep, n)可以将字段FIELD按照sep分隔：
            + (1).当n大于0时取第n个分隔符(n从1开始)之后的全部内容；
            + (2).当n小于0时取倒数第n个分隔符(n从-1开始)之前的全部内容；
        因此，本题可以先用substring_index(profile, ',', 3)取出"180cm,75kg,27"，然后用substring_index(profile, ',', -1)取出27。
        当然也可以用substring_index(substring_index(profile, ",", -2), ",", 1)取出27。
        附：substring_index
        + 多少参赛者：计数统计，count(device_id)
    + select substring_index(substring_index(profile, ',', 3), ',', -1) as age, count(device_id) as number from user_submit group by age 
+ [SQL33 找出每个学校GPA最低的同学](https://www.nowcoder.com/practice/90778f5ab7d64d35a40dc1095ff79065?tpId=199&tqId=1980672&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：gpa最低，看似min(gpa)，但是要留意，是每个学校里的最低，不是全局最低。min(gpa)的时候对应同学的ID丢了，直接干是拿不到最低gpa对应的同学ID的；
        + 每个学校最低： 第一种方式是用group by把学校分组，然后计算得到每个学校最低gpa，再去找这个学校里和这个gpa相等的同学ID。注意这样如果最低gpa对应多个同学，都会输出，题目没有明确此种情况，心理明白就行。 第二种方式是利用窗口函数，先按学校分组计算排序gpa，得到最低gpa的记录在用子查询语法拿到需要的列即可。此题中rou_number可以得到排序后的位序，取位序为1即可得到最小值（升序时）。
        + 窗口函数语法：row_number/rank/dense_rank over (partition by FIELD1 order by FIELD2)
    + select a.device_id, a.university, a.gpa
      from user_profile a
      right join
      (
          select university, min(gpa) as gpa
          from user_profile
          group by university
      ) as b
      on a.university=b.university and a.gpa=b.gpa
      order by a.university
+ [SQL34 统计复旦用户8月练题情况](https://www.nowcoder.com/practice/53235096538a456b9220fce120c062b3?tpId=199&tqId=1980673&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：需要是复旦大学的（来自表user_profile.university），8月份练习情况（来自表question_practice_detail.date）
        + 从date中取month：用month函数即可；
        + 总题目：count(question_id)
        + 正确的题目数：sum(if(qpd.result='right', 1, 0))
        + 按列聚合：需要输出每个用户的统计结果，因此加上group by up.device_id
    + select up.device_id, '复旦大学',
          count(question_id) as question_cnt,
          sum(if(qpd.result='right', 1, 0)) as right_question_cnt
      from user_profile as up
      
      left join question_practice_detail as qpd
        on qpd.device_id = up.device_id and month(qpd.date) = 8
      
      where up.university = '复旦大学'
      group by up.device_id\
+ [SQL35 浙大不同难度题目的正确率](https://www.nowcoder.com/practice/d8a4f7b1ded04948b5435a45f03ead8c?tpId=199&tqId=1980674&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：浙江大学的用户；
          不同难度：difficult_level（question_detail表中的列），需要分组统计，因此用到group by，；
          正确率：表面理解就是正确数÷总数，正确的是result='right'（question_practice_detail表），数目用函数count，总数是count(question_id)；
          多张表联合查询：需要用到join，join有多种语法，因为条件限定需要是浙江大学的用户，所以需要是user_profile表的并且能统计出题目难度的记录，因此用user_profile表inner join另外两张表。
    + select difficult_level,
         avg(if(qpd.result='right', 1, 0)) as correct_rate
     #    sum(if(qpd.result='right', 1, 0)) / count(qpd.question_id) as correct_rate
     #    count(if(qpd.result='right', 1, null)) / count(qpd.question_id) as correct_rate
     from user_profile as up
     
     inner join question_practice_detail as qpd
         on up.device_id = qpd.device_id
     
     inner join question_detail as qd
         on qd.question_id = qpd.question_id
     
     where up.university = '浙江大学'
     group by qd.difficult_level
     order by correct_rate asc;
+ [SQL39 21年8月份练题总数](https://www.nowcoder.com/practice/b8f30b239b454ed490367b53ea95607d?tpId=199&tqId=2002640&ru=/exam/oj&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Foj)
    + 问题分解
        + 限定条件：2021年8月份，匹配date字段即可，匹配方法主要有三种：
          （1）like语法：date like "2021-08%"
          （2）year、month函数：year(date)='2021' and month(date)='08'；
          （3）date_format函数：date_format(date, '%Y-%m')='2021-08';
        + 2：总用户数：count函数计数，因为用户有重复，所以需要distinct去重，即count(distinct device_id)
        + 3：总次数：count(question_id)即可
    + select count(distinct device_id) as did_cnt, count(question_id) as question_cnt from question_practice_detail where date like "2021-08%"


