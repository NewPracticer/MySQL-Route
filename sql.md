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
