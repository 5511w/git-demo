 #### GROUP BY 语句
---
 根据一个或多个列对结果集进行分组

 ```sql
 SELECT column1, aggregate_function(column2)
FROM table_name
WHERE condition
GROUP BY column1
HAVING ....
;
```

* column1：指定分组的列。
* aggregate_function(column2)：对分组后的每个组执行的聚合函数。
* table_name：要查询的表名。
* condition：可选，用于筛选结果的条件。
* HAVING:可选，分组条件

实例：
```sql
SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
--  Table structure for `employee_tbl`
-- ----------------------------
DROP TABLE IF EXISTS `employee_tbl`;
CREATE TABLE `employee_tbl` (
  `id` INT(11) NOT NULL,
  `name` CHAR(10) NOT NULL DEFAULT '',
  `date` datetime NOT NULL,
  `signin` tinyint(4) NOT NULL DEFAULT '0' COMMENT '登录次数',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
--  Records of `employee_tbl`
-- ----------------------------
BEGIN;
INSERT INTO `employee_tbl` VALUES ('1', '小明', '2016-04-22 15:25:33', '1'), ('2', '小王', '2016-04-20 15:25:47', '3'), ('3', '小丽', '2016-04-19 15:26:02', '2'), ('4', '小王', '2016-04-07 15:26:14', '4'), ('5', '小明', '2016-04-11 15:26:40', '4'), ('6', '小明', '2016-04-04 15:26:54', '2');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;
```

导入成功后，执行以下 SQL 语句：

```sql
mysql> set names utf8;
mysql> SELECT * FROM employee_tbl;
+----+--------+---------------------+--------+
| id | name   | date                | signin |
+----+--------+---------------------+--------+
|  1 | 小明 | 2016-04-22 15:25:33 |      1 |
|  2 | 小王 | 2016-04-20 15:25:47 |      3 |
|  3 | 小丽 | 2016-04-19 15:26:02 |      2 |
|  4 | 小王 | 2016-04-07 15:26:14 |      4 |
|  5 | 小明 | 2016-04-11 15:26:40 |      4 |
|  6 | 小明 | 2016-04-04 15:26:54 |      2 |
+----+--------+---------------------+--------+
6 rows in set (0.00 sec)
```

将数据表按名字进行分组，并统计每个人有多少条记录：

```sql
mysql> SELECT name, COUNT(*) FROM   employee_tbl GROUP BY name;
+--------+----------+
| name   | COUNT(*) |
+--------+----------+
| 小丽 |        1 |
| 小明 |        3 |
| 小王 |        2 |
+--------+----------+
3 rows in set (0.01 sec)
```

WITH ROLLUP 

* 在多级分组（例如 GROUP BY year, month）时，会逐级产生小计和总计。
* 这里只按一个字段分组，所以只产生一个总计行（相当于所有行的 SUM(signin)）。

例：

```sql
mysql> SELECT name, SUM(signin) as signin_count FROM  employee_tbl GROUP BY name WITH ROLLUP;
+--------+--------------+
| name   | signin_count |
+--------+--------------+
| 小丽 |            2 |
| 小明 |            7 |
| 小王 |            7 |
| NULL   |           16 |
+--------+--------------+
4 rows in set (0.00 sec)
```

可以使用 coalesce 来设置一个可以取代 NUll 的名称

```sql
select coalesce(a,b,c);
```

参数说明：如果 a==null，则选择 b；如果 b==null,则选择 c；如果 a!=null,则选择 a；如果 a b c 都为 null ，则返回为 null（没意义）。

如果名字为空我们使用总数代替：

```sql
mysql> SELECT coalesce(name, '总数'), SUM(signin) as signin_count FROM  employee_tbl GROUP BY name WITH ROLLUP;
+--------------------------+--------------+
| coalesce(name, '总数') | signin_count |
+--------------------------+--------------+
| 小丽                   |            2 |
| 小明                   |            7 |
| 小王                   |            7 |
| 总数                   |           16 |
+--------------------------+--------------+
4 rows in set (0.01 sec)
```

MySQL 中 COALESCE() 和 IFNULL() 函数的区别
COALESCE() 和 IFNULL() 都是 MySQL 中用于处理 NULL 值的函数，但它们有一些重要区别：
* IFNULL(expr1, expr2) - 只接受2个参数
* COALESCE(expr1, expr2, ..., exprN) - 可以接受多个参数

功能：

* IFNULL() - 如果第一个表达式为 NULL，则返回第二个表达式
* COALESCE() - 返回参数列表中第一个非 NULL 的值

兼容性：

* COALESCE() 是 SQL 标准函数，大多数数据库都支持
* IFNULL() 是 MySQL 特有的函数


练习：

1. 统计总人数

```sql
select count(*) as 总人数 from student;
```

2. 统计平均年龄、最大年龄、最小年龄

```sql
select avg(age) 平均年龄,max(age) 最大年龄,min(age) 最小年龄 from student;
```

3. 按年龄分组，统计每个年龄有多少人

```sql
select age,count(*) 人数
from student group by age;
```

4. 分组后筛选：只显示人数大于等于2的年龄组

```sql 
select age,count(*) 人数
from student group by age having count(*) >= 2;
```