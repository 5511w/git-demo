### mysql连接的使用
---
 JOIN 在两个或多个表中查询数据。JOIN 按照功能大致分为如下三类：
* INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。
* LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
* RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

#### inner join

INNER JOIN 返回两个表中满足连接条件的匹配行

```sql
SELECT column1, column2, ...
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```
* table1.column_name = table2.column_name 是连接条件，指定了两个表中用于匹配的列。

```sql
-- 查询所有有成绩的学生姓名、科目、分数
SELECT student.name, score.subject, score.score
FROM student
INNER JOIN score ON student.id = score.student_id;
```

#### left join

返回左表（LEFT JOIN 左边的表）的所有行，右表有匹配则显示，没有匹配则用 NULL 填充。

```sql
ELECT 列名
FROM 表A
LEFT JOIN 表B ON 表A.公共字段 = 表B.公共字段;
```

```sql
-- 查询所有学生及其成绩（包含无成绩的学生）
SELECT student.name, score.subject, score.score
FROM student
LEFT JOIN score ON student.id = score.student_id;
```

内连接：只留两边都有的
左连接：左表全留，右表有就匹配、没有就空着