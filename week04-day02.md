### 子查询（嵌套查询）
---
子查询就是在一个SQL语句内部，在嵌套另一个完整的SELECT语句。

* 内层的select称为子查询
* 外层的select（或 INSERT/UPDATE/DELETE）称为主查询

执行逻辑（非相关子查询）：先执行子查询，将结果作为主查询的条件或数据源。

#### IN/NOT IN
---
IN/NOT IN 用于判断一个值是否属于子查询返回的结果集（一列多行）

```sql
select 列名 from 表名 where 列名 in (子查询);  -- 列值等于子查询中任一值
```

* 如果子查询结果中包含 NULL，NOT IN 会返回 空结果（因为 x NOT IN (1, 2, NULL) 永远为 FALSE/UNKNOWN）。
安全改写：使用 NOT EXISTS 代替（见后文）。

#### 单行子查询、多行子查询
----
按子查询返回的行数分类：

|类型|返回行数|返回值|常用操作符|示例|
|----|----|----|----|----|
|单行子查询|1行|一个值|=, >, <, >=, <=, <>|WHERE salary > (SELECT AVG(salary) ...)|
|多行子查询|多行|一列值|IN, NOT IN, ANY, ALL, EXISTS|WHERE id IN (SELECT ...)|

单行子查询示例：
```sql
SELECT * FROM student 
WHERE age = (SELECT MIN(age) FROM student);
```

EXISTS 运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回 True，否则返回 False。

```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```

NOT EXISTS


用于检查子查询是否没有返回任何行的条件运算符。

```sql
SELECT columns
FROM table1 t1
WHERE NOT EXISTS (
    SELECT 1
    FROM table2 t2
    WHERE t2.key = t1.key
        AND (其他条件)
);
```

对 t1 中的每一行，执行子查询。

如果子查询 返回至少一行，则 NOT EXISTS 为 FALSE，该行被排除。

如果子查询 返回空集，则 NOT EXISTS 为 TRUE，该行被选中。

SELECT 1 只是习惯写法，实际不关心选什么列，只关心有没有行返回。

CASE

不同条件下返回不同的数值。

如果没有匹配条件（且没有其他条件），则返回NULL。

```sql
CASE WHEN condition1 THEN value1 
       WHEN condition2 THEN value2  
       ELSE def_value 
  END 
```

例

```sql
SELECT name, population
      ,CASE WHEN population<1000000 
            THEN 'small'
            WHEN population<10000000 
            THEN 'medium'
            ELSE 'large'
       END
  FROM bbc
```